ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:187:91 - error TS2345: Argument of type 'number | undefined' is not assignable to parameter of type 'number'.
  Type 'undefined' is not assignable to type 'number'.

187                                   <div class="grid-row {{expandedRows.includes(mraRequest.index) ? 'selected' : ''}}"
                                                                                              ~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:188:69 - error TS2345: Argument of type 'number | undefined' is not assignable to parameter of type 'number'.
  Type 'undefined' is not assignable to type 'number'.

188                                       (click)="toggleRow(mraRequest.index)">
                                                                        ~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:201:76 - error TS2532: Object is possibly 'undefined'.

201                                               [ngClass]="mraRequest.status.class === 'approved' ? 'approved-text' :
                                                                               ~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:202:65 - error TS2532: Object is possibly 'undefined'.

202                                               mraRequest.status.class === 'rejected' ? 'rejected-text' : 'pending-text'">
                                                                    ~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:203:67 - error TS2532: Object is possibly 'undefined'.

203                                               {{mraRequest.status.value}}
                                                                      ~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:209:111 - error TS2345: Argument of type 'number | undefined' is not assignable to parameter of type 'number'.
  Type 'undefined' is not assignable to type 'number'.

209                                   <div class="table-details-wrapper" [class.expanded]="rowExpanded(mraRequest.index)">
                                                                                                                  ~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:343:94 - error TS2339: Property 'updtDttm' does not exist on type 'MRARequestCompany'.

343                                                               {{company.request.mraRqstCmpny.updtDttm}}
                                                                                                 ~~~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:357:109 - error TS2339: Property 'name' does not exist on type 'EditMRARequestInput | ApprovalOverrideInput'.
  Property 'name' does not exist on type 'ApprovalOverrideInput'.

357                                                                           [(ngModel)]="company.edit.payload.name">
                                                                                                                ~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:357:109 - error TS2339: Property 'name' does not exist on type 'EditMRARequestInput | ApprovalOverrideInput'.
  Property 'name' does not exist on type 'ApprovalOverrideInput'.

357                                                                           [(ngModel)]="company.edit.payload.name">
                                                                                                                ~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:357:109 - error TS2532: Object is possibly 'undefined'.

357                                                                           [(ngModel)]="company.edit.payload.name">
                                                                                                                ~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:357:109 - error TS2532: Object is possibly 'undefined'.

357                                                                           [(ngModel)]="company.edit.payload.name">
                                                                                                                ~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:358:113 - error TS2339: Property 'name' does not exist on type 'EditMRARequestInput | ApprovalOverrideInput'.
  Property 'name' does not exist on type 'ApprovalOverrideInput'.

358                                                                       <ng-container *ngIf="company.edit.payload.name">
                                                                                                                    ~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:358:113 - error TS2532: Object is possibly 'undefined'.

358                                                                       <ng-container *ngIf="company.edit.payload.name">
                                                                                                                    ~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:361:109 - error TS2339: Property 'name' does not exist on type 'EditMRARequestInput | ApprovalOverrideInput'.
  Property 'name' does not exist on type 'ApprovalOverrideInput'.

361                                                                               (click)="company.edit.payload.name=''"
                                                                                                                ~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:361:109 - error TS2532: Object is possibly 'undefined'.

361                                                                               (click)="company.edit.payload.name=''"
                                                                                                                ~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:376:109 - error TS2339: Property 'tin' does not exist on type 'EditMRARequestInput | ApprovalOverrideInput'.
  Property 'tin' does not exist on type 'ApprovalOverrideInput'.

376                                                                           [(ngModel)]="company.edit.payload.tin">
                                                                                                                ~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:376:109 - error TS2339: Property 'tin' does not exist on type 'EditMRARequestInput | ApprovalOverrideInput'.
  Property 'tin' does not exist on type 'ApprovalOverrideInput'.

376                                                                           [(ngModel)]="company.edit.payload.tin">
                                                                                                                ~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:376:109 - error TS2532: Object is possibly 'undefined'.

376                                                                           [(ngModel)]="company.edit.payload.tin">
                                                                                                                ~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:376:109 - error TS2532: Object is possibly 'undefined'.

376                                                                           [(ngModel)]="company.edit.payload.tin">
                                                                                                                ~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:377:113 - error TS2339: Property 'tin' does not exist on type 'EditMRARequestInput | ApprovalOverrideInput'.
  Property 'tin' does not exist on type 'ApprovalOverrideInput'.

377                                                                       <ng-container *ngIf="company.edit.payload.tin">
                                                                                                                    ~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:377:113 - error TS2532: Object is possibly 'undefined'.

377                                                                       <ng-container *ngIf="company.edit.payload.tin">
                                                                                                                    ~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:380:109 - error TS2339: Property 'tin' does not exist on type 'EditMRARequestInput | ApprovalOverrideInput'.
  Property 'tin' does not exist on type 'ApprovalOverrideInput'.

