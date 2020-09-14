# INTRO
This is a quick tutorial to show how you can send messages between components in Angular 9 and RxJS.

### Observable.subscribe()
The observable subscribe method is used by angular components to subscribe to messages that are sent to an observable.

### Subject.next()
The subject next method is used to send messages to an observable which are then sent to all angular components that are subscribers (a.k.a. observers) of that observable.


## Angular 9 Message Service
With the message service you can subscribe to new messages in any component with `onMessage()` method, send messages from any component with the `sendMessage(message: string)` method, and clear messages from any component with the `clearMessages()` method.

The `clearMessages()` method actually just sends an empty message by calling `this.subject.next()` without any arguments, the logic to clear the messages when an empty message is received is in the app component below.

```typescript
import { Injectable } from '@angular/core';
import { Observable, Subject } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class MessageService {
    private subject = new Subject<any>();

    sendMessage(message: string) {
        this.subject.next({ text: message });
    }

    clearMessages() {
        this.subject.next();
    }

    onMessage(): Observable<any> {
        return this.subject.asObservable();
    }
}
```

## App Component that Receives Messages
The app component uses the message service to subscribe to new **messages** and push them into the messages array which is displayed in the app component template. If an empty message is received then the messages array is cleared which automatically removes the messages from the UI.

```typescript
import { Component, OnDestroy } from '@angular/core';
import { Subscription } from 'rxjs';

import { MessageService } from './_services/index';

@Component({ selector: 'app', templateUrl: 'app.component.html' })
export class AppComponent implements OnDestroy {
    messages: any[] = [];
    subscription: Subscription;

    constructor(private messageService: MessageService) {
        // subscribe to home component messages
        this.subscription = this.messageService.onMessage().subscribe(message => {
          if (message) {
            this.messages.push(message);
          } else {
            // clear messages when empty message received
            this.messages = [];
          }
        });
    }

    ngOnDestroy() {
        // unsubscribe to ensure no memory leaks
        this.subscription.unsubscribe();
    }
}
```

## Home Component that Sends Messages
The home component uses the message service to send messages that are displayed by the app component.

```typescript
import { Component } from '@angular/core';

import { MessageService } from '../_services/index';

@Component({ templateUrl: 'home.component.html' })
export class HomeComponent {
    constructor(private messageService: MessageService) { }

    sendMessage(): void {
        // send message to subscribers via observable subject
        this.messageService.sendMessage('Message from Home Component to App Component!');
    }

    clearMessages(): void {
        // clear messages
        this.messageService.clearMessages();
    }
}
```