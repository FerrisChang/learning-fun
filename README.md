import { Component, OnInit } from '@angular/core';
import { CommonModule, Location } from '@angular/common';
import { ActivatedRoute } from '@angular/router';
import { forkJoin, map, switchMap, tap } from 'rxjs';
import { Workflow } from 'src/app/model/workflow.model';
import { TraceVocabService } from 'src/app/services/trace-vocab/trace-vocab.service';
import { DiagramElementEvent } from './workflow-diagram/workflow-diagram-element/workflow-diagram-element.component';
import { VerifiableCredentialWrapper } from 'src/app/model/wrappers/verifiable-credential-wrapper';
import { GoogleGeocoderService } from 'src/app/services/google-geocoder/google-geocoder.service';
import {
  MapViewComponent,
  Marker,
} from '../../util/map-view/map-view.component';
import { VerifiableCredentialType } from 'src/app/types/verifiable-credential-type';
import {
  VcTabId,
  VcTabsComponent,
  VcTypeDesc,
} from './vc-tabs/vc-tabs.component';
import { LatLngLiteral } from 'leaflet';
import { HttpClient } from '@angular/common/http';
import { WorkflowInformationPaneComponent } from './workflow-information-pane/workflow-information-pane.component';
import { MatIconModule } from '@angular/material/icon';
import { WorkflowDiagramComponent } from './workflow-diagram/workflow-diagram.component';
import { MatProgressSpinnerModule } from '@angular/material/progress-spinner';
import {
  ActionButton,
  ActionButtonsComponent,
} from './action-buttons/action-buttons.component';
import { SwitchComponent } from '../../util/switch/switch.component';
import { EventsComponent } from './vc-details/events/events.component';
import { setParentWidth } from 'src/app/util/set-parent-width';
import { workflowSchema } from './workflow-schema';
import { LocalStateService } from 'src/app/services/local-state/local-state.service';
import { v4 as uuidv4 } from 'uuid';
import {
  MatExpansionPanel,
  MatExpansionModule,
} from '@angular/material/expansion';
import { VpAuditDialogComponent } from '../../modals/vp-audit/vp-audit-dialog/vp-audit-dialog.component';
import { MatDialog, MatDialogRef } from '@angular/material/dialog';
import {
  holdWorkflowButton,
  refreshWorkflowButton,
  rejectWorkflowButton,
  releaseWorkflowButton,
  vpAuditWorkflowButton,
} from './action-buttons/action-buttons.models';

export const cargoFlowMapTabId: 'cargo-flow-map' = 'cargo-flow-map';
export const eventsTabId: 'events' = 'events';

export function makeEpcisKey(event: any) {
  return `${event.epcis?.vcObjId}`;
}

@Component({
  selector: 'app-workflow-details',
  templateUrl: './workflow-details.component.html',
  styleUrls: ['./workflow-details.component.scss'],
  standalone: true,
  imports: [
    WorkflowInformationPaneComponent,
    EventsComponent,
    WorkflowDiagramComponent,
    ActionButtonsComponent,
    VcTabsComponent,
    MapViewComponent,
    SwitchComponent,
    MatExpansionPanel,
    MatExpansionModule,
    MatIconModule,
    MatProgressSpinnerModule,
    CommonModule,
    MatExpansionModule,
  ],
})
export class WorkflowDetailsComponent implements OnInit {
  workflowId!: number;
  entryNumber!: string;

