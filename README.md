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
      <button class="mark-read" type="button" (click)="markAllAsRead($event)">
        Mark all as read
      </button>
    </div>

    <ul class="alerts-list" *ngIf="alerts.length > 0; else emptyState">
      <li
        class="alert-item"
        *ngFor="let alert of alerts; trackBy: trackByAlertId"
        [class.unread]="!alert.read"
        (click)="markAlertAsRead(alert.id, $event)"
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
        <button
          type="button"
          class="dismiss"
          aria-label="Dismiss notification"
          (click)="removeAlert(alert.id, $event)"
        >
          ×
        </button>
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



:host {
  display: inline-block;
  font-family: inherit;
}

.notifications-wrapper {
  position: relative;
}

.notification-button {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  background: #ffffff;
  border: 1px solid rgba(15, 23, 42, 0.12);
  border-radius: 999px;
  padding: 0.35rem 0.9rem;
  cursor: pointer;
  transition: box-shadow 150ms ease, transform 150ms ease;
  font-weight: 600;
  color: #0f172a;
}

.notification-button:hover {
  box-shadow: 0 12px 24px rgba(15, 23, 42, 0.1);
  transform: translateY(-1px);
}

.notification-button:focus-visible {
  outline: 2px solid #6366f1;
  outline-offset: 3px;
}

.bell-icon {
  display: flex;
  width: 24px;
  height: 24px;
  background: rgba(99, 102, 241, 0.12);
  border-radius: 999px;
  justify-content: center;
  align-items: center;
}

.bell-icon svg {
  width: 16px;
  height: 16px;
  fill: #4c1d95;
}

.button-label {
  font-size: 0.9rem;
}

.badge {
  min-width: 1.25rem;
  padding: 0 0.35rem;
  height: 1.25rem;
  border-radius: 999px;
  background: #f43f5e;
  color: #fff;
  font-size: 0.75rem;
  font-weight: 700;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.dropdown-panel {
  position: absolute;
  right: 0;
  top: calc(100% + 0.75rem);
  width: min(340px, 80vw);
  background: #ffffff;
  border-radius: 1rem;
  box-shadow: 0 24px 60px rgba(3, 7, 18, 0.18);
  border: 1px solid rgba(15, 23, 42, 0.08);
  padding: 1rem;
  z-index: 10;
}

.dropdown-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.75rem;
}

.dropdown-header h3 {
  margin: 0;
  font-size: 1rem;
  font-weight: 700;
  color: #111827;
}

.dropdown-header p {
  margin: 0;
  font-size: 0.78rem;
  color: #6b7280;
}

.mark-read {
  border: none;
  background: transparent;
  font-size: 0.78rem;
  font-weight: 600;
  color: #6366f1;
  cursor: pointer;
}

.alerts-list {
  list-style: none;
  margin: 0;
  padding: 0;
  max-height: 360px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.alert-item {
  display: grid;
  grid-template-columns: auto 1fr auto;
  gap: 0.6rem;
  padding: 0.65rem;
  border-radius: 0.85rem;
  background: rgba(99, 102, 241, 0.04);
  cursor: pointer;
  transition: background 120ms ease, transform 120ms ease;
}

.alert-item:hover {
  background: rgba(99, 102, 241, 0.1);
  transform: translateX(2px);
}

.alert-item.unread {
  box-shadow: inset 0 0 0 1px rgba(99, 102, 241, 0.45);
  background: rgba(99, 102, 241, 0.12);
}

.avatar {
  width: 40px;
  height: 40px;
  border-radius: 999px;
  background: linear-gradient(135deg, #6366f1, #a855f7);
  color: #fff;
  font-weight: 700;
  display: flex;
  align-items: center;
  justify-content: center;
  text-transform: uppercase;
}

.alert-body {
  display: flex;
  flex-direction: column;
  gap: 0.2rem;
}

.alert-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 0.82rem;
  color: #0f172a;
  font-weight: 600;
}

.alert-title {
  margin-right: 0.5rem;
}

.alert-time {
  font-size: 0.74rem;
  color: #94a3b8;
  font-weight: 500;
}

.alert-message {
  margin: 0;
  color: #475569;
  font-size: 0.82rem;
  line-height: 1.2rem;
}

.dismiss {
  border: none;
  background: transparent;
  color: #94a3b8;
  font-size: 1.1rem;
  cursor: pointer;
  align-self: flex-start;
}

.dismiss:hover {
  color: #0f172a;
}

.empty-state {
  text-align: center;
  padding: 2rem 1rem;
  color: #475569;
  font-size: 0.9rem;
}

.empty-state p {
  margin: 0 0 0.25rem;
  font-weight: 600;
  color: #0f172a;
}




import { CommonModule } from '@angular/common';
import { Component, ElementRef, HostListener } from '@angular/core';

@Component({
  selector: 'app-notifications-updates',
  standalone: true,
  imports: [CommonModule],
  templateUrl: './notifications-updates.component.html',
  styleUrl: './notifications-updates.component.css'
})
export class NotificationsUpdatesComponent {
  dropdownOpen = false;

  alerts: NotificationAlert[] = [
    {
      id: 1,
      title: 'New comment',
      message: 'Carlos mentioned you on the quarterly review thread.',
      timeAgo: '2m',
      read: false
    },
    {
      id: 2,
      title: 'Team milestone',
      message: 'Design System v2.1 is now live for everyone.',
      timeAgo: '45m',
      read: false
    },
    {
      id: 3,
      title: 'New follower',
      message: 'Alex Lee started following your updates.',
      timeAgo: '3h',
      read: true
    },
    {
      id: 4,
      title: 'Security alert',
      message: 'New login from Chrome on macOS.',
      timeAgo: '1d',
      read: true
    }
  ];

  constructor(private host: ElementRef<HTMLElement>) {}

  get unreadCount(): number {
    return this.alerts.reduce((total, alert) => total + (alert.read ? 0 : 1), 0);
  }

  toggleDropdown(event: MouseEvent): void {
    event.stopPropagation();
    this.dropdownOpen = !this.dropdownOpen;
  }

  markAlertAsRead(alertId: number, event?: MouseEvent): void {
    event?.stopPropagation();
    this.alerts = this.alerts.map((alert) =>
      alert.id === alertId ? { ...alert, read: true } : alert
    );
  }

  markAllAsRead(event?: MouseEvent): void {
    event?.stopPropagation();
    if (this.unreadCount === 0) {
      return;
    }
    this.alerts = this.alerts.map((alert) => ({ ...alert, read: true }));
  }

  removeAlert(alertId: number, event: MouseEvent): void {
    event.stopPropagation();
    this.alerts = this.alerts.filter((alert) => alert.id !== alertId);
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

type NotificationAlert = {
  id: number;
  title: string;
  message: string;
  timeAgo: string;
  read: boolean;
};
