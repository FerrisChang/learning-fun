/* Shared styling pulled from companies-table for consistent look-and-feel */
.companies-table-frame {
  display: flex;
  flex-direction: column;
  margin-left: 2.5%;
  width: 95%;
  height: 100%;
}

.filters-container {
  display: flex;
  flex-direction: column;
  gap: 1em;
  width: 95%;
  margin-left: 2.5%;
}

.filters-row {
  display: flex;
  gap: 1em;
  flex-wrap: wrap;
}

.search-form-field {
  flex: 1;
  min-width: 220px;
  max-width: 320px;
}

.table {
  flex: 9;
  max-height: 100%;
  overflow-y: auto;
  margin-bottom: 0;
}

.grid-container {
  display: flex;
  flex-direction: column;
  width: 100%;
  min-width: 0;
}

.grid-header {
  display: grid;
  grid-template-columns: minmax(50px, 0.5fr) minmax(150px, 2fr) minmax(150px, 2fr) minmax(120px, 1.5fr) minmax(120px, 1.3fr) minmax(200px, 2.5fr) minmax(150px, 1.5fr) minmax(160px, 1.5fr);
  gap: 1px;
  background-color: #e0e0e0;
  position: sticky;
  top: 0;
  z-index: 10;
  font-weight: 600;
}

.grid-row-wrapper {
  display: flex;
  flex-direction: column;
  width: 100%;
}

.grid-row {
  display: grid;
  grid-template-columns: minmax(50px, 0.5fr) minmax(150px, 2fr) minmax(150px, 2fr) minmax(120px, 1.5fr) minmax(120px, 1.3fr) minmax(200px, 2.5fr) minmax(150px, 1.5fr) minmax(160px, 1.5fr);
  gap: 1px;
  background-color: #e0e0e0;
  cursor: pointer;
  text-align: center;
  font-size: 1.05em;
  transition: background-color 0.2s;
}

.grid-row:hover {
  background-color: rgb(184, 181, 181);
}

.grid-row.selected {
  background-color: rgb(227 237 246);
}

.grid-cell {
  background-color: white;
  padding: 0.75em;
  display: flex;
  align-items: center;
  justify-content: center;
  word-wrap: break-word;
  overflow-wrap: break-word;
  overflow: hidden;
  text-overflow: ellipsis;
  min-height: 3em;
  min-width: 0;
}

.grid-header .grid-cell {
  background-color: #f5f5f5;
  font-weight: 600;
  padding: 1em 0.75em;
  text-align: center;
}

@media (max-width: 1400px) {
  .grid-header,
  .grid-row {
    grid-template-columns: minmax(40px, 0.4fr) minmax(120px, 1.8fr) minmax(120px, 1.6fr) minmax(100px, 1.2fr) minmax(90px, 1fr) minmax(150px, 2fr) minmax(130px, 1.2fr) minmax(140px, 1.2fr);
  }
}

@media (max-width: 992px) {
  .filters-row {
    flex-direction: column;
  }

  .grid-header,
  .grid-row {
    grid-template-columns: minmax(35px, 0.3fr) minmax(110px, 1.5fr) minmax(110px, 1.3fr) minmax(90px, 1.1fr) minmax(75px, 0.8fr) minmax(130px, 1.5fr) minmax(120px, 1.1fr) minmax(120px, 1.1fr);
  }

  .grid-cell {
    font-size: 0.95em;
    padding: 0.6em;
  }
}

@media (max-width: 768px) {
  .table {
    overflow-x: auto;
  }

  .grid-header,
  .grid-row {
    grid-template-columns: minmax(30px, 0.25fr) minmax(100px, 1.3fr) minmax(100px, 1.2fr) minmax(80px, 0.9fr) minmax(70px, 0.7fr) minmax(110px, 1.3fr) minmax(100px, 1fr) minmax(100px, 1fr);
  }

  .grid-cell {
    font-size: 0.9em;
    padding: 0.5em;
  }
}

@media (max-width: 576px) {
  .filters-container {
    width: 100%;
    margin-left: 0;
  }

  .grid-header,
  .grid-row {
    grid-template-columns: minmax(25px, 0.2fr) minmax(90px, 1fr) minmax(90px, 1fr) minmax(70px, 0.8fr) minmax(60px, 0.7fr) minmax(90px, 1.1fr) minmax(80px, 0.9fr) minmax(80px, 0.9fr);
  }

  .grid-cell {
    font-size: 0.85em;
    padding: 0.4em;
  }
}

#map {
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  height: 460px;
}

.text-header.clickable:hover {
  text-decoration: underline;
  color: rgb(0, 60, 100);
  font-weight: 550;
}

.mat-expansion-panel-header-title {
  font-size: 1.2em
}

.details-frame {
  flex: 1;
  padding-bottom: 1em;
}

.information.details-row {
 display: grid !important; 
 grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
 gap: 10px; 
}



.details-row {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding-bottom: 1em;
}


.flag {
 margin-right: 1em;
 align-items: center;
}
.error-search-frame {
 margin-left: 1.75em;
}

.flag-detail {
 width: 3em;
 height: 3em;
}
::ng-deep .status-tooltip {
 white-space: pre-line;
}
.details-frame-1 {
  flex: 7;
  display: flex;
  flex-direction: column
}

.auto-vetting-row {
  margin-top: 1em;
}

.edit.details-row {
  display: flex;
  flex-direction: row;
  gap: 2em;
  margin-top: 1em;
}

.edit-frame {
  margin-left: 1.5em;
  display: flex;
  gap: 1em;
}

.edit-field-container {
  flex: 0;
  display: flex;
  flex-direction: column;
  margin-top: 1em;
}

.edit-field-container>.details-header {
  width: 100%;
}

.details-field-container {
  flex: 1;
  display: inline-flex;
  margin-top: 1em;
}

.details-expansion-header {
  font-size: 1.2em;
}

.details-wrapper-header {
  flex: 1;
  cursor: pointer;
  margin-left: 0.25em;
  font-weight: 550;
  font-size: 1.1em;
  display: flex;
  gap: 2em;
}

.details-wrapper-header>.right-header {
  flex: 3;
}

.details-header {
  margin-left: 0.25em;
  font-weight: 550;
  font-size: 1em;
}

.details-field {
  flex: 1;
  margin-left: 0.5em;
  font-size: 1.1em;
}

.additional-action-item:hover,
.additional-action-item>span:hover {
  font-weight: 550 !important;
}

.status-details-text {
  padding-left: 1em;
  padding-right: 1em;
  width: fit-content;
}

.override.details-row {
  display: flex;
  flex-direction: row;
  gap: 1.5em;
  margin-top: 1em;
  justify-items: center;
  justify-content: space-between;
  width: 100%;
}


.details-wrapper-header>div>.error-message {
  float: left;
  font-size: 1em;
  font-weight: 550;
  width: 75%;
  color: red;
}

.status-transformation-frame {
  display: flex;
  gap: 1em;
}

.comment-frame {
  flex: 2;
}

.comment-frame>.comment {
  width: 90%;
  font-size: 1.2em;
  border-radius: 3px;
  height: 6em !important;
}

.comment-frame>.comment.error {
  border-color: red;
  border-width: 2px;
}

.details-row>.status-frame {
  flex: 2;
  display: flex;
  flex-direction: column;
  margin-left: auto;
  margin-right: auto;
}

.status-header {
  font-weight: 550;
  margin-left: auto;
  margin-right: auto;
}

.current-status {
  margin-left: auto;
  margin-right: auto;
}
.current-status-container {
 width: 100%; 
 height: 4em; 
 display: flex;
}
.current-status-container>.status-text {
 margin: auto;
 font-size: 1.25em; 
 color: black; 
 text-align: center;
}
.current-status-container.approved {
 background-color: var(--approved-container-background);
}
.current-status-container.rejected {
 background-color: var(--rejected-container-background);
}
.current-status-container.pending {
 background-color: var(--pending-container-background);
}

.details-row>.transformation {
  flex: 1;
  display: flex;
}

.transformation-icon {
  margin-left: auto;
  margin-top: 1em;
  margin-right: auto;
  transform: scale(2);
}

.status-selection {
  width: 80%;
}

.mra-status-option.approved {
  text-align: center;
  background-color: var(--approved-container-background) !important;
  color: black;
}

.mra-status-option.approved.mdc-list-item--selected>span {
  font-weight: 550;
  color: black !important;
}

.mra-status-option.rejected {
 background-color:  var(--rejected-container-background) !important;
 color: black;
}

.mra-status-option.pending {
 background-color: var(--pending-container-background) !important;
 color: black;
}

.details-primary-location {
  width: fit-content;
  padding-right: 1.5em;
  padding-left: 1.5em;
}

.details-location-frame {
  display: flex;
  gap: 0.5em;
}

.table-details-wrapper {
  overflow: hidden;
  display: flex;
  flex-direction: column;
  height: 0;
  opacity: 0;
  width: 100%;
  transition: 0.2s ease-out;
  padding-bottom: 0.5em;
}

.table-details-wrapper.expanded {
  opacity: 1;
  padding-top: 1em;
  padding-bottom: 2em;
  height: auto;
}

.table-details-wrapper.expanded:hover {
  background-color: white !important;
}

.location-detail {
 padding: 0.75em 0.75em 0.75em 1em; 
  flex: 1;
  border: 3px solid black;
}

.location-detail>.flex-row>.details-field-container {
  display: flex;
  flex: 1;
  flex-wrap: wrap;
}
.location-detail>.flex-row>.details-field-container>div {
 flex: 0 1 auto;
}

.location-detail>.flex-row {
  display: flex;
  gap: 0.5em;
}

.primaryLocation {
  border: 3px solid #198754 !important;
}

.location-header {
  font-size: 1.3em;
  font-weight: 550;
  margin-bottom: 0.5em;
  width: fit-content;
  cursor: pointer;
}

.location-header:hover {
  text-decoration: underline;
  color: rgb(0, 58, 106);
}

.bolded.location-header {
  font-weight: 700;
}

.details-expansion-header:hover {
  background-color: whitesmoke !important;
}

.approved,
.true {
  background-color: #198754;
}

.pending {
  background-color: rgb(239, 255, 170);
}

.rejected,
.false {
  background-color: #dc3545;
}

.table-cell {
  width: 40%;
  height: 80%;
  text-align: center;
}

body {
  min-height: 100vh;
  min-height: -webkit-fill-available;
}

html {
  height: -webkit-fill-available;
}

main {
  height: 100vh;
  height: -webkit-fill-available;
  max-height: 100vh;
  overflow-x: auto;
  overflow-y: hidden;
}

.dropdown-toggle {
  outline: 0;
}

.btn-toggle {
  padding: .25rem .5rem;
  font-weight: 600;
  color: var(--bs-emphasis-color);
  background-color: transparent;
}

.btn-toggle:hover,
.btn-toggle:focus {
  color: rgba(var(--bs-emphasis-color-rgb), .85);
  background-color: var(--bs-tertiary-bg);
}


.btn-toggle[aria-expanded="true"] {
  color: rgba(var(--bs-emphasis-color-rgb), .85);
}

.btn-toggle[aria-expanded="true"]::before {
  transform: rotate(90deg);
}

.btn-toggle-nav a {
  padding: .1875rem .5rem;
  margin-top: .125rem;
  margin-left: 1.25rem;
}

