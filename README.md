
.companies-table-frame {
  display: flex;
  flex-direction: column;
  margin-left: 2.5%;
  width: 95%;
  height: 100%;
}

.filters {
  flex: 0;
  display: flex;
  gap: 1em
}

/* start of the tables field styling */ 
.table {
  flex: 9;
  max-height: 100%;
  overflow-y: auto;
  margin-bottom: 0;
}
.company-table-layout {
  height: 50em;
  display: flex; 
  gap: 2em;
}
.companies-table-frame {
  flex: 1;
  max-height: 100%;
  flex-direction: column; 
  display: flex
}
.search-form-field {
  height: 5em;
}

/* Grid Layout Styles */
.grid-container {
  display: flex;
  flex-direction: column;
  width: 100%;
  min-width: 0;
}

.flex-row {
  display: flex;
  flex-direction: row;
  gap: 1em;
}

.flex-column {
  display: flex;
  flex-direction: column;
}

.grid-header {
  display: grid;
  grid-template-columns: minmax(50px, 0.5fr) minmax(150px, 2fr) minmax(120px, 1.5fr) minmax(100px, 1fr) minmax(200px, 2.5fr) minmax(100px, 1.5fr) minmax(150px, 2fr) minmax(180px, 2fr);
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
  grid-template-columns: minmax(50px, 0.5fr) minmax(150px, 2fr) minmax(120px, 1.5fr) minmax(100px, 1fr) minmax(200px, 2.5fr) minmax(100px, 1.5fr) minmax(150px, 2fr) minmax(180px, 2fr);
  gap: 1px;
  background-color: #e0e0e0;
  cursor: pointer;
  text-align: center;
  font-size: 1.1em;
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

/* Responsive Grid Layout */
@media (max-width: 1400px) {
  .grid-header,
  .grid-row {
    grid-template-columns: minmax(40px, 0.4fr) minmax(120px, 1.8fr) minmax(100px, 1.2fr) minmax(80px, 0.8fr) minmax(150px, 2fr) minmax(80px, 1.2fr) minmax(120px, 1.5fr) minmax(140px, 1.5fr);
  }
}

@media (max-width: 1200px) {
  .grid-header,
  .grid-row {
    grid-template-columns: minmax(40px, 0.4fr) minmax(100px, 1.5fr) minmax(90px, 1fr) minmax(70px, 0.7fr) minmax(120px, 1.5fr) minmax(70px, 1fr) minmax(100px, 1.2fr) minmax(120px, 1.2fr);
  }
  
  .grid-cell {
    font-size: 0.95em;
    padding: 0.6em;
  }
}

@media (max-width: 992px) {
  .grid-header,
  .grid-row {
    grid-template-columns: minmax(35px, 0.3fr) minmax(90px, 1.2fr) minmax(80px, 0.9fr) minmax(60px, 0.6fr) minmax(100px, 1.2fr) minmax(60px, 0.8fr) minmax(90px, 1fr) minmax(100px, 1fr);
  }
  
  .grid-cell {
    font-size: 0.9em;
    padding: 0.5em;
  }
}

@media (max-width: 768px) {
  .grid-header,
  .grid-row {
    grid-template-columns: minmax(30px, 0.25fr) minmax(80px, 1fr) minmax(70px, 0.8fr) minmax(50px, 0.5fr) minmax(90px, 1fr) minmax(50px, 0.7fr) minmax(80px, 0.9fr) minmax(90px, 0.9fr);
  }
  
  .grid-cell {
    font-size: 0.85em;
    padding: 0.4em;
  }
}

@media (max-width: 576px) {
  .table {
    overflow-x: auto;
    overflow-y: auto;
  }
  
  .grid-container {
    min-width: 800px;
  }
  
  .grid-header,
  .grid-row {
    grid-template-columns: minmax(25px, 0.2fr) minmax(70px, 0.9fr) minmax(60px, 0.7fr) minmax(40px, 0.4fr) minmax(80px, 0.9fr) minmax(40px, 0.6fr) minmax(70px, 0.8fr) minmax(80px, 0.8fr);
  }
  
  .grid-cell {
    font-size: 0.8em;
    padding: 0.35em;
  }
  
  .filters {
    flex-direction: column;
    gap: 0.5em;
  }
  
  .search-form-field {
    height: auto;
  }
}

.details-frame {
  flex: 1;
}

.table-details-frame {
  height: 0;
  padding: 0;
}

.details-row {
  display: flex;
  flex-direction: column;
  width: 100%;
  padding-bottom: 0.5em;
}

.table-row.selected {
  background-color: rgb(227 237 246);
}

.table-row.selected>td {
  background-color: transparent;
}

.table-details-wrapper {
  overflow: hidden;
  display: flex;
  flex-direction: column;
  height: 0;
  opacity: 0;
  width: 100%;
  transition: 0.2s ease-out;
  background-color: white;
  padding: 0;
  margin: 0;
}

.table-details-wrapper.expanded {
  opacity: 1;
  padding-top: 1em;
  padding-bottom: 2em;
  padding-left: 1em;
  padding-right: 1em;
  height: auto;
  margin-top: 1px;
}

.table-details-header {
  flex: 1;
  display: flex;
}
.table-details-header>.details-text {
  font-weight: 500;
  font-size: 1.25em;
}
.table-details-header>.details-button {
  flex: 0;
  margin-left: auto;
  margin-right: 1em;
}
.details-row {
  display: flex;
  flex-direction: column;
  width: 100%;
}

.address-container {
  flex: 2;
  display: flex;
  gap: 1em;  
}
.address-container>.details-header {
  flex: 1;
}
.address-container>.details-field {
  flex: 2;
}
.details-frame-1 {
  flex: 7;
  display: flex;
  flex-direction: column;
  padding-bottom: 1em;
}

.flag {
  flex: 1;
  display: flex;
  align-items: center;
}

.flag-detail {
  height: 100%;
  flex: auto;
}
.details-field-container {
  flex: 1;
  display: inline-block;
  cursor: pointer;
  margin-top: 1em;
}
.address.details-field-container {
  display: inline-flex; 
  gap: 1em;
}
.details-header {
  font-size: 1em;
  font-weight: 500;
  padding-right: 1em;
}
.location-row-2>.details-field-container {
  display: flex;
  flex: 1;
  flex-wrap: wrap;
}
.location-row-2>.details-field-container>div {
  flex: 0 1 auto;
}
.location-row-2 {
  display: flex; 
  gap: 0.5em;
}

.details-field {
  font-size: 1em;
}
.details-location-frame {
  display: flex; 
  gap: 1em; 
  flex-wrap: wrap;
  justify-content: space-between;
}

.status-details-text {
  width: fit-content;
  align-self: center;
  display: inline-block;
}

.details-primary-location {
  width: fit-content;
  display: inline-flex; 
}
.details-primary-location>.details-header {
  padding-right: 0.5em; 
}
.primaryLocation {
  border: 3px solid #198754 !important; 
}
.location-detail {
  flex: 1;
  padding: 0.75em 0.75em 0.75em 1em; 
  border: 3px solid black;
}
.location-detail:hover {
  border-color: rgb(0, 58, 106) !important; 
  cursor:pointer;
}
.location-header {
  font-size: 1.2em; 
  font-weight: 550;
  width: fit-content; 
  margin-bottom: 0.25em;
  cursor: pointer;
}

.address.details-field-container {
  display: flex; 
}
.address.details-field-container>.details-field {
  flex: 2;
}

/* custom css for company use case */
.modal-opener {
  font-weight: 600;
  font-size: 1em;
  color: rgb(13, 110, 253);
  cursor: pointer;
} 
.mat-expansion-panel-header-title {
  font-size: 1.2em
}
.details-expansion-header {
  padding-left: 0; 
}


/* Sort CSS */
.sortable-header {
  cursor: pointer;
  user-select: none;
}

.sortable-header:hover {
  background-color: rgba(0,0,0,0.04);
}

.sortable-header .header-content {
  display: flex;
  align-items: center;
  gap: 4px;
}

.sortable-header .sort-icon {
  font-size: 18px;
  height: 18px;
  width: 18px;
  opacity: 0.5;
}

.sortable-header:hover .sort-icon {
  opacity: 0.8;
}



<div class="companies-table-frame">
  <div *ngIf="allowFilter" class="filters">
      <mat-form-field class="search-form-field" *ngIf="filters.includes('name')">
          <mat-label>Company Name</mat-label>
          <input matInput type="text" [(ngModel)]="filter.name" (ngModelChange)="filterUpdate('name', $event)">
          <ng-container *ngIf="filter.name">
              <button matSuffix mat-icon-button aria-label="Clear" (click)="filter.name='';filterUpdate('name', '')"
                  style="position: absolute; right: 0px; top: 5px;">
                  <mat-icon>close</mat-icon>
              </button>
          </ng-container>
      </mat-form-field>
      <mat-form-field class="search-form-field" *ngIf="filters.includes('country')">
          <mat-label>Country</mat-label>
          <input matInput type="text" [(ngModel)]="filter.country" (ngModelChange)="filterUpdate('country', $event)">
          <ng-container *ngIf="filter.country">
              <button matSuffix mat-icon-button aria-label="Clear" (click)="filter.country='';filterUpdate('country', '')"
                  style="position: absolute; right: 0px; top: 5px;">
                  <mat-icon>close</mat-icon>
              </button>
          </ng-container>
      </mat-form-field>
      <mat-form-field class="search-form-field" *ngIf="filters.includes('address')">
          <mat-label>Address</mat-label>
          <input matInput type="text" [(ngModel)]="filter.address" (ngModelChange)="filterUpdate('address', $event)">
          <ng-container *ngIf="filter.address">
              <button matSuffix mat-icon-button aria-label="Clear" (click)="filter.address='';filterUpdate('address', '')"
                  style="position: absolute; right: 0px; top: 5px;">
                  <mat-icon>close</mat-icon>
              </button>
          </ng-container>
      </mat-form-field>
      <mat-form-field class="search-form-field" *ngIf="filters.includes('company_id')">
          <mat-label>Company ID</mat-label>
          <input matInput type="text" [(ngModel)]="filter.company_id" (ngModelChange)="filterUpdate('company_id', $event)">
          <ng-container *ngIf="filter.company_id">
              <button matSuffix mat-icon-button aria-label="Clear" (click)="filter.company_id=''; filterUpdate('company_id', '')"
                  style="position: absolute; right: 0px; top: 5px;">
                  <mat-icon>close</mat-icon>
              </button>
          </ng-container>
      </mat-form-field>
      <mat-form-field class="search-form-field" *ngIf="filters.includes('status')">
          <mat-label>MRA Statuses</mat-label>
          <mat-select [formControl]="statusesForm" multiple [panelWidth]="''">
              <mat-option *ngFor="let mraRequestStatus of MRAList"
                  [value]="mraRequestStatus.value">{{mraRequestStatus.name}}</mat-option>
          </mat-select>
      </mat-form-field>
      <mat-form-field class="search-form-field" *ngIf="filters.includes('tin')">
          <mat-label>TIN</mat-label>
          <input matInput type="text" [(ngModel)]="filter.tin" (ngModelChange)="filterUpdate('tin', $event)">
          <ng-container *ngIf="filter.tin">
              <button matSuffix mat-icon-button aria-label="Clear" (click)="filter.tin=''; filterUpdate('tin', '')"
                  style="position: absolute; right: 0px; top: 5px;">
                  <mat-icon>close</mat-icon>
              </button>
          </ng-container>
      </mat-form-field>
  </div>
  <div class="table">
      <div class="grid-container">
          <!-- Grid Header -->
          <div class="grid-header">
              <div class="grid-cell sortable-header" (click)="sort('index')">
                  <div class="header-content">
                      #
                      <mat-icon class="sort-icon">{{getSortIcon('index')}}</mat-icon>
                  </div>
              </div>
              <div class="grid-cell sortable-header" (click)="sort('name')">
                  <div class="header-content">
                      Company Name
                      <mat-icon class="sort-icon">{{getSortIcon('name')}}</mat-icon>
                  </div>
              </div>
              <div class="grid-cell sortable-header" (click)="sort('company_id')">
                  <div class="header-content">
                      Company ID
                      <mat-icon class="sort-icon">{{getSortIcon('company_id')}}</mat-icon>
                  </div>
              </div>
              <div class="grid-cell sortable-header" (click)="sort('g2g_id')">
                  <div class="header-content">
                      G2G ID
                      <mat-icon class="sort-icon">{{getSortIcon('g2g_id')}}</mat-icon>
                  </div>
              </div>
              <div class="grid-cell">Address</div>
              <div class="grid-cell">Country</div>
              <div class="grid-cell">Current MRA Status</div>
              <div class="grid-cell">Approval Status Last Updated</div>
          </div>

          <!-- Grid Rows -->
          <div class="grid-row-wrapper" *ngFor="let company of filteredCompanies">
              <div class="grid-row {{selected.includes(company.id) ? 'selected' : ''}}" 
                   (click)="toggleRow(company.id)">
                  <div class="grid-cell">{{company.id+1}}</div>
                  <div class="grid-cell">{{company.name}}</div>
                  <div class="grid-cell">{{company.companyID.id}}</div>
                  <div class="grid-cell">{{company.g2gID}}</div>
                  <div class="grid-cell">{{company.companyAddress}}</div>
                  <div class="grid-cell">{{getThisCountryNameFromISO(company.hostCountry)}}</div>
                  <div class="grid-cell">
                      <div class="table-cell status-text"
                          [ngClass]="company.mra_status.class ==='approved' ? 'approved-text' : company.mra_status.class ==='rejected' 
                          ? 'rejected-text' : 'pending-text'">
                          {{company.mra_status.value}}
                      </div>
                  </div>
                  <div class="grid-cell">{{company.company.apprvlStusDttm}}</div>
              </div>

              <!-- Expandable Details Row -->
              <div class="table-details-wrapper" [class.expanded]="rowExpanded(company.id)">
                  <div class="table-details-header">
                      <div class="details-text">
                          Company Information
                      </div>
                      <button class="details-button" mat-raised-button (click)="openModal(company)">
                          Details
                      </button>
                  </div>
                  <div class="details-row">
                      <div class="flex-row">
                          <div class="details-frame-1">
                              <div class="flex-row">
                                  <div class="details-field-container">
                                      <mat-label class="details-header">Company Name:</mat-label>
                                      <span class="details-field">
                                          {{company.name}}
                                      </span>
                                  </div>
                              </div>
                              <div class="flex-row">
                                  <div class="details-field-container">
                                      <mat-label class="details-header">Company ID:</mat-label>
                                      <span class="details-field">
                                          {{company.companyID.id}}
                                      </span>
                                  </div>
                                  <div class="details-field-container">
                                      <mat-label class="details-header">G2G ID:</mat-label>
                                      <span class="details-field">
                                          {{company.g2gID}}
                                      </span>
                                  </div>
                                  <div class="details-field-container">
                                      <mat-label class="details-header">TIN:</mat-label>
                                      <span class="details-field">
                                          {{company.tin}}
                                      </span>
                                  </div>
                              </div>
                              <div class="flex-row">
                                  <div class="details-field-container">
                                      <mat-label class="details-header">Country: </mat-label>
                                      <span class="details-field">
                                          {{getThisCountryNameFromISO(company.hostCountry)}}
                                      </span>
                                  </div>
                                  <div class="details-field-container">
                                      <mat-label class="details-header">Status: </mat-label>
                                      <div class="details-field status-text status-details-text"
                                          [ngClass]="company.mra_status.class === 'approved' ? 'approved-text' : 
                                          company.mra_status.class === 'rejected' ? 'rejected-text' : 'pending-text'"
                                      >
                                          {{company.mra_status.value}}
                                      </div>
                                  </div>
                                  <div class="details-field-container"><!--placeholder--></div>
                              </div>
                          </div>
                      </div>
                  </div>
                  <div class="locations-row" *ngIf="company.company.cmpnyLocList.length>0">
                      <mat-expansion-panel class="remove-expansion-padding" [expanded]="true">
                          <mat-expansion-panel-header class="details-expansion-header">
                              <mat-panel-title> 
                                      Company Locations
                                  </mat-panel-title>
                          </mat-expansion-panel-header>
                             <div class="details-location-frame">
                              <div *ngFor="let location of company.company.cmpnyLocList; index as locationIndex;" 
                              (click)="openAddressModal(company, location)"
                              class="location-detail flex-column" [class.primaryLocation]="location.isPrmryLoc">
                                  <div>
                                      <div class="location-header" >
                                          {{location.isPrmryLoc ? 'Primary Location' : 'Location '+(locationIndex)}}:
                                      </div>
                                  </div>
                                  <div class="location-details flex-column">
                                      <div class="flex-row">
                                          <div class="address details-field-container">
                                              <mat-label class="details-header">Address: </mat-label>
                                              <div class="address details-field" *ngIf="location.cmpnyAddr">
                                                  {{location.cmpnyAddr}}
                                              </div>
                                              <div class="address details-field" *ngIf="!location.cmpnyAddr">
                                                  <div class="address-text" *ngFor="let address of createAddress(location)">
                                                      {{address}}
                                                  </div>
                                              </div>
                                          </div>
                                          <div class="details-field-container">
                                              <mat-label class="details-header">Location Role:</mat-label>
                                              <span class="details-field">
                                                  {{getLocationRole(location)}}
                                              </span>
                                          </div>
                                      </div>
                                      <div class="location-row-2 flex-row">
                                          <div class="details-field-container" *ngIf="location.latud && location.lngtud">
                                              <mat-label class="details-header">Latitude/Latitude:</mat-label>
                                              <span class="details-field">
                                                  {{location.latud}}/{{location.lngtud}}
                                              </span>
                                          </div>
                                          <div class="details-field-container">
                                              <mat-label class="details-header">Last Updated:</mat-label>
                                              <span class="details-field">
                                                  {{location.updtDttm || company.lastUpdated}}
                                              </span>
                                          </div>
                                          <div class="details-field-container details-primary-location">
                                              <mat-label class="details-header">Primary Location: </mat-label>
                                              <div class="details-field status-text"
                                                  [ngClass]="location.isPrmryLoc ? 'approved-text' :  'rejected-text'">
                                                  {{location.isPrmryLoc ? 'Yes': 'No'}}
                                          </div>
                                          </div>
                                      </div>
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
