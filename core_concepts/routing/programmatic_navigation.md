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

## using relative paths
For a dynamic path, pass an array of path segments, followed by the parameters for each segment.
The fragments are applied to the current URL or the one provided in the relativeTo property of the options object, if supplied.

you can get the currently active route by injecting route which is of type `ActivatedRoute` and make
sure to import `ActivatedRoute` from the `@angular/router` package too.
It simply injects the currently active routes, so for the component you loaded,
this will be the route which loaded this component.
with that extra piece of information, Angular knows what our currently active route is.

```typescript
//servers.component.ts
import { Component, OnInit } from '@angular/core';
import { ServersService } from './servers.service';
import { Router, ActivatedRoute } from '@angular/router';
import { Route } from '@angular/compiler/src/core';

@Component({
  selector: 'app-servers',
  templateUrl: './servers.component.html',
  styleUrls: ['./servers.component.css']
})
export class ServersComponent implements OnInit {
  public servers: {id: number, name: string, status: string}[] = [];

  constructor(private serversService: ServersService, private router: Router, private route: ActivatedRoute) { }

  ngOnInit() {
    this.servers = this.serversService.getServers();
  }

  onReload() {
    this.router.navigate(['servers'], {relativeTo: this.route});
  }

}
```