  workflow!: Workflow;
  vcList: VerifiableCredentialWrapper[] = [];
  selectedTabId: any = '';
  selectedVcWrapper!: VerifiableCredentialWrapper;
  tabsDefaultId!: VcTabId;
  vcWrappersInitialized = false;
  markers: Marker[] = [];
  highlightPosition!: google.maps.LatLngLiteral;
  selectedSchemaType!: VerifiableCredentialType;
  selectSchemaType!: VerifiableCredentialType;
  selectEventOrder = 0;
  cargoFlowMapTabId = cargoFlowMapTabId;
  eventsTabId = eventsTabId;
  nullLatLng: LatLngLiteral = { lat: 0, lng: 0 };
  vcTypesDesc: VcTypeDesc[] = [];
  hasEvents = false;
  error = '';
  epcisEvents!: any[];
  issuerMarker: Marker | null = null;
  modal!: MatDialogRef<VpAuditDialogComponent>;
  // there are the action button rows
  actionButtonRow1: ActionButton[] = [
    refreshWorkflowButton,
    vpAuditWorkflowButton,
  ];
  actionButtonRow2: ActionButton[] = [
    releaseWorkflowButton,
    holdWorkflowButton,
    rejectWorkflowButton,
  ];
  constructor(
    private route: ActivatedRoute,
    private location: Location,
    private traceVocabService: TraceVocabService,
    private googleGeocoderService: GoogleGeocoderService,
    private httpClient: HttpClient,
    private localStateService: LocalStateService,
    private modalOpener: MatDialog
  ) {}

