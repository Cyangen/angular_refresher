# Intro


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
    this.router.navigate(['/servers']);
  }
}
```