.btn-toggle-nav a:hover,
.btn-toggle-nav a:focus {
  background-color: var(--bs-tertiary-bg);
}

.scrollarea {
  overflow-y: auto;
}


.navbar-svg-icon {
  position: relative;
  top: -2px;
  margin-right: 5px
}

.navbar-svg-icon-inactive {
  position: relative;
  margin-right: 5px
}

.btn-filter {
  box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
  ;
}

.navbar-btn-hover:hover {
  background-color: rgb(225, 236, 247);
  border-radius: 5px;

}


.btn-filter:hover {
  color: rgb(8, 67, 230);
}

mat-sidenav {
  width: 350px;
}

.widget-card {
  box-shadow: 2px 2px 4px 1px rgba(0, 0, 0, 0.1);
  border-color: rgb(142, 153, 163);
}


.card-hdr-class {
  background: rgb(0, 60, 110);
  color: white;
}

.example-accordion {
  display: block;
  max-width: 500px;
}

.example-accordion-item {
  display: block;
  border: solid 1px #ccc;
}

.example-accordion-item+.example-accordion-item {
  border-top: none;
}

.example-accordion-item-header {
  display: flex;
  align-content: center;
  justify-content: space-between;
}

.example-accordion-item-description {
  font-size: 0.85em;
  color: #999;
}

.flag-input {
  width: 1.25em;
  margin-right: 0.5em;
  height: 1.25em;
}

.example-accordion-item-header,
.example-accordion-item-body {
  padding: 16px;
}

.example-accordion-item-header:hover {
  cursor: pointer;
  background-color: #eee;
}

.example-accordion-item:first-child {
  border-top-left-radius: 4px;
  border-top-right-radius: 4px;
}

.example-accordion-item:last-child {
  border-bottom-left-radius: 4px;
  border-bottom-right-radius: 4px;
}

.input-container {
  width: 95%;
}

.filters-row {
  display: flex;
  gap: 1em;
}

.search-form-field {
  flex: 1;
  margin-left: 1em;
  margin-right: 1em;
}

.mat-expansion-panel-header-title {
  font-size: 1.1em
}

.details-expansion-header {
  padding-left: 0.5;
}
.company-frame {
 display: flex; 
 width: 100%; 
}
.company-frame>.actions-frame {
 flex: 1; 
 display: flex; 
 flex-direction: column;
 gap: 1em;
}
.company-frame>.content {
 flex: 4; 
 display: flex; 
 flex-direction: column; 
}
.additional-action-item {
 flex: 0; 
 padding-top: 1em; 
 padding-bottom: 1em; 
}

.mra-request-information-frame {
 display: flex; 
 flex-direction: column; 
 margin-left: 0.5em;
}
.mra-request-information-header {
 font-size: 1.2em; 
 font-weight: 550;
}
.mra-request-content {
 display: grid !important; 
 grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
 gap: 10px; 
 padding-bottom: 2em;
}

.mat-mdc-paginator {
  background-color: rgba(255, 255, 255, 0);
}

/* Testing Area Change when needed */
.mra-status-option {
  &.approved,
  &.rejected,
  &.pending {
    color: black !important
  }
  &.approved {
    background-color: #ddffef!important;
  }
  &.rejected {
    background-color: #ffd6da!important;
  }
  &.pending {
    background-color: #f2ffa8!important;
  }
}
.mra-status-option:hover {
  &.approved {
    color: #0026ff!important;
    background-color: #ffffff!important;
  }
  &.rejected {
    color: #0026ff!important;
    background-color: #ffffff!important;
  }
  &.pending {
    color: #0026ff!important;
    background-color: #ffffff!important;
  }
}

