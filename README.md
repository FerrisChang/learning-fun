
<div class="container-fluid  ">
  <h1 class="  px-4 pt-4 pb-2 " style="border-bottom: 2px solid rgba(0, 60, 110, 0.15);">
      <svg xmlns="http://www.w3.org/2000/svg" width="22" height="22" fill="currentColor" class="bi bi-search"
          viewBox="0 0 18 18">
          <path
              d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001q.044.06.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1 1 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0" />
      </svg>
      Search MRA Requests
  </h1>
</div>


<div class="row mx-4 pb-3">
  Once MRA Requests are searched, these following search filters are available to filter the current results.
</div>

<div class="input-container pb-3" *ngIf="mraRequestCompanies.length>0">
  <div class="filters-container">
      <div class="filters-row">
          <mat-form-field class="search-form-field">
              <mat-label>MRA Request ID</mat-label>
              <input matInput type="text" [(ngModel)]="searchItems.request_id"
                  (ngModelChange)="filterUpdate('request_id', $event)">
              <ng-container *ngIf="searchItems.request_id">
                  <button matSuffix mat-icon-button aria-label="Clear"
                      (click)="searchItems.request_id='';filterUpdate('request_id', '')"
                      style="position: absolute; right: 0px; top: 5px;">
                      <mat-icon>close</mat-icon>
                  </button>
              </ng-container>
          </mat-form-field>
          <mat-form-field class="search-form-field">
              <mat-label>Company Name</mat-label>
              <input matInput type="text" [(ngModel)]="searchItems.company"
                  (ngModelChange)="filterUpdate('name', $event)">
              <ng-container *ngIf="searchItems.company">
                  <button matSuffix mat-icon-button aria-label="Clear"
                      (click)="searchItems.company='';filterUpdate('name', '')"
                      style="position: absolute; right: 0px; top: 5px;">
                      <mat-icon>close</mat-icon>
                  </button>
              </ng-container>
          </mat-form-field>
          <!--
          Date field to be figured out another time 
          -->
          <!-- <form [formGroup]="dateRangeForm">
              <mat-form-field class="search-form-field" style="width: 300px;">
                  <mat-label>Enter the date updated range</mat-label>
                  <mat-date-range-input [rangePicker]="picker" formGroupName="dateRange">
                      <input matStartDate formControlName="start" placeholder="Start date">
                      <input matEndDate formControlName="end" placeholder="End date">
                  </mat-date-range-input>
                  <mat-hint>MM/DD/YYYY – MM/DD/YYYY</mat-hint>
                  <mat-datepicker-toggle matIconSuffix [for]="picker"></mat-datepicker-toggle>
                  <mat-date-range-picker #picker></mat-date-range-picker>
              </mat-form-field>
          </form> -->
          <mat-form-field class="search-form-field">
              <mat-label>TIN</mat-label>
              <input matInput type="text" [(ngModel)]="searchItems.tin" (ngModelChange)="filterUpdate('tin', $event)">
              <ng-container *ngIf="searchItems.tin">
                  <button matSuffix mat-icon-button aria-label="Clear"
                      (click)="searchItems.tin=''; filterUpdate('tin', '')"
                      style="position: absolute; right: 0px; top: 5px;">
                      <mat-icon>close</mat-icon>
                  </button>
              </ng-container>
          </mat-form-field>
      </div>
      <div class="filters-row">
          <mat-form-field class="search-form-field">
              <mat-label>G2G ID</mat-label>
              <input matInput type="text" [(ngModel)]="searchItems.g2g_id"
                  (ngModelChange)="filterUpdate('g2g_id', $event)">
              <ng-container *ngIf="searchItems.g2g_id">
                  <button matSuffix mat-icon-button aria-label="Clear"
                      (click)="searchItems.g2g_id='';filterUpdate('g2g_id', '')"
                      style="position: absolute; right: 0px; top: 5px;">
                      <mat-icon>close</mat-icon>
                  </button>
              </ng-container>
          </mat-form-field>
          <mat-form-field class="search-form-field">
              <mat-label>MRA Request Statuses</mat-label>
              <mat-select [formControl]="statusesForm" multiple [panelWidth]="''">
                  <mat-option *ngFor="let mraRequestStatus of MRAList"
                      [value]="mraRequestStatus.value">{{mraRequestStatus.name}}</mat-option>
              </mat-select>
          </mat-form-field>
          <mat-form-field class="search-form-field">
              <mat-label>From Country</mat-label>
              <mat-select [formControl]="countriesForm" multiple [panelWidth]="''">
                  <mat-option *ngFor="let country of mainCountries" [value]="country.code">
                      <span *ngIf="country.name.length>2"
                          class="flag-input fi fi-{{country.code.toLowerCase()}}"></span>
                      {{country.name}}</mat-option>
              </mat-select>
          </mat-form-field>
      </div>
  </div>

</div>
<div class="searching-container" *ngIf="searching.length>0 && searching!=='error'">
  <mat-progress-spinner class="spinner" [mode]="'indeterminate'"></mat-progress-spinner>
  <div class="searching-text">
      {{searching}}
  </div>
</div>
<div class="error-search-frame text-danger" *ngIf="searching==='error'">
  <h3>Unable to connect to the server, please try again or contact support.</h3>
</div>

