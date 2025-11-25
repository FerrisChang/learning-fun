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
                      <button class="details-button" mat-raised-button (click)="openModal(company.company)">
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