  resetLatLng() {
    this.highlightPosition = this.nullLatLng;
  }
  ngOnInit() {
    this.route.params
      .pipe(
        tap((params: any) => {
          // loads in the parameters
          this.workflowId = params.workflowId;
          this.entryNumber = params.entryNumber;
        }),
        switchMap((params) => {
          // based on parameters, it will graph the workflow accordingly
          return forkJoin([this.loadWorkflow()]);
        })
      )
      .subscribe({
        next: async ([workflow]) => {
          if (workflow.workflowId == null) {
            console.error(
              `Cannot ${this.workflowId ? 'Workflow ID' : 'Entry Number'}: ${
                this.workflowId || this.entryNumber
              }`
            );
            this.error = `${
              this.workflowId ? 'Workflow Object ID' : 'Entry Number'
            } is not valid: ${this.workflowId || this.entryNumber}`;
          } else {
            console.log('Workflow itself: ', workflow);
            this.workflow = { ...workflow };
            await this.loadVcWrappers();
            await this.loadMarkers();
            this.tabsDefaultId = cargoFlowMapTabId;
          }
        },
        error: (error) => {
          console.error(
            `Invalid ${this.workflowId ? 'Workflow ID' : 'Entry Number'}: ${
              this.workflowId || this.entryNumber
            }\nError: ${error}`
          );
          this.error = `${
            this.workflowId ? 'Workflow Object ID' : 'Entry Number'
          } is not valid: ${this.workflowId || this.entryNumber}`;
        },
      });

    setTimeout(() => {
      setParentWidth('workflow-details');
    }, 1000);
  }
  openVpModal() {
    this.modal = this.modalOpener.open(VpAuditDialogComponent, {
      height: '80%',
      width: '75%',
      data: { workflowObjId: this.workflow.workflowObjId },
    });
    this.modal.afterClosed().subscribe((result) => {
      console.log(`Dialog result: ${result}`);
    });
  }
  // handles the button actions in the workfow action panel
  handleWorkflowAction(action: string) {
    console.log('Action Expressed: ', action);
    switch (action) {
      case 'openVpAuditModal':
        this.openVpModal();
        break;
      /*
      Handle release, hold, reject buttons later
      */
      case 'release':
        break;
      case 'hold':
        break;
      case 'reject':
        break;
      case 'refresh':
        this.loadWorkflow().subscribe({
          next: async (workflow) => {
            if (workflow.workflowId == null) {
              console.error(
                `Cannot ${this.workflowId ? 'Workflow ID' : 'Entry Number'}: ${
                  this.workflowId || this.entryNumber
                }`
              );
              this.error = `${
                this.workflowId ? 'Workflow Object ID' : 'Entry Number'
              } is not valid: ${this.workflowId || this.entryNumber}`;
            } else {
              console.log('Workflow itself: ', workflow);
              this.workflow = { ...workflow };
              await this.loadVcWrappers();
              await this.loadMarkers();
              this.tabsDefaultId = cargoFlowMapTabId;
            }
          },
          error: (error) => {
            console.error(
              `Invalid ${this.workflowId ? 'Workflow ID' : 'Entry Number'}: ${
                this.workflowId || this.entryNumber
              }\nError: ${error}`
            );
            this.error = `${
              this.workflowId ? 'Workflow Object ID' : 'Entry Number'
            } is not valid: ${this.workflowId || this.entryNumber}`;
          },
        });
        break;
    }
  }
  async loadVcWrappers() {
    this.vcList = [];
    this.vcWrappersInitialized = false;

    const vcWrappersInit: Promise<void>[] = [];

    this.epcisEvents = [];

    for (const vcType of this.workflow.vcTypes) {
      // for the case of EPCIS Event VCs we need to clone a VC for each entry in the events array
      if (vcType === VerifiableCredentialType.EPCISEventCredential) {
        // get the parent VC
        const parentVcWrapper = new VerifiableCredentialWrapper(
          this.workflow.workflowObjId,
          vcType,
          this.googleGeocoderService,
          this.httpClient
        );
        await parentVcWrapper.init();
        // enumerate the events
        for (const event of parentVcWrapper.getDetails().events ?? []) {
          const vcWrapper = await VerifiableCredentialWrapper.clone(
            this.workflow.workflowObjId,
            event.epcis?.busStep,
            {
              ...event,
            },
            event.vc,
            makeEpcisKey(event),
            this.googleGeocoderService,
            this.httpClient
          );

          this.vcList.push(vcWrapper);

          vcWrappersInit.push(vcWrapper.init());
        }

        // display the events on the map
        const epcisEvents = parentVcWrapper
          .getDetails()
          .events?.filter(
            (event: any) =>
              event.epcis?.busStep ===
                VerifiableCredentialType.EPCISEventTransferring ||
              event.epcis?.busStep ===
                VerifiableCredentialType.EPCISEventTransporting
          );
        this.epcisEvents = [
          ...this.epcisEvents,
          ...(epcisEvents ? epcisEvents : []),
        ];
      } else {
        // other than Event VC
        const vcWrapper = new VerifiableCredentialWrapper(
          this.workflow.workflowObjId,
          vcType,
          this.googleGeocoderService,
          this.httpClient
        );

        // parse the issuer and subject
        const triplet = this.workflow.triplets.find(
          (triplet) => triplet.vcType === vcType
        );

        vcWrapper.setIssuer(triplet?.issuer ?? {});
        vcWrapper.setSubjects(triplet?.subjects);

        this.vcList.push(vcWrapper);

        vcWrappersInit.push(vcWrapper.init());
      }
    }

    await Promise.all(vcWrappersInit);

    // store vc convenenience array
    this.workflow.vcList = [];

    for (const vcWrapper of this.vcList) {
      this.workflow.vcList.push(vcWrapper.getDetails()?.vc);
    }

    await this.loadMarkers();

    this.vcTypesDesc = [];

    workflowSchema.forEach((schemaVcType) => {
      // if this vc is present then add it, otherwise, add a ghost
      const vcWrapper = this.vcList.find(
        (vcWrapper) => schemaVcType === vcWrapper.getVcType()
      );
      if (vcWrapper) {
        this.vcTypesDesc.push({
          vcType: vcWrapper.getVcType(),
          config: { workflowObjId: this.workflow.workflowObjId, vcWrapper },
          missing: false,
        });
      } else {
        this.vcTypesDesc.push({
          vcType: schemaVcType,
          config: {
            vcWrapper:
              VerifiableCredentialWrapper.getGhostWrapper(schemaVcType),
          } as any,
          missing: true,
        });
      }
    });

    this.vcWrappersInitialized = true;

    this.hasEvents = !!this.vcList.find(
      (vcWrapper) =>
        vcWrapper.getVcType() ==
          VerifiableCredentialType.EPCISEventTransferring ||
        vcWrapper.getVcType() == VerifiableCredentialType.EPCISEventTransporting
    );
  }
  async loadMarkers() {
    this.markers = [];

    // enrich the epcis events with latlng
    for (const epcisEvent of this.epcisEvents) {
      if (!epcisEvent.source.geoLatud || !epcisEvent.source.geoLngtud) {
        const latlng = await this.googleGeocoderService.geocode(
          epcisEvent.source.addrLn1,
          epcisEvent.source.addrLoc,
          epcisEvent.source.addrRgn,
          epcisEvent.source.addrPstlCd
        );
        epcisEvent.source.geoLatud = latlng[0];
        epcisEvent.source.geoLngtud = latlng[1];
      }
    }

    // create markers from the events
    // e[0].source => e[0].destination ... e[N-1].source => e[N-1].destination
    for (let i = this.epcisEvents.length - 1; i >= 0; i--) {
      let epcisEvent = this.epcisEvents[i];
      // e[i].source
      this.markers = [
        ...this.markers,
        {
          id: uuidv4(),
          label: '',
          position: {
            lat: epcisEvent.source.geoLatud,
            lng: epcisEvent.source.geoLngtud,
          },
          title: epcisEvent.source.orgNm,
      
        },
      ];
      // e[i].destination
      this.markers = [
        ...this.markers,
        {
          id: uuidv4(),
          label: '',
          position: {
            lat: epcisEvent.destination.geoLatud,
            lng: epcisEvent.destination.geoLngtud,
          },
          title: epcisEvent.destination.orgNm,
        },
      ];
    };

    console.log('>>> epcis events', this.epcisEvents, this.markers);
  }
  createMockMarkers() {
    const points = [
      {
        position: { lat: 43.1, lng: 1.45 },
        label: 'Scenic Ridge',
      },
      {
        position: { lat: 43.15, lng: 1.5 },
        label: 'Valley Overlook',
      },
      {
        position: { lat: 43.2, lng: 1.55 },
        label: 'Plateau',
      },
      {
        position: { lat: 43.25, lng: 1.6 },
        label: 'Forest Edge',
      },
      {
        position: { lat: 43.3, lng: 1.65 },
        label: 'River Crossing',
      },
      {
        position: { lat: 43.28, lng: 1.58 },
        label: 'Meadow',
      },
      {
        position: { lat: 43.23, lng: 1.52 },
        label: 'Return Launch',
      },
    ];

    this.markers = points.map((p) => {
      return {
        id: uuidv4(),
        ...p,
        title: p.label,
      };
    });
  }
  loadWorkflow() {
    return this.workflowId
      ? this.traceVocabService.getWorkflow(this.workflowId)
      : this.traceVocabService.getWorkflowFromEntryNumber(this.entryNumber);
  }

