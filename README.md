
import { Injectable } from '@angular/core';
import { BehaviorSubject, Observable } from 'rxjs';
import { NotificationAlert } from './notification-alert.model';

const STORAGE_KEY = 'app_notifications';

// TODO: Remove example notifications when ready for production
// Example notifications for development/demo purposes
// These will be used when localStorage is empty
const EXAMPLE_NOTIFICATIONS: NotificationAlert[] = [
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
  },
  {
    id: 5,
    title: 'Project update',
    message: 'The Q4 roadmap has been published. Check it out!',
    timeAgo: '2d',
    read: false
  },
  {
    id: 6,
    title: 'Meeting reminder',
    message: 'Team standup starts in 15 minutes.',
    timeAgo: '5m',
    read: false
  },
  {
    id: 7,
    title: 'Document shared',
    message: 'Sarah shared "Product Requirements" with you.',
    timeAgo: '1h',
    read: true
  },
  {
    id: 8,
    title: 'Code review',
    message: 'Your pull request #1234 has been approved.',
    timeAgo: '4h',
    read: false
  },
  {
    id: 9,
    title: 'System maintenance',
    message: 'Scheduled maintenance will occur tonight at 2 AM EST.',
    timeAgo: '1d',
    read: true
  },
  {
    id: 10,
    title: 'Welcome!',
    message: 'Thanks for joining our platform. Get started with the onboarding guide.',
    timeAgo: '3d',
    read: true
  }
];

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
    // Check if we're in a browser environment (localStorage is not available during SSR)
    if (typeof window === 'undefined' || typeof localStorage === 'undefined') {
      // TODO: Remove example notifications - return [] when ready for production
      return EXAMPLE_NOTIFICATIONS;
    }
    
    try {
      const stored = localStorage.getItem(STORAGE_KEY);
      if (stored) {
        const parsed = JSON.parse(stored);
        if (Array.isArray(parsed)) {
          return parsed;
        }
      }
      // TODO: Remove example notifications - return [] when ready for production
      // Use example notifications when localStorage is empty (for development/demo)
      return EXAMPLE_NOTIFICATIONS;
    } catch (error) {
      console.error('Error loading notifications from localStorage:', error);
      // TODO: Remove example notifications - return [] when ready for production
      return EXAMPLE_NOTIFICATIONS;
    }
  }
}

