# Intro
In Angular you can also programmatically navigate via a Router service we inject into our component.
This is useful when you want trigger a navigation after doing something else in your methods.

```HTML
<!-- home.component.html -->
<button (click)="onLoadServers()">Load Servers</button>
```

```typescript
//home.component.ts
import { Component, OnInit } from '@angular/core';
import { Router } from '@angular/router';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {

  constructor(private router: Router) { }

  onLoadServers() {
    // other code here
    this.router.navigate(['/servers']); // navigating to an absolute path here
  }
}
```
