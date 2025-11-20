
import { Component, Input, OnChanges, SimpleChanges, ChangeDetectorRef } from '@angular/core';
import { FormControl } from '@angular/forms';
import { Company, CompanyLocation, CompanyOnMapView } from 'src/app/company-full-view/companies-types';
import { MRAList } from 'src/app/mra-request-full-view/mra-request-full-view.component';
import { sampleCompany } from './models/companies';
import { MatDialog, MatDialogRef } from '@angular/material/dialog';
import { CompanyMraDlgComponent } from 'src/app/company-mra-dlg/company-mra-dlg.component';
import { ViewLocationComponent } from 'src/app/utils/view-location/view-location.component';
import { UtilsService } from 'src/app/services/utils.service';

export type CompanyTableFilter = {
  address: any;
  name: string,
  status: string[],
  country: string,
  company_id: string
  tin: string
}

export type SortState = {
  column: string;
  direction: 'asc' | 'desc' | '';
}

@Component({
  selector: 'app-companies-table',
  templateUrl: './companies-table.component.html',
  styleUrls: ['./companies-table.component.scss']
})
export class CompaniesTableComponent implements OnChanges {
  @Input() companies: CompanyOnMapView[] = sampleCompany;
  @Input() headers: string[] = ['index', 'name', 'country', 'company_id', 'status'];
  @Input() filters: string[] = ['index', 'name', 'country', 'company_id', 'status', 'tin'];
  @Input() allowFilter : boolean = false;
  MRAList = MRAList;
  filter: CompanyTableFilter = {
    name: '',
    country: '',
    company_id: '',
    tin: '',
    address: '',
    status: []
  };

  statusesForm: FormControl = new FormControl<string[]>([]);
  selected: number[] = [];
  filteredCompanies: CompanyOnMapView[] = [];
  modal!: MatDialogRef<CompanyMraDlgComponent>;
  sortState: SortState = {column: '', direction: ''};

  constructor (
    public dialogModel: MatDialog,
    private utils: UtilsService,
    private cdr: ChangeDetectorRef
  ) {}

  ngOnChanges(changes: SimpleChanges) {
    if (changes['companies']) {
      console.log('Companies input changed:', this.companies);
      this.filteredCompanies = [...this.companies];
      this.applyFilters();
      if (this.sortState.column && this.sortState.direction) {
        this.applySorting();
      }
    }
  }

  ngOnInit() {
    console.log("Companies being added for the first time: ", this.companies);
    this.filteredCompanies = [...this.companies];
    this.statusesForm.valueChanges.subscribe((status: string[]) => {
      console.log('Status form value changed:', status);
      this.filter.status = status || []; // Ensure we always have an array, even if empty
      this.filterUpdate('status', status || []);
    });
  }

  private applyFilters() {
    if (!this.companies) return;
    
    console.log('Applying filters with status:', this.filter.status);
    
    this.filteredCompanies = this.companies.filter((company: CompanyOnMapView) => {
      // If no statuses are selected, return all companies
      const statusMatch = this.filter.status.length === 0 ? true :
        this.filter.status.some(selectedStatus => {
          const companyStatus = company.mra_status.value.toLowerCase();
          const selectedStatusLower = selectedStatus.toLowerCase();
          return companyStatus === selectedStatusLower;
        });
      
      let allFilterConditions: boolean[] = [
        company.name.toLowerCase().includes(this.filter.name.toLowerCase()),
        company.hostCountry.toLowerCase().includes(this.filter.country.toLowerCase()),
        company.companyAddress.toLowerCase().includes(this.filter.address.toLowerCase()),
        company.companyID.id.toLowerCase().includes(this.filter.company_id.toLowerCase()),
        statusMatch,
        company.tin.toLowerCase().includes(this.filter.tin.toLowerCase()),
      ];
      
      return allFilterConditions.every(condition => condition);
    });
  }

