# events between components

## Case studies
### events on a shared service
Create the service
```
import {Injectable} from '@angular/core';
import * as Rx from 'rxjs/Rx';

@Injectable()
export class EventsService {
    private listeners: any = null;
    private eventsSubject: Rx.Subject<{name:string; args: any[]}> = null;
    private events: Rx.Observable<{name:string; args: any[]}> = null;
    
    constructor() {
        this.listeners = {};
        this.eventsSubject = new Rx.Subject();

        this.events = Rx.Observable.from(this.eventsSubject);

        this.events.subscribe(
            ({name, args}) => {
                if (this.listeners[name]) {
                    for (let listener of this.listeners[name]) {
                        listener(...args);
                    }
                }
            });
    }
    on(name, listener) {
        if (!this.listeners[name]) {
            this.listeners[name] = [];
        }

        this.listeners[name].push(listener);
    }

    broadcast(name, ...args) {
        this.eventsSubject.next({
            name,
            args
        });
    }
}
```
Inject the event service at the root module
```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
...
import { UtilService, EventsService } from './services';

const appRoutes : Routes = [
      {path: 'designpanel', component: DesignPanelComponent},
      ...
];
@NgModule({
  declarations: [
    AppComponent,
    ...
  ],
  imports: [
    HttpModule,
    BrowserModule,
    ModalModule.forRoot(),
    FormsModule,
    RouterModule.forRoot(appRoutes)
  ],
  bootstrap: [AppComponent],
  providers: [EventsService, ...]
})
export class AppModule { }
```
Broadcast the event
```
import { Component, OnInit, TemplateRef } from '@angular/core';
import { Project, User } from './../models';
import { EventsService } from './../services';


@Component({
  selector: 'app-projects',
  templateUrl: './projects.component.html',
  styleUrls: ['./projects.component.css']
})
export class ProjectsComponent implements OnInit {

  constructor(private _event: EventsService) { 
    ...
  }

  removeProject(project: string){
    ...
    this._event.broadcast("projects", "project_removed");

  }
}
```
Subscribe the event
```
import { Component, TemplateRef } from '@angular/core';
import { Router } from '@angular/router';
...
import { EventsService} from './services';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {

  constructor(private _event: EventsService,
    ...
  ){}

  ngOnInit(){
    this._event.on("projects", n=>{
      this.resetProjectMenu();
    });
  }
  
  ...
}

```
## Reference
- https://stackoverflow.com/questions/34700438/global-events-in-angular