380                                                                               (click)="company.edit.payload.tin=''"
                                                                                                                ~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:380:109 - error TS2532: Object is possibly 'undefined'.

380                                                                               (click)="company.edit.payload.tin=''"
                                                                                                                ~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:394:109 - error TS2339: Property 'roleList' does not exist on type 'EditMRARequestInput | ApprovalOverrideInput'.
  Property 'roleList' does not exist on type 'ApprovalOverrideInput'.

394                                                                           [(ngModel)]="company.edit.payload.roleList">
                                                                                                                ~~~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:394:109 - error TS2339: Property 'roleList' does not exist on type 'EditMRARequestInput | ApprovalOverrideInput'.
  Property 'roleList' does not exist on type 'ApprovalOverrideInput'.

394                                                                           [(ngModel)]="company.edit.payload.roleList">
                                                                                                                ~~~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:394:109 - error TS2532: Object is possibly 'undefined'.

394                                                                           [(ngModel)]="company.edit.payload.roleList">
                                                                                                                ~~~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:394:109 - error TS2532: Object is possibly 'undefined'.

394                                                                           [(ngModel)]="company.edit.payload.roleList">
                                                                                                                ~~~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:395:113 - error TS2339: Property 'roleList' does not exist on type 'EditMRARequestInput | ApprovalOverrideInput'.
  Property 'roleList' does not exist on type 'ApprovalOverrideInput'.

395                                                                       <ng-container *ngIf="company.edit.payload.roleList">
                                                                                                                    ~~~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:395:113 - error TS2532: Object is possibly 'undefined'.

395                                                                       <ng-container *ngIf="company.edit.payload.roleList">
                                                                                                                    ~~~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:398:109 - error TS2339: Property 'roleList' does not exist on type 'EditMRARequestInput | ApprovalOverrideInput'.
  Property 'roleList' does not exist on type 'ApprovalOverrideInput'.

398                                                                               (click)="company.edit.payload.roleList=''"
                                                                                                                ~~~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:398:109 - error TS2532: Object is possibly 'undefined'.

398                                                                               (click)="company.edit.payload.roleList=''"
                                                                                                                ~~~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:410:97 - error TS2532: Object is possibly 'undefined'.

410                                                               [(ngModel)]="company.edit.payload.comment"
                                                                                                    ~~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:410:97 - error TS2532: Object is possibly 'undefined'.

410                                                               [(ngModel)]="company.edit.payload.comment"
                                                                                                    ~~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:441:111 - error TS2339: Property 'status' does not exist on type 'EditMRARequestInput | ApprovalOverrideInput'.
  Property 'status' does not exist on type 'EditMRARequestInput'.

441                                                                       <mat-label *ngIf="!company.edit.payload.status" class="mra-status-label">MRA Request Statuses</mat-label>
                                                                                                                  ~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:441:111 - error TS2532: Object is possibly 'undefined'.

441                                                                       <mat-label *ngIf="!company.edit.payload.status" class="mra-status-label">MRA Request Statuses</mat-label>
                                                                                                                  ~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:442:115 - error TS2339: Property 'status' does not exist on type 'EditMRARequestInput | ApprovalOverrideInput'.
  Property 'status' does not exist on type 'EditMRARequestInput'.

442                                                                       <mat-select [(value)]="company.edit.payload.status"
                                                                                                                      ~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:442:115 - error TS2339: Property 'status' does not exist on type 'EditMRARequestInput | ApprovalOverrideInput'.
  Property 'status' does not exist on type 'EditMRARequestInput'.

442                                                                       <mat-select [(value)]="company.edit.payload.status"
                                                                                                                      ~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:442:115 - error TS2532: Object is possibly 'undefined'.

442                                                                       <mat-select [(value)]="company.edit.payload.status"
                                                                                                                      ~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:442:115 - error TS2532: Object is possibly 'undefined'.

442                                                                       <mat-select [(value)]="company.edit.payload.status"
                                                                                                                      ~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:457:97 - error TS2532: Object is possibly 'undefined'.

457                                                               [(ngModel)]="company.edit.payload.comment"
                                                                                                    ~~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:457:97 - error TS2532: Object is possibly 'undefined'.

457                                                               [(ngModel)]="company.edit.payload.comment"
                                                                                                    ~~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
ERROR
src/app/mra-request-full-view/mra-request-full-view.component.html:491:120 - error TS2345: Argument of type 'MRARequestCompanyLocation' is not assignable to parameter of type 'CompanyLocation'.

491                                                                                   *ngFor="let address of createAddress(location)">
                                                                                                                           ~~~~~~~~

  src/app/mra-request-full-view/mra-request-full-view.component.ts:61:15
    61  templateUrl: './mra-request-full-view.component.html',
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    Error occurs in the template of component MRARequestFullViewComponent.
