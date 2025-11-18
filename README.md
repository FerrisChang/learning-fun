
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
        <table mat-table [dataSource]="filteredCompanies" multiTemplateDataRows class="table table-hover">
            <ng-container matColumnDef="index">
                <th mat-header-cell *matHeaderCellDef (click)="sort('index')" class="sortable-header">
                    <div class="header-content">
                        #
                        <mat-icon class="sort-icon">{{getSortIcon('index')}}</mat-icon>
                    </div>
                </th>
                <td mat-cell *matCellDef="let company">
                    {{company.id+1}}
                </td>
            </ng-container>
            <ng-container matColumnDef="name">
                <th mat-header-cell *matHeaderCellDef (click)="sort('name')" class="sortable-header">
                    <div class="header-content">
                        Company Name
                        <mat-icon class="sort-icon">{{getSortIcon('name')}}</mat-icon>
                    </div>
                </th>
                <td mat-cell *matCellDef="let company">
                    {{company.name}}
                </td>
            </ng-container>
            <ng-container matColumnDef="company_id">
                <th mat-header-cell *matHeaderCellDef (click)="sort('company_id')" class="sortable-header">
                    <div class="header-content">
                        Company ID
                        <mat-icon class="sort-icon">{{getSortIcon('company_id')}}</mat-icon>
                    </div>
                </th>
                <td mat-cell *matCellDef="let company">
                    {{company.companyID.id}}
                </td>
            </ng-container>
            <ng-container matColumnDef="g2g_id">
                <th mat-header-cell *matHeaderCellDef (click)="sort('g2g_id')" class="sortable-header">
                    <div class="header-content">
                        G2G ID
                        <mat-icon class="sort-icon">{{getSortIcon('g2g_id')}}</mat-icon>
                    </div>
                </th>
                <td mat-cell *matCellDef="let company">
                    {{company.g2gID}}
                </td>
            </ng-container>
            <ng-container matColumnDef="address">
                <th mat-header-cell *matHeaderCellDef>
                    Address
                </th>
                <td mat-cell *matCellDef="let company">
                    {{company.companyAddress}}
                </td>
            </ng-container>
            <ng-container matColumnDef="country">
                <th mat-header-cell *matHeaderCellDef>
                    Country
                </th>
                <td mat-cell *matCellDef="let company">
                    {{getThisCountryNameFromISO(company.hostCountry)}}
                </td>
            </ng-container>
            <ng-container matColumnDef="status">
                <th mat-header-cell *matHeaderCellDef>
                    Current MRA Status
                </th>
                <td mat-cell *matCellDef="let company">
                    <div class="table-cell status-text"
                    [ngClass]="company.mra_status.class ==='approved' ? 'approved-text' : company.mra_status.class ==='rejected' 
                    ? 'rejected-text' : 'pending-text'">
                    {{company.mra_status.value}}
                    </div>
                </td>
            </ng-container>
            <ng-container matColumnDef="status_updated">
                <th mat-header-cell *matHeaderCellDef>
                    Approval Status Last Updated
                </th>
                <td mat-cell *matCellDef="let company">
                    {{company.company.apprvlStusDttm}}
                </td>
            </ng-container>

            <ng-container matColumnDef="extra-details">
                <td mat-cell *matCellDef="let company" [attr.colspan]="headers.length"
                    class="table-details-frame">
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
                                    <!-- <div class="flex-row">
                                        <div class="address-container">
                                            <mat-label class="details-header">Primary Location</mat-label>
                                            <div class="details-field" *ngIf="company.companyAddress">
                                                {{company.companyAddress}}
                                            </div>
                                            <div class="details-field" *ngIf="!company.companyAddress">
                                                <div class="address-text" *ngFor="let address of company.address">
                                                    {{address}}
                                                </div>
                                            </div>
                                        </div>
                                        
                                    </div> -->
                                </div>
                            </div>
                        </div>
                        <div class="locations-row" *ngIf="company.company.cmpnyLocList.length>0">
                            <mat-expansion-panel class="remove-expansion-padding"  [expanded]="true">
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
                </td>
            </ng-container>
            <tr mat-header-row *matHeaderRowDef="headers"></tr>
            <tr mat-row class="table-row {{selected.includes(row.id) ? 'selected' : ''}}" *matRowDef="let row; columns: headers;"
                (click)="toggleRow(row.id)"></tr>
            <tr mat-row *matRowDef="let row; columns: ['extra-details'];" class="table-details-row"></tr>
            
        </table>
    </div>
</div>






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

// start of the tables field styling 
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

.table-row:hover {
  background-color: rgb(184, 181, 181);
}

tr.table-details-row {
  height: 0;
  width: 100%;
}
tr.table-details-row:hover>* {
  background-color: white !important;
  --bs-table-bg-state: white;
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

.table-row {
  cursor: pointer;
  text-align: center;
  font-size: 1.1em;
}

.table-details-wrapper {
  overflow: hidden;
  display: flex;
  flex-direction: column;
  height: 0;
  opacity: 0;
  width: 100%;
  transition: 0.2s ease-out;
}

.table-details-wrapper.expanded {
  opacity: 1;
  padding-top: 1em;
  padding-bottom: 2em;
  height: auto;
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
.address.details-field-container>.details-header {
  //flex: 1;
}
.address.details-field-container>.details-field {
  flex: 2;
}

// custom css for company use case
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


// Sort CSS
.sortable-header {
  cursor: pointer;
  user-select: none;

  &:hover {
    background-color: rgba(0,0,0,0.04);
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

  &:hover .sort-icon{
    opacity: 0.8;
  }
}


