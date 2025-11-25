
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
	createAddress(location : CompanyLocation ) {
		return this.utils.convertLocationConversion(location, 'company');
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
	toggleRow (index : number) {
		let searchIndex = this.expandedRows.indexOf(index); 
		if (searchIndex>=0) {
			this.expandedRows.splice(searchIndex, 1); 
		}
		else {
			this.expandedRows.push(index);
		}
	}
	rowExpanded (index : number) {
		return this.expandedRows.includes(index); 
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