::ng-deep {
 .mat-mdc-text-field-wrapper.mdc-text-field.mdc-text-field--filled {
   &[class*="approved"] {
     background-color: #ddffef!important;
     .mat-mdc-select-value {
       font-weight: 550;
       color: #000!important;
     }
   }
   &[class*="rejected"] {
     background-color: #ffd6da!important;
     .mat-mdc-select-value {
       font-weight: 550;
       color: #000!important;
     }
   }
   &[class*="pending"] {
     background-color: #f2ffa8!important;
     .mat-mdc-select-value {
       font-weight: 550;
       color: #000!important;
     }
   }
 }
}

.sortable-header {
 cursor: pointer;
 user-select: none;
 
 &:hover {
   background-color: rgba(0,0,0,0.1);
 }

 .header-content {
   display: flex;
   align-items: center;
   gap: 4px;
 }

 .sort-icon {
   font-size: 18px;
   height: 18px;
   width: 18px;
   opacity: 0.5;
 }

 &:hover .sort-icon {
   opacity: 0.8;
 }
}





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
                                  <div class="grid-row {{expandedRows.includes(mraRequest.index) ? 'selected' : ''}}"
                                      (click)="toggleRow(mraRequest.index)">
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
                                              [ngClass]="mraRequest.status.class === 'approved' ? 'approved-text' : 
                                              mraRequest.status.class === 'rejected' ? 'rejected-text' : 'pending-text'">
                                              {{mraRequest.status.value}}
                                          </div>
                                      </div>
                                      <div class="grid-cell">{{mraRequest.date_updated}}</div>
                                  </div>

                                  <div class="table-details-wrapper" [class.expanded]="rowExpanded(mraRequest.index)">
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
                                                              {{company.request.mraRqstCmpny.updtDttm}}
                                                          </span>
                                                      </div>
                                                  </div>
                                                  <div class="edit details-row" *ngIf="company.edit.mode=='edit'">
                                                      <div class="edit-frame">
                                                          <div class="edit-field-container">
                                                              <mat-label class="details-header"
                                                                  [class.text-error]="company.edit.error.includes('name')">Company
                                                                  Name:</mat-label>
                                                              <span class="details-field">
                                                                  <mat-form-field class="edit-form-field">
                                                                      <mat-label>Company Name</mat-label>
                                                                      <input matInput type="text"
                                                                          [(ngModel)]="company.edit.payload.name">
                                                                      <ng-container *ngIf="company.edit.payload.name">
                                                                          <button matSuffix mat-icon-button
                                                                              aria-label="Clear"
                                                                              (click)="company.edit.payload.name=''"
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
                                                                          [(ngModel)]="company.edit.payload.tin">
                                                                      <ng-container *ngIf="company.edit.payload.tin">
                                                                          <button matSuffix mat-icon-button
                                                                              aria-label="Clear"
                                                                              (click)="company.edit.payload.tin=''"
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
                                                                          [(ngModel)]="company.edit.payload.roleList">
                                                                      <ng-container *ngIf="company.edit.payload.roleList">
                                                                          <button matSuffix mat-icon-button
                                                                              aria-label="Clear"
                                                                              (click)="company.edit.payload.roleList=''"
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
                                                              [(ngModel)]="company.edit.payload.comment"
                                                              placeholder="Enter comment here..." cdkTextareaAutosize
                                                              #autosize="cdkTextareaAutosize" cdkAutosizeMinRows="1"
                                                              cdkAutosizeMaxRows="6"></textarea>
                                                      </div>
                                                  </div>
                                                  <div class="override details-row" *ngIf="company.edit.mode=='override'">
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
                                                                      <mat-label *ngIf="!company.edit.payload.status" class="mra-status-label">MRA Request Statuses</mat-label>
                                                                      <mat-select [(value)]="company.edit.payload.status"
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
                                                              [(ngModel)]="company.edit.payload.comment"
                                                              placeholder="Enter comment here..." cdkTextareaAutosize
                                                              #autosize="cdkTextareaAutosize" cdkAutosizeMinRows="1"
                                                              cdkAutosizeMaxRows="6"></textarea>
                                                      </div>
                                                  </div>
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