    sort(column: string) {
      console.log('Sorting by column:', column)
      if(this.sortState.column === column) {
        // Toggle direction if same column
        this.sortState.direction = this.sortState.direction === 'asc' ? 'desc' : this.sortState.column === 'desc' ? '' : 'asc';
      } else {
         // New column, start with ascending
         this.sortState.column = column;
         this.sortState.direction = 'asc';
      }

      if (!this.sortState.direction) {
        this.filteredCompanies = [...this.companies];
        this.applyFilters();
      } else {
        this.applySorting();
      }
      this.cdr.detectChanges();
    }

    private applySorting() {
      console.log('Applying sort:', this.sortState);
      if (!this.sortState.column || !this.sortState.direction) {
        return;
      }

      const sortedCompanies = [...this.filteredCompanies].sort((a, b) => {
        let valueA: any;
        let valueB: any;

        switch (this.sortState.column) {
          case 'name' :
            valueA = a.name;
            valueB = b.name;
            break;
          case 'country' :
            valueA = this.getThisCountryNameFromISO(a.hostCountry);
            valueB = this.getThisCountryNameFromISO(b.hostCountry);
            break;
          case 'company_id' :
            valueA = a.companyID.id;
            valueB = b.companyID.id;
            break;
          case 'g2g_id' :
            valueA = a.g2gID;
            valueB = b.g2gID;
            break;
          case 'status' :
            valueA = a.mra_status.value;
            valueB = b.mra_status.value;
            break;
          case 'status_updated' :
            valueA = a.company.apprvlStusDttm;
            valueB = b.company.apprvlStusDttm;
            break;
          case 'index' :
            valueA = a.id;
            valueB = b.id;
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

    this.filteredCompanies = sortedCompanies;
  }

  getSortIcon(column: string): string {
    if (this.sortState.column !== column) {
      return 'unfold_more';
    }
    return this.sortState.direction === 'asc' ? 'arrow_upward':
      this.sortState.direction === 'desc' ? 'arrow_downward':
      'unfold_more';
  }

  createAddress(location : CompanyLocation ) {
    return this.utils.convertLocationConversion(location, 'company');
  }

  openModal(company: Company) {
    this.modal = this.dialogModel.open(CompanyMraDlgComponent, {
      height: '80%',
      width: '75%',
      data: { company: company }
    });
    this.modal.afterClosed().subscribe(result => {
      console.log(`Dialog result: ${result}`);
    });
  }
  addressModal!: MatDialogRef<ViewLocationComponent>;
  openAddressModal(company: CompanyOnMapView, location: CompanyLocation) {
    this.addressModal = this.dialogModel.open(ViewLocationComponent, {
      width: '75%',
      height: '75%',
      data: {
        name: company.name,
        country: company.hostCountry,
        address: this.utils.convertLocationConversion(location, 'company'),
        locationFound: false,
        location: [0, 0]
      }
    })
  }
  toggleRow = (id: number) => {
    let selectedIndex = this.selected.indexOf(id);
    if (selectedIndex >= 0) {
      this.selected.splice(selectedIndex, 1);
    }
    else {
      this.selected.push(id);
    }
  }
  rowExpanded = (id: number) => {
    return this.selected.includes(id);
  }
  getThisCountryNameFromISO = (countryCode: string) => {
    return this.utils.translateCodeToCountry(countryCode);
  }
  filterConditions: string[] = ['name', 'country', 'address', 'company_id', 'status', 'tin']
  filterUpdate = (filterType: string, value: string | string[]) => {
    console.log('Filter update:', { filterType, value });
    if (filterType === 'status') {
      const statusArray = Array.isArray(value) ? value : [value];
    } else {
      (this.filter as any)[filterType] = value
    }
    this.applyFilters();
    if (this.sortState.column && this.sortState.direction) {
      this.applySorting();
    }
  }

  getLocationRole (location: CompanyLocation) {
    if (!location.rolList) {
      return '';
    }
    return location.rolList?.split(',').join(', ');
  }
}