  onSelectTab(selectedTabId: VcTabId) {
    switch (selectedTabId) {
      case cargoFlowMapTabId:
        this.selectedTabId = cargoFlowMapTabId;

        break;

      case eventsTabId:
        this.selectedTabId = eventsTabId;
        break;
      default:
        this.selectSchemaType = selectedTabId;

        this.selectedTabId = this.selectSchemaType;

        break;
    }

    if (selectedTabId !== cargoFlowMapTabId) {
      this.issuerMarker = null;
    }
    this.localStateService.clearIssuerMarkers();
  }
  onDiagramElementClick(diagramElementEvent: DiagramElementEvent) {
    const eventType = diagramElementEvent.type;

    if (eventType === 'vc') {
      if (
        VerifiableCredentialWrapper.isEpcisEvent(
          diagramElementEvent.vcWrapper.getVcType()
        )
      ) {
        this.selectedTabId = 'events';
        this.selectSchemaType = diagramElementEvent.vcWrapper.getVcType();
        this.selectEventOrder = diagramElementEvent.eventOrder!;
      } else {
        this.selectedTabId = diagramElementEvent.vcWrapper.getVcType();
      }
      this.selectedVcWrapper = diagramElementEvent.vcWrapper;
    } else {
      if (diagramElementEvent.type === 'show-issuer-location') {
        this.selectedTabId = cargoFlowMapTabId;

        this.issuerMarker = {
          id: uuidv4(),
          label: 'Issuer',
          title: 'Issuer',
          position: diagramElementEvent.address,
          semantics: 'issuer-location',
        };
      } else if (diagramElementEvent.type === 'clear-issuer-location') {
        this.selectedTabId = cargoFlowMapTabId;

        this.issuerMarker = null;
      }
    }
    this.tabsDefaultId = this.selectedTabId;
  }
  onHighlightPositionChanged(position: google.maps.LatLngLiteral) {
    this.highlightPosition = position;
  }

  onBack() {
    this.location.back();
  }
}
