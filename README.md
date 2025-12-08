
import { Injectable } from '@angular/core';
import { BehaviorSubject, Observable } from 'rxjs';
import { NotificationAlert } from './notification-alert.model';

const STORAGE_KEY = 'app_notifications';

@Injectable({
  providedIn: 'root'
})
export class NotificationsService {
  private notificationsSubject: BehaviorSubject<NotificationAlert[]>;
  public notifications$: Observable<NotificationAlert[]>;

  constructor() {
    const initialNotifications = this.loadFromStorage();
    this.notificationsSubject = new BehaviorSubject<NotificationAlert[]>(initialNotifications);
    this.notifications$ = this.notificationsSubject.asObservable();
  }

  getNotifications(): NotificationAlert[] {
    return this.notificationsSubject.value;
  }

  getNotifications$(): Observable<NotificationAlert[]> {
    return this.notifications$;
  }

  getUnreadCount(): number {
    return this.notificationsSubject.value.filter(alert => !alert.read).length;
  }

  private loadFromStorage(): NotificationAlert[] {
    try {
      const stored = localStorage.getItem(STORAGE_KEY);
      if (stored) {
        const parsed = JSON.parse(stored);
        if (Array.isArray(parsed)) {
          return parsed;
        }
      }
      return [];
    } catch (error) {
      console.error('Error loading notifications from localStorage:', error);
      return [];
    }
  }
}



export interface NotificationAlert {
  id: number;
  title: string;
  message: string;
  timeAgo: string;
  read: boolean;
}




<div class="notifications-wrapper">
  <button
    class="notification-button"
    type="button"
    aria-haspopup="true"
    [attr.aria-expanded]="dropdownOpen"
    (click)="toggleDropdown($event)"
  >
    <span class="bell-icon" aria-hidden="true">
      <svg viewBox="0 0 24 24" focusable="false">
        <path
          d="M12 2a7 7 0 0 0-7 7v4.382l-.894 1.789A1 1 0 0 0 5.03 17h13.94a1 1 0 0 0 .924-1.447L19 13.382V9a7 7 0 0 0-7-7Zm0 20a3 3 0 0 0 2.995-2.824L15 19h-6a3 3 0 0 0 2.824 2.995L12 22Z"
        />
      </svg>
    </span>
    <span class="button-label">Updates</span>
    <span class="badge" *ngIf="unreadCount > 0">{{ unreadCount }}</span>
  </button>

  <div class="dropdown-panel" *ngIf="dropdownOpen">
    <div class="dropdown-header">
      <div>
        <h3>Notifications</h3>
        <p>{{ unreadCount }} unread</p>
      </div>
    </div>

    <ul class="alerts-list" *ngIf="alerts.length > 0; else emptyState">
      <li
        class="alert-item"
        *ngFor="let alert of alerts; trackBy: trackByAlertId"
        [class.unread]="!alert.read"
      >
        <div class="avatar" aria-hidden="true">
          {{ alert.title[0] }}
        </div>
        <div class="alert-body">
          <div class="alert-header">
            <span class="alert-title">{{ alert.title }}</span>
            <span class="alert-time">{{ alert.timeAgo }}</span>
          </div>
          <p class="alert-message">
            {{ alert.message }}
          </p>
        </div>
      </li>
    </ul>

    <ng-template #emptyState>
      <div class="empty-state">
        <p>You’re all caught up 🎉</p>
        <span>We’ll let you know when something new happens.</span>
      </div>
    </ng-template>
  </div>
</div>







import { CommonModule } from '@angular/common';
import { Component, ElementRef, HostListener, OnInit, OnDestroy } from '@angular/core';
import { Subscription } from 'rxjs';
import { NotificationAlert } from './notification-alert.model';
import { NotificationsService } from './notifications.service';

@Component({
  selector: 'app-notifications-updates',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './notifications-updates.component.html',
  styleUrl: './notifications-updates.component.css'
})
export class NotificationsUpdatesComponent implements OnInit, OnDestroy {
  dropdownOpen = false;
  alerts: NotificationAlert[] = [];
  private subscriptions = new Subscription();

  constructor(
    private host: ElementRef<HTMLElement>,
    private notificationsService: NotificationsService
  ) {}

  ngOnInit(): void {
    // Subscribe to notifications changes
    const notificationsSub = this.notificationsService.getNotifications$().subscribe(
      notifications => {
        this.alerts = notifications;
      }
    );
    this.subscriptions.add(notificationsSub);

    // Load initial notifications
    this.alerts = this.notificationsService.getNotifications();
  }

  ngOnDestroy(): void {
    // Clean up subscriptions
    this.subscriptions.unsubscribe();
  }

  get unreadCount(): number {
    return this.notificationsService.getUnreadCount();
  }

  toggleDropdown(event: MouseEvent): void {
    event.stopPropagation();
    this.dropdownOpen = !this.dropdownOpen;
  }


  trackByAlertId(_: number, alert: NotificationAlert): number {
    return alert.id;
  }

  @HostListener('document:click', ['$event'])
  closeDropdown(event: Event): void {
    if (!this.dropdownOpen) {
      return;
    }
    if (!this.host.nativeElement.contains(event.target as Node)) {
      this.dropdownOpen = false;
    }
  }

  @HostListener('document:keydown.escape')
  handleEscape(): void {
    this.dropdownOpen = false;
  }
}