<div class="container-fluid" style="  position: relative;" *ngIf="searching.length===0">
  <div class="row">
      <div class="col ">
          <div class="card widget-card">
              <div class="card-header card-hdr-class">
                  <div class="row">
                      <div class="col">
                          <h4 class="card-title mb-1 pt-1">Search Results</h4>
                      </div>
                      <div class="col-1 float-right">
                          <button class="btn btn-sm px-2 pt-1 pb-1 mb-0 mt-0  " [matMenuTriggerFor]="menu">
                              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="#FFFFFF"
                                  class="bi bi-three-dots-vertical" viewBox="0 0 16 16">
                                  <path
                                      d="M9.5 13a1.5 1.5 0 1 1-3 0 1.5 1.5 0 0 1 3 0zm0-5a1.5 1.5 0 1 1-3 0 1.5 1.5 0 0 1 3 0zm0-5a1.5 1.5 0 1 1-3 0 1.5 1.5 0 0 1 3 0z" />
                              </svg>
                          </button>
                          <mat-menu #menu="matMenu">
                              <button mat-menu-item (click)="exportToJSON()">Export JSON</button>
                          </mat-menu>
                      </div>
                  </div>
              </div>
              <div class="card-body px-0 pt-0 pb-0 ">
                  <div class="companies-table-frame">
                      <div class="table">
                          <div class="grid-container">
                              <div class="grid-header">
                                  <div class="grid-cell sortable-header" (click)="sort('index')">
                                      <div class="header-content">
                                          #
                                          <mat-icon class="sort-icon">{{getSortIcon('index')}}</mat-icon>
                                      </div>
                                  </div>
                                  <div class="grid-cell sortable-header" (click)="sort('request_id')">
                                      <div class="header-content">
                                          MRA Request ID
                                          <mat-icon class="sort-icon">{{getSortIcon('request_id')}}</mat-icon>
                                      </div>
                                  </div>
                                  <div class="grid-cell sortable-header" (click)="sort('name')">
                                      <div class="header-content">
                                          AEO Program
                                          <mat-icon class="sort-icon">{{getSortIcon('name')}}</mat-icon>
                                      </div>
                                  </div>
                                  <div class="grid-cell sortable-header" (click)="sort('count')">
                                      <div class="header-content">
                                          Company Count
                                          <mat-icon class="sort-icon">{{getSortIcon('count')}}</mat-icon>
                                      </div>
                                  </div>
                                  <div class="grid-cell sortable-header" (click)="sort('country')">
                                      <div class="header-content">
                                          Country
                                          <mat-icon class="sort-icon">{{getSortIcon('country')}}</mat-icon>
                                      </div>
                                  </div>
                                  <div class="grid-cell">
                                      Status Flags
                                  </div>
                                  <div class="grid-cell">
                                      Current MRA Status
                                  </div>
                                  <div class="grid-cell sortable-header" (click)="sort('date_updated')">
                                      <div class="header-content">
                                          Last Updated Date
                                          <mat-icon class="sort-icon">{{getSortIcon('date_updated')}}</mat-icon>
                                      </div>
                                  </div>
                              </div>

                              <div class="grid-row-wrapper" *ngFor="let mraRequest of filteredMraRequests">
                                  <div class="grid-row {{expandedRows.includes(mraRequest.index ?? 0) ? 'selected' : ''}}"
                                      (click)="toggleRow(mraRequest.index ?? 0)">
                                      <div class="grid-cell">{{mraRequest.index}}</div>
                                      <div class="grid-cell">{{mraRequest.request_id}}</div>
                                      <div class="grid-cell">{{mraRequest.aeo_name}}</div>
                                      <div class="grid-cell">{{mraRequest.company_count}}</div>
                                      <div class="grid-cell">{{getThisCountryNameFromISO(mraRequest.country)}}</div>
                                      <div class="grid-cell">
                                          <button *ngIf="mraRequest.flags.mraStatus" mat-raised-button
                                              [matTooltip]="generateStatusTooltip(mraRequest.flags.mraStatus)"
                                              [matTooltipClass]="'status-tooltip'" matTooltipPosition="right">Status</button>
                                      </div>
                                      <div class="grid-cell">
                                          <div class="table-cell status-text"
                                              [ngClass]="mraRequest.status?.class === 'approved' ? 'approved-text' : 
                                              mraRequest.status?.class === 'rejected' ? 'rejected-text' : 'pending-text'">
                                              {{mraRequest.status?.value}}
                                          </div>
                                      </div>
                                      <div class="grid-cell">{{mraRequest.date_updated}}</div>
                                  </div>

                                  <div class="table-details-wrapper" [class.expanded]="rowExpanded(mraRequest.index ?? 0)">
                                      <div class="mra-request-information-frame">
                                          <div class="mra-request-information-header">
                                              Addtional Information About MRA Request
                                          </div>
                                          <div class="mra-request-content">
                                              <div class="details-field-container">
                                                  <mat-label class="details-header">MRA Request
                                                      ID:</mat-label>
                                                  <span class="details-field">
                                                      {{mraRequest.request_id}}
                                                  </span>
                                              </div>
                                              <div class="details-field-container">
                                                  <mat-label class="details-header">Aeo Program Name:</mat-label>
                                                  <span class="details-field">
                                                      {{mraRequest.aeo_name}}
                                                  </span>
                                              </div>
                                              <div class="details-field-container">
                                                  <mat-label class="details-header">Country:</mat-label>
                                                  <span class="details-field">
                                                      {{getThisCountryNameFromISO(mraRequest.country)}}
                                                  </span>
                                              </div>
                                              <div class="details-field-container">
                                                  <mat-label class="details-header">Company Count:</mat-label>
                                                  <span class="details-field">
                                                      {{mraRequest.company_count}}
                                                  </span>
                                              </div>
                                              
                                              <div class="details-field-container">
                                                  <mat-label class="details-header">Comment:</mat-label>
                                                  <span class="details-field">
                                                      {{mraRequest.request.cmt}}
                                                  </span>
                                              </div>
                                              <div class="details-field-container">
                                                  <mat-label class="details-header">Date Updated:</mat-label>
                                                  <span class="details-field">
                                                      {{mraRequest.date_updated}}
                                                  </span>
                                              </div>
                                              <div class="details-field-container">
                                                  <mat-label class="details-header">Created Date:</mat-label>
                                                  <span class="details-field">
                                                      {{mraRequest.request.ctte_ddtm}}
                                                  </span>
                                              </div>
                                              <div class="details-field-container">
                                                  <mat-label class="details-header">Message Sender ID:</mat-label>
                                                  <span class="details-field">
                                                      {{mraRequest.request.msgSndrId}}
                                                  </span>
                                              </div>
                                              <div class="details-field-container">
                                                  <mat-label class="details-header">Message Sent Date:</mat-label>
                                                  <span class="details-field">
                                                      {{mraRequest.request.msgSentDttm}}
                                                  </span>
                                              </div>
                                          </div>
                                          
                                      </div>
                                      <mat-expansion-panel *ngFor="let company of mraRequest.companies" [expanded]="true">
                                          <mat-expansion-panel-header class="details-expansion-header">
                                              <mat-panel-title>
                                                  <div class="flag"
                                                      *ngIf="getThisCountryNameFromISO(company.country).length>2">
                                                      <span
                                                          class="flag-detail fi fi-{{company.country.toLowerCase()}}"></span>
                                                  </div>
                                                  {{company.company_name}}
                                              </mat-panel-title>
                                              <mat-panel-description>
                                                  
                                              </mat-panel-description>
                                          </mat-expansion-panel-header>
                                          <div class="company-frame">
                                              <div class="content">
                                                  <div class="information details-row" *ngIf="company.edit.mode==''">
                                                      <div class="details-field-container">
                                                          <mat-label class="details-header">Country:</mat-label>
                                                          <span class="details-field">
                                                              {{getThisCountryNameFromISO(company.country)}}
                                                          </span>
                                                      </div>
                                                      <div class="details-field-container">
                                                          <mat-label class="details-header">MRA Request
                                                              ID:</mat-label>
                                                          <span class="details-field">
                                                              {{company.request_id}}
                                                          </span>
                                                      </div>
                                                      <div class="details-field-container">
                                                          <mat-label class="details-header">G2G ID:</mat-label>
                                                          <span class="details-field">
                                                              {{company.g2g_id}}
                                                          </span>
                                                      </div>

                                                      <div class="details-field-container">
                                                          <mat-label class="details-header">TIN:</mat-label>
                                                          <span class="details-field">
                                                              {{company.request.mraRqstCmpny.tin}}
                                                          </span>
                                                      </div>

                                                      <div class="details-field-container">
                                                          <mat-label class="details-header">Role List:</mat-label>
                                                          <span class="details-field">
                                                              {{company.request.mraRqstCmpny.rolList}}
                                                          </span>
                                                      </div>
                                                      <div class="details-field-container">
                                                          <mat-label class="details-header">Status:</mat-label>
                                                          <div class="details-field status-details-text"
                                                              [ngClass]="company.status.class === 'approved' ? 'approved-text' : 
                                                                      company.status.class === 'rejected' ? 'rejected-text' : 'pending-text'">
                                                              {{company.status.value}}
                                                          </div>
                                                      </div>
                                                      <div class="details-field-container">
                                                          <mat-label class="details-header">Approval Status
                                                              Updated:</mat-label>
                                                          <span class="details-field">
                                                              {{company.request.mraRqstCmpny.apprvlStusDttm}}
                                                          </span>
                                                      </div>
                                                      <div class="details-field-container">
                                                          <mat-label class="details-header">Date Last
                                                              Updated:</mat-label>
                                                          <span class="details-field">
                                                              {{company.updatedDate || company.request.mraRqstCmpny.apprvlStusDttm}}
                                                          </span>
                                                      </div>
                                                  </div>
                                                  <ng-container *ngIf="company.edit.mode=='edit' && getEditPayload(company) as editPayload">
                                                  <div class="edit details-row">
                                                      <div class="edit-frame">
                                                          <div class="edit-field-container">
                                                              <mat-label class="details-header"
                                                                  [class.text-error]="company.edit.error.includes('name')">Company
                                                                  Name:</mat-label>
                                                              <span class="details-field">
                                                                  <mat-form-field class="edit-form-field">
                                                                      <mat-label>Company Name</mat-label>
                                                                      <input matInput type="text"
                                                                          [(ngModel)]="editPayload.name">
                                                                      <ng-container *ngIf="editPayload.name">
                                                                          <button matSuffix mat-icon-button
                                                                              aria-label="Clear"
                                                                              (click)="editPayload.name=''"
                                                                              style="position: absolute; right: 0px; top: 5px;">
                                                                              <mat-icon>close</mat-icon>
                                                                          </button>
                                                                      </ng-container>
                                                                  </mat-form-field>
                                                              </span>
                                                          </div>
                                                          <div class="edit-field-container">
                                                              <mat-label class="details-header"
                                                                  [class.text-error]="company.edit.error.includes('TIN')">TIN:</mat-label>
                                                              <span class="details-field">
                                                                  <mat-form-field class="edit-form-field">
                                                                      <mat-label>TIN</mat-label>
                                                                      <input matInput type="text"
                                                                          [(ngModel)]="editPayload.tin">
                                                                      <ng-container *ngIf="editPayload.tin">
                                                                          <button matSuffix mat-icon-button
                                                                              aria-label="Clear"
                                                                              (click)="editPayload.tin=''"
                                                                              style="position: absolute; right: 0px; top: 5px;">
                                                                              <mat-icon>close</mat-icon>
                                                                          </button>
                                                                      </ng-container>
                                                                  </mat-form-field>
                                                              </span>
                                                          </div>
                                                          <div class="edit-field-container">
                                                              <mat-label class="details-header">Role List:</mat-label>
                                                              <span class="details-field">
                                                                  <mat-form-field class="edit-form-field">
                                                                      <mat-label>Role List</mat-label>
                                                                      <input matInput type="text"
                                                                          [(ngModel)]="editPayload.roleList">
                                                                      <ng-container *ngIf="editPayload.roleList">
                                                                          <button matSuffix mat-icon-button
                                                                              aria-label="Clear"
                                                                              (click)="editPayload.roleList=''"
                                                                              style="position: absolute; right: 0px; top: 5px;">
                                                                              <mat-icon>close</mat-icon>
                                                                          </button>
                                                                      </ng-container>
                                                                  </mat-form-field>
                                                              </span>
                                                          </div>
                                                      </div>
                                                      <div class="comment-frame">
                                                          <textarea class="comment" matInput
                                                              [class.error]="company.edit.error.includes('comment')"
                                                              [(ngModel)]="editPayload.comment"
                                                              placeholder="Enter comment here..." cdkTextareaAutosize
                                                              #autosize="cdkTextareaAutosize" cdkAutosizeMinRows="1"
                                                              cdkAutosizeMaxRows="6"></textarea>
                                                      </div>
                                                  </div>
                                                  </ng-container>
                                                  <ng-container *ngIf="company.edit.mode=='override' && getOverridePayload(company) as overridePayload">
                                                  <div class="override details-row">
                                                      <div class="status-transformation-frame">
                                                          <div class="status-frame">
                                                              <div class="status-header">
                                                                  Current Approval Status
                                                              </div>
                                                              <div class="current-status">
                                                                  <div class="current-status-container {{company.status.class}}">
                                                                      <div class="status-text">
                                                                          {{company.status.value}}
                                                                      </div>
                                                                  </div>
                                                              </div>
                                                          </div>
                                                          <div class="transformation">
                                                              <mat-icon
                                                                  class="transformation-icon">chevron_right</mat-icon>
                                                          </div>
                                                          <div class="status-frame">
                                                              <div class="status-header"
                                                                  [class.text-error]="company.edit.error.includes('Status')">
                                                                  Updated Approval Status
                                                              </div>
                                                              <div class="current-status">
                                                                  <mat-form-field appearance="fill" [id]="getStatusFieldId(company.request_id)" class="current-status">
                                                                      <mat-label *ngIf="!overridePayload.status" class="mra-status-label">MRA Request Statuses</mat-label>
                                                                      <mat-select [(value)]="overridePayload.status"
                                                                          [panelWidth]="''"
                                                                          [id]="getStatusSelectId(company.request_id)"
                                                                          (selectionChange)="addStatusClassToTextField($event.value, company.request_id)">
                                                                          <mat-option *ngFor="let companyStatus of MRAList"
                                                                              class="mra-status-option {{companyStatus.class}}"
                                                                              [value]="companyStatus.value">{{companyStatus.name.toUpperCase()}}</mat-option>
                                                                      </mat-select>
                                                                  </mat-form-field>
                                                              </div>
                                                          </div>
                                                      </div>
                                                      <div class="comment-frame">
                                                          <textarea class="comment" matInput
                                                              [class.error]="company.edit.error.includes('comment')"
                                                              [(ngModel)]="overridePayload.comment"
                                                              placeholder="Enter comment here..." cdkTextareaAutosize
                                                              #autosize="cdkTextareaAutosize" cdkAutosizeMinRows="1"
                                                              cdkAutosizeMaxRows="6"></textarea>
                                                      </div>
                                                  </div>
                                                  </ng-container>
                                                  <div class="locations-row">
                                                      <mat-expansion-panel [expanded]="true">
                                                          <mat-expansion-panel-header class="details-expansion-header">
                                                              <mat-panel-title>
                                                                  Company Locations
                                                              </mat-panel-title>
                                                          </mat-expansion-panel-header>
                                                          <div class="details-location-frame">
                                                              <div *ngFor="let location of company.request.mraRqstCmpny.mraRqstCmpnyLocList; index as locationIndex;"
                                                                  class="location-detail"
                                                                  [class.primaryLocation]="location.isPrmryLoc">
                                                                  <div class="location-header"
                                                                      (click)="openAddressModal(company, location)">
                                                                      {{location.isPrmryLoc ? 'Primary Location' : 'Other
                                                                      Location
                                                                      '+(locationIndex)}}:
                                                                  </div>
                                                                  <div class="flex-row">
                                                                      <div class="details-field-container">
                                                                          <mat-label class="details-header">Company
                                                                              Address: </mat-label>
                                                                          <span class="details-field"
                                                                              *ngIf="location.cmpnyAddr">
                                                                              {{location.cmpnyAddr}}
                                                                          </span>
                                                                          <div class="address details-field"
                                                                              *ngIf="!location.cmpnyAddr">
                                                                              <div class="address-text"
                                                                                  *ngFor="let address of createAddress(location)">
                                                                                  {{address}}
                                                                              </div>
                                                                          </div>
                                                                      </div>
                                                                      <div class="details-field-container">
                                                                          <mat-label class="detail-header">Location Role: </mat-label>
                                                                          <div class="details-field">{{getLocationRole(location)}}</div>
                                                                      </div>
                                                                  </div>
                                                                  <div class="flex-row">
                                                                      <div class="details-field-container"
                                                                          *ngIf="location.lngtud && location.latud">
                                                                          <mat-label
                                                                              class="details-header">Longitude/Latitude:</mat-label>
                                                                          <div class="details-field">
                                                                              {{location.lngtud}}/{{location.latud}}
                                                                          </div>
                                                                      </div>
                                                                      <div class="details-field-container">
                                                                          <mat-label class="details-header">Last
                                                                              Updated:</mat-label>
                                                                          <span class="details-field">
                                                                              {{location.updtDttm}}
                                                                          </span>
                                                                      </div>
                                                                      <div class="details-field-container">
                                                                          <mat-label class="details-header">Primary
                                                                              Location:</mat-label>
                                                                          <div class=" details-primary-location status-text details-field"
                                                                              [ngClass]="location.isPrmryLoc ? 'approved-text' :  'rejected-text'">
                                                                              {{location.isPrmryLoc ? 'Yes': 'No'}}
                                                                          </div>
                                                                      </div>
                                                                  </div>
                                                              </div>
                                                          </div>
                                                      </mat-expansion-panel>
                                                  </div>
                                              </div>
                                              <div class="actions-frame">
                                                  <button mat-raised-button *ngIf="company.edit.mode.length>0"
                                                      (click)="toggleEditMode(company, '', 0)"
                                                      class="btn btn-sm">Cancel</button>
                                                  <button mat-raised-button *ngIf="company.edit.mode.length>0"
                                                      (click)="saveEdit(company, 0)" color="primary"
                                                      class="btn btn-sm">Save</button>
                                                  <button mat-raised-button class="additional-action-item"
                                                      *ngIf="company.edit.mode.length===0" (click)="openModal(company)">
                                                      Details</button>
                                                  <button mat-raised-button color="primary" class="additional-action-item"
                                                      *ngIf="company.edit.mode.length===0"
                                                      (click)="toggleEditMode(company, 'override', 0)">Override
                                                      Approval Status</button>
                                                  <div *ngIf="company.edit.error.length>0" class="error-message rejected-text">
                                                      {{company.edit.error}}
                                                  </div>
                                              </div>
                                          </div>


                                      </mat-expansion-panel>
                                  </div>
                              </div>
                          </div>
                      </div>
                  </div>
              </div>

              <div class="card-footer">
                  <mat-paginator 
                      [length]="pageTotalLength"
                      [pageSize]="pageSize"
                      [pageSizeOptions]="pageSizeOptions"
                      [pageIndex]="currentPage"
                      (page)="onPageChange($event)"
                      aria-label="Select page">
                  </mat-paginator>
              </div>
          </div>
      </div>
  </div>
</div>






import { Component, Input, ViewChild, AfterViewInit} from '@angular/core';
import { BackendapiService } from '../backendapi.service';
import { Task, DB_Country } from '../dashboard-view/dashboard-view.component';
import { ActivatedRoute } from '@angular/router';
import { FormBuilder, FormControl, FormGroup } from '@angular/forms';
import { MRARequest, MRARequestCompanyTotalPayload, MRARequestPayload, MRARequestPayloadInput, MRARequestTotalResponse, MRARequestStatusInput, MRARequestTableOutput, Country, MRARequestFilters, ApprovalOverrideInput, EditMRARequestInput, MRARequestCompanyLocation, MRARequestCompanyTableOutput } from './mra-request-types';
import { LoadingService } from '../services/loading.service';
import { UtilsService } from '../services/utils.service';
import { MatDialog, MatDialogRef } from '@angular/material/dialog';
import { MatSnackBar } from '@angular/material/snack-bar';
import { CompanyMraDlgComponent } from '../company-mra-dlg/company-mra-dlg.component';
import { MraRequestModalComponent } from './dialog/mra-request-modal/mra-request-modal.component';
import { ViewLocationComponent } from '../utils/view-location/view-location.component';
import { CompanyLocation } from '../company-full-view/companies-types';
import { emptyMraRequestTableOutput, emptyMraRequestTotalResponse } from './mra-request-models';
import { MatPaginator, PageEvent } from '@angular/material/paginator';

export const MRAList = [
	{
		name: 'Approved',
		value: 'APPROVED',
		class: 'approved'
	},
	{
		name: 'Rejected',
		value: 'REJECTED',
		class: 'rejected'
	},
	{
		name: 'Pending Approval',
		value: 'PENDING_APPROVAL',
		class: 'pending'
	},
	{
		name: 'Pending Review',
		value: 'PENDING_REVIEW',
		class: 'pending'
	},
	{
		name: 'Suspended',
		value: 'SUSPENDED',
		class: 'rejected'
	},
	{
		name: 'Removed',
		value: 'REMOVED',
		class: 'rejected'
	}
]

export type SortState = {
	column: string;
	direction: 'asc' | 'desc' | '';
}



@Component({
	selector: 'app-mra-request-full-view',
	templateUrl: './mra-request-full-view.component.html',
	styleUrls: ['./mra-request-full-view.component.scss']
})
export class MRARequestFullViewComponent implements AfterViewInit {
	@ViewChild(MatPaginator) paginator!: MatPaginator;
	MRAList: MRARequestStatusInput[] = MRAList;
	defaultSelectStatus: string[] = this.MRAList.map((mraStatus) => mraStatus.value)

	defaultSelectCountry: string[] = [];
	statusesForm: FormControl = new FormControl(this.defaultSelectStatus);
	countriesForm: FormControl = new FormControl(this.defaultSelectCountry);
	@Input() searchItems: MRARequestFilters = {
		request_id: '',
		company: '',
		tin: '',
		g2g_id: '',
		countries: this.defaultSelectCountry,
		status: this.defaultSelectStatus,
	};

	companyName = '';
	countryName = '';

	submissionDialog? : MatDialog; 

	constructor(private backendApi: BackendapiService, private route: ActivatedRoute, private fb: FormBuilder,
		private snackBar: MatSnackBar,
		private dialogModal: MatDialog, 
		private loader : LoadingService, private utils : UtilsService) {
	}

	getThisCountryNameFromISO(countryIso: string) {
		return this.utils.translateCodeToCountry(countryIso);
	}


	mainCountries: Country[] = [];
	searching: string = '';
	sortState: SortState = { column: '', direction: '' };


	ngOnInit() {
		this.statusesForm.valueChanges.subscribe((status) => {
			this.searchItems.status = status
			this.filterResults('status', status); 
		})
		this.countriesForm.valueChanges.subscribe((countries) => {
			this.searchItems.countries = countries
			this.filterResults('country', countries); 
		})
		this.loader.searchFilter.subscribe((filter)=>{
			console.log("Filter for MRA Request: ", filter)
			if (filter!==null) {
				this.searching = 'Searching...'; // starts the searching modal
				this.doSearch(filter); // does the search filter, then it resets the search filter 
				this.loader.searchFilter.next(null); 
			}
		})

	}

	pageTotalLength = 0;
	pageSize = 15;
	pageSizeOptions = [15, 25, 50];
	currentPage = 0;
	_mraRequestsList: MRARequestTableOutput[] = [];

	ngAfterViewInit() {
		// Configure the paginator
		this.paginator.pageSize = this.pageSize;
		this.paginator.pageSizeOptions = this.pageSizeOptions;
		this.paginator.length = this.pageTotalLength;

		// Subscribe to page changes
		this.paginator.page.subscribe((event: PageEvent) => {
			this.onPageChange(event);
		});
	}

	onPageChange(event: PageEvent) {
		this.currentPage = event.pageIndex;
		this.pageSize = event.pageSize;
		this.updateDisplayedRequests();
	}

	private updateDisplayedRequests() {
		const startIndex = this.currentPage * this.pageSize;
		const endIndex = startIndex + this.pageSize;
		this.filteredMraRequests = this._mraRequestsList.slice(startIndex, endIndex);
		console.log('Pagination state:', {
			currentPage: this.currentPage,
			pageSize: this.pageSize,
			totalItems: this.pageTotalLength,
			displayedItems: this.filteredMraRequests.length,
			allItems: this._mraRequestsList.length
		});
	}

	expandedRows : number [] = [];
	addressModal!: MatDialogRef<ViewLocationComponent>;
	mraRequestCompanies: MRARequestCompanyTableOutput [] = [];
	filteredMraRequestCompanies : MRARequestCompanyTableOutput [] = [] 
	mraRequestMap : Map<string, MRARequestTableOutput> = new Map(); // this is a unique map of the mra requests 
	filteredMraRequests : MRARequestTableOutput [] = []; 
	mraRequestHeaders: string[] = ['index', 'request_id', 'name', 'country', 'count', 'date_updated', 'flags']
	// creates the initial mapping for MRA Requests
	createMRARequestMapping () {
		let mraRequestIndex = 1; 
		// sets all the mra requests
		for (let mraRequestCompany of this.mraRequestCompanies) {
			let mraRequest = this.mraRequestMap.get(mraRequestCompany.request_id); 
			if (!mraRequest) {
				this.mraRequestMap.set(mraRequestCompany.request_id, 
					{
						index: mraRequestIndex++, 
						request_id : mraRequestCompany.request_id,
						aeo_name: mraRequestCompany.request.mraRqst.aeoPgmNm, 
						company_count: mraRequestCompany.request.mraRqst.cmpnyCt || 1, 
						country: mraRequestCompany.request.mraRqst.cntryCd || mraRequestCompany.request.mraRqst.hostCtry, 
						date_updated: mraRequestCompany.request.mraRqst.updt_dttm, 
						request: mraRequestCompany.request.mraRqst,
						status: {
							class: 'pending',
							value: 'PENDING'
						},
						flags: {
							mraStatus: ""  
						},
						companies: [mraRequestCompany]
					}
				)
			}
			else {
				this.mraRequestMap.set(mraRequestCompany.request_id, 
					{
						...mraRequest, 
						companies: [...mraRequest.companies, mraRequestCompany]
					}
				)
			}
		}
		// remaps the mra requests flags
		for (let [mraRequestID, mraRequest] of this.mraRequestMap) {
			let [rejectedCount, suspendedCount, removedCount] = mraRequest.companies.reduce(( counts : any, company : any)=>{
				counts[0] = company.status.value === "REJECTED" ? counts[0]+1 : counts[0]; 
				counts[1] = company.status.value === "SUSPENDED" ? counts[1]+1 : counts[1]; 
				counts[2] = company.status.value === "REMOVED" ? counts[2]+1 : counts[2]; 
				return counts;
			}, [0,0]);
			mraRequest.flags.mraStatus = `${rejectedCount > 0 ? `${rejectedCount} Companies Rejected`: ''}
			${suspendedCount > 0 ? `${suspendedCount} Companies Suspended`: ''}
			${removedCount > 0 ? `${removedCount} Companies Removed`: ''}`
			mraRequest.flags.mraStatus = mraRequest.flags.mraStatus.trim(); 
			this.mraRequestMap.set(mraRequestID, mraRequest); 
		}
		console.log("MRA Request Map: ", this.mraRequestMap);
	}
	generateStatusTooltip (tooltip : string) {
		return tooltip; 
	}
	// creates a list of mra requests based on the current filtered list
	createFilteredMraRequestList () {
		let filteredRequestIDs = this.filteredMraRequestCompanies.reduce((request_ids : string [], company : MRARequestCompanyTableOutput)=>{
			return request_ids.includes(company.request_id) ? request_ids : [...request_ids, company.request_id]
		}, [])
		this.filteredMraRequests = filteredRequestIDs.map((request_id)=>{
			return this.mraRequestMap.get(request_id) || emptyMraRequestTableOutput
		}) || []; 

	}
	createAddress(location : CompanyLocation | MRARequestCompanyLocation) {
		// Type guard to determine which type of location this is
		if ('cmpnyAddr' in location || 'rolList' in location) {
			// It's an MRARequestCompanyLocation
			return this.utils.convertLocationConversion(location as any, 'mra-request-company');
		} else {
			// It's a CompanyLocation
			return this.utils.convertLocationConversion(location, 'company');
		}
	}
	openAddressModal(company: MRARequestCompanyTableOutput, location: MRARequestCompanyLocation) {
	  this.addressModal = this.dialogModal.open(ViewLocationComponent, {
		width: '75%',
		height: '75%',
		data: {
		  name: company.company_name,
		  country: company.country,
		  address: this.utils.convertLocationConversion(location, 'mra-request-company'),
		  locationFound: false,
		  location: [0, 0]
		}
	  })
	}
	toggleEditMode (mraRequest: MRARequestCompanyTableOutput,  mode : string, locationIndex : number) {
		// rearranged to make them emppty
		switch (mode) {
			case 'edit': 
			// they will be binded to original
				mraRequest.edit.payload = {
					comment: '', 
					tin: mraRequest.request.mraRqstCmpny.tin, 
					name: mraRequest.company_name, 
					roleList: mraRequest.request.mraRqstCmpny.rolList
				}; 
				break;
			case 'override': 
				mraRequest.edit.payload = {
					status: '', 
					comment: ''
				}; 
				break;
			case '':
				mraRequest.edit.payload = undefined; 
				break; 
		}
		if (mode === 'location') {
			// will do seperate processing if the mode turns out to be location
		}
		else {
			mraRequest.edit.mode = mraRequest.edit.mode === mode ? '' : mode
			mraRequest.edit.error = ''; // clears at any error messages
			this.updateMainTable(mraRequest); // updates mra request to make sure everything is updated
		}
	}
	// this is the save editting process
	saveEdit (mraRequest : MRARequestCompanyTableOutput, locationIndex : number) {
		// for each mode, it will submit backend calls or do verification beforehand
		let payload = mraRequest.edit.payload
		let mode = mraRequest.edit.mode; 
		let errors : string [] = this.verifyPayload(mraRequest, payload, mode); 
		if (errors.length>0) {
			mraRequest.edit.error = errors.join("\n");
		}
		else {
			// once verification is complete, it could do backend calls
			switch (mraRequest.edit.mode) {
				case 'edit':
					let currentEditPayload : EditMRARequestInput = mraRequest.edit.payload as EditMRARequestInput
					this.backendApi.editCompany({
					}).subscribe({next: (res)=>{
						this.toggleEditMode(mraRequest, '', 0) // returns back to normal
					}, error: (err)=>{
						console.error("Error occured when saving: ", err)
					}})
					console.log("Payload for edit: ", payload) 
					break; 
				case 'override':
					let currentOverridePayload : ApprovalOverrideInput = mraRequest.edit.payload as ApprovalOverrideInput
					this.backendApi.overrideApprovalStatus({
						mraRqstId: mraRequest.request_id,
						tin: mraRequest.request.mraRqstCmpny.tin,
						apprvlStusCd: currentOverridePayload.status,
						cmt: currentOverridePayload.comment
					}).subscribe({next: (res : any)=>{
						mraRequest.status = this.MRAList.find((status)=>{
							return status.value === res.mraRqstCmpnyStatus.apprvlStusCd
						}) || mraRequest.status;
						this.snackBar.open(
							`Approval status sent successfully.`,
							'Close',
							{
							  duration: 5000,
							  panelClass: 'errorSnack',
							}
						  );
						this.toggleEditMode(mraRequest, '', 0) // returns back to normal
					}, error: (err)=>{
						console.error("Error occured when saving: ", err)
					}})
					break; 
				case 'location': 
					console.log("location information updated")
					break;
			}
		}
		// updates the main request table at the end
	}
	// makes sure that the requests are updated
	updateMainTable (mraRequest : MRARequestCompanyTableOutput) {
		// copies the edit to the main mra request list  
		let mraRequestFromTotalIndex = this.mraRequestCompanies.findIndex((request : MRARequestCompanyTableOutput)=>{
			return request.index === mraRequest.index
		}) 
		if (mraRequestFromTotalIndex >= 0) {
			this.mraRequestCompanies[mraRequestFromTotalIndex].edit = mraRequest.edit; 
		} 
	}
	// ensures that the payload is good 
	verifyPayload (original: MRARequestCompanyTableOutput, payload : any, mode : string) : string [] {
		let results : string [] = []
		console.log("Payload being screened")
		switch (mode) {
			case 'override':
				if (payload.comment.length===0) {
					results.push("Comments are currently empty. Changes require comments to be added.")
				}
				if (payload.status.length===0) {
					results.push("Status for override is currently empty. Status would need to be filled in to make a change.")
				}
				if (payload.status && original.status.value === payload.status) {
					results.push("Status for override is the same as the original. Status would need to be different to submit a change.")
				}
				break;  
			case 'edit':
				// check for empties
				if (payload.comment.length===0) {
					results.push("Comments are currently empty. Changes require comments to be added.")
				}
				if (payload.tin.length===0) {
					results.push("TIN input is currently empty. TIN would need to be filled.")
				}
				if (payload.name.length===0) {
					results.push("Company Name is currently empty. Company name would need to be filled.")
				}
				break; 
		}
		return results
	}
	toggleRow (index : number | undefined) {
		if (index === undefined) return;
		let searchIndex = this.expandedRows.indexOf(index); 
		if (searchIndex>=0) {
			this.expandedRows.splice(searchIndex, 1); 
		}
		else {
			this.expandedRows.push(index);
		}
	}
	rowExpanded (index : number | undefined) {
		if (index === undefined) return false;
		return this.expandedRows.includes(index); 
	}
	// Type guard for EditMRARequestInput
	isEditPayload(payload: EditMRARequestInput | ApprovalOverrideInput | undefined): payload is EditMRARequestInput {
		return payload !== undefined && 'name' in payload && 'tin' in payload;
	}
	// Type guard for ApprovalOverrideInput
	isOverridePayload(payload: EditMRARequestInput | ApprovalOverrideInput | undefined): payload is ApprovalOverrideInput {
		return payload !== undefined && 'status' in payload && !('name' in payload);
	}
	// Helper methods for template type narrowing
	getEditPayload(company: MRARequestCompanyTableOutput): EditMRARequestInput | undefined {
		return this.isEditPayload(company.edit.payload) ? company.edit.payload : undefined;
	}
	getOverridePayload(company: MRARequestCompanyTableOutput): ApprovalOverrideInput | undefined {
		return this.isOverridePayload(company.edit.payload) ? company.edit.payload : undefined;
	}
	filtersToLook : string [] = ['request_id', 'name', 'g2g_id', 'country', 'status', 'tin']
	doSearch(filter: MRARequestCompanyTotalPayload) {
		console.log("Search Items: ", this.searchItems);
		console.log(filter);
		this.backendApi.getMraRequestFromSearch(filter, {
			totalNumberOfElements: 0,
			totalPages: 0,
			currentPage: 0
		}).subscribe({next: (mraRequests: MRARequestTotalResponse[]) => {
			this.searching = 'Loading Results...';
			console.log("MRA Ruquest Total Response: ", mraRequests)
			let unique_countries: string[] = [];
			let mraRequestCompaniesTable: MRARequestCompanyTableOutput[] = mraRequests.map((mraRequest: MRARequestTotalResponse, index: number) => {
				let status = mraRequest.mraRqstCmpny.apprvlStusCd.includes("PENDING") ?
					"pending" : mraRequest.mraRqstCmpny.apprvlStusCd.includes("APPROVED") ?
						"approved" : "rejected";
				let country = mraRequest.mraRqst.cntryCd || mraRequest.mraRqst.hostCtry;
				if (!unique_countries.includes(country)) {
					unique_countries.push(country);
				}
				return {
					index: index + 1,
					request_id: mraRequest.mraRqst.mraRqstId,
					tin: mraRequest.mraRqstCmpny.tin, 
					roleList: mraRequest.mraRqstCmpny.rolList?.split(",") || [], 
					g2g_id: mraRequest.mraRqstCmpny.g2gCmpnyId,
					company_name: mraRequest.mraRqstCmpny.cmpnyNm,
					country: mraRequest.mraRqst.cntryCd || mraRequest.mraRqst.hostCtry,
					status: {
						class: status,
						value: mraRequest.mraRqstCmpny.apprvlStusCd.replaceAll("_"," ")
					},
					approvalStatusDate: mraRequest.mraRqstCmpny.apprvlStusDttm,
					updatedDate: mraRequest.mraRqstCmpny.apprvlStusDttm,
					locations: mraRequest.mraRqstCmpny.mraRqstCmpnyLocList,
					monetaryValue: 0,
					mainAddress: '',
					containers: 0,
					request: mraRequest,
					edit: {
						mode: '',
						error: '',
						disabled: false,
						locationEdit: []
					}
				}
			}) || [];
			this.mraRequestCompanies = mraRequestCompaniesTable;
			this.mainCountries = unique_countries.map((country: string) => {
				return {
					name: this.utils.translateCodeToCountry(country),
					code: country,
				}
			});
			this.searching = '';
			this.createMRARequestMapping();
			this.filterResults('new_search', []);

		}, error: ()=>{
			this.searching = 'error'
		}});
	}
	filterUpdate = (type : string, filterValue : string) => {
		// filters based on the results change of the filter 
		this.filterResults(type, filterValue);
	}
	modal!: MatDialogRef<CompanyMraDlgComponent>;
	openModal = (mraRequestCompany : MRARequestCompanyTableOutput) => {
		// checks if it's normal view mode
		if (mraRequestCompany.edit.mode=='') {
			this.modal = this.dialogModal.open(CompanyMraDlgComponent, {
				height: '90%', 
				width: '70%', 
				data: {
					mraRequestCompany: mraRequestCompany
				}
			})
		}
	}
	filterResults = (filter: string, currentValue: string | string[]) => {
		this.filteredMraRequestCompanies = filter !== 'new_search' ? this.mraRequestCompanies.filter((mraRequest) => {
			let allFilterConditions: boolean[] = [
				mraRequest.request_id.toLowerCase().includes(this.searchItems.request_id.toLowerCase()),
				mraRequest.company_name.toLowerCase().includes(this.searchItems.company.toLowerCase()),
				mraRequest.g2g_id.toLowerCase().includes(this.searchItems.g2g_id.toLowerCase()),
				this.searchItems.countries.includes(mraRequest.country) || this.searchItems.countries.length === 0,
				this.searchItems.status.includes(mraRequest.status.value) || this.searchItems.status.length === 0,
				mraRequest.request.mraRqstCmpny.tin.toLowerCase().includes(this.searchItems.tin.toLowerCase())
			];
			let value: any = currentValue;
			let filterIndex = this.filtersToLook.indexOf(filter);
			let condition = true;

			switch (filter) {
				case 'request_id':
					condition = mraRequest.request_id.toLowerCase().includes(value.toLowerCase());
					break;
				case 'name':
					condition = mraRequest.company_name.toLowerCase().includes(value.toLowerCase());
					break;
				case 'g2g_id':
					condition = mraRequest.g2g_id.toLowerCase().includes(value.toLowerCase());
					break;
				case 'country':
					condition = value.includes(mraRequest.country) || value.length === 0;
					break;
				case 'status':
					condition = value.includes(mraRequest.status.value) || value.length === 0;
					break;
				case 'tin':
					condition = mraRequest.request.mraRqstCmpny.tin.toLowerCase().includes(value.toLowerCase());
					break;
			}
			if (filterIndex > 0) {
				allFilterConditions[filterIndex] = condition;
			}
			return allFilterConditions.reduce((totalCondition, condition) => {
				return totalCondition && condition;
			}, true);
		}) : this.mraRequestCompanies;

		this.createFilteredMraRequestList();
		this._mraRequestsList = [...this.filteredMraRequests];
		this.pageTotalLength = this._mraRequestsList.length;
		this.currentPage = 0;
		this.updateDisplayedRequests();
	}

	showCompanyView(countryName: string, companyName: string) {
		this.closeCompanyView();
		this.companyName = companyName;
		this.countryName = countryName;
	}

	closeCompanyView() {
		this.companyName = '';
		this.countryName = '';
	}

 // Testing Area Change when needed

 	addStatusClassToTextField(status: string, requestId: string) {
		// Find the specific text field wrapper using the unique request ID
		// Using a more specific selector to ensure we only get the direct child wrapper
		const textFieldWrapper = document.querySelector(`#status-field-${requestId} .mat-mdc-text-field-wrapper.mdc-text-field.mdc-text-field--filled`);
		
		if (textFieldWrapper) {
			// Remove existing status classes from this specific field
			textFieldWrapper.classList.remove('approved', 'rejected', 'pending');

			// Add the new status class
			if (status) {
				const selectedStatus = this.MRAList.find(s => s.value === status);
				if (selectedStatus) {
					textFieldWrapper.classList.add(selectedStatus.class);
				}
			}
		}
	}

	getStatusFieldId(requestId: string): string {
		return `status-field-${requestId}`;
	}

	getStatusSelectId(requestId: string): string {
		return `status-select-${requestId}`;
	}

	getLocationRole(location:MRARequestCompanyLocation) {
		if (!location.rolList) {
			return '';
		}
		return location.rolList.split(',').join(', ');
	}


	sort (column: string) {
		console.log('Sorting by column:', column);
		if (this.sortState.column === column) {
			this.sortState.direction = this.sortState.direction === 'asc' ? 'desc' :
			this.sortState.direction === 'desc' ? '' : 'asc' ;
		} else {
			this.sortState.column = column;
			this.sortState.direction = 'asc';
		}

		if (!this.sortState.direction) {
			this.filteredMraRequests = [...this._mraRequestsList];
			this.applyFilters();
		} else {
			this.applySorting();
		}
	}

	private applySorting() {
		console.log('Applying sort:', this.sortState);
		if (!this.sortState.column || !this.sortState.direction) {
			return;
		}

		const sortRequests = [...this.filteredMraRequests].sort((a, b) => {
			let valueA: any;
			let valueB: any;

			switch (this.sortState.column) {
				case 'request_id':
					valueA = a.request_id;
					valueB = b.request_id;
					break;
				case 'name':
					valueA = a.aeo_name;
					valueB = b.aeo_name;
					break;
				case 'country':
					valueA = this.getThisCountryNameFromISO(a.country);
					valueB = this.getThisCountryNameFromISO(b.country);
					break;
				case 'count':
					valueA = a.company_count;
					valueB = b.company_count;
					break;
				case 'date_updated':
					valueA = a.date_updated;
					valueB = b.date_updated;
					break;
				case 'index':
					valueA = a.index;
					valueB = b.index;
					break;
				default:
					return 0;
			}

			if (typeof valueA === 'string') {
				valueA = valueA.toLowerCase();
				valueB = valueB.toLowerCase();
			}

			if (valueA === null || valueA === undefined) valueA = '';
			if (valueB === null || valueB === undefined) valueB = '';

			if (this.sortState.direction === 'asc') {
				return valueA < valueB ? -1 : valueA > valueB ? 1 : 0;
			} else {
				return valueA > valueB ? -1 : valueA < valueB ? 1 : 0;
			}
		});

		this.filteredMraRequests = sortRequests;
	}

	private applyFilters() {
		this.createFilteredMraRequestList();
	}

	getSortIcon(column: string): string {
		if (this.sortState.column !== column) {
			return 'unfold_more';
		}

		return this.sortState.direction === 'asc' ? 'arrow_upward' :
		this.sortState.direction === 'desc' ? 'arrow_downward' : 'unfold_more';
	}

	exportToJSON() {
		const jsonString = JSON.stringify(this._mraRequestsList, null, 2);
		const blob = new Blob([jsonString], {type:'application/json'});
		const url = URL.createObjectURL(blob);

		const a = document.createElement('a');
		a.href = url;
		a.download = 'mra_request_companies_data.json';
		a.click();
		URL.revokeObjectURL(url);
	}
}
