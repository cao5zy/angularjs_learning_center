# Observable
[Home](README.md)


Observables provide support for passing messages between publishers and subscribers in your application. Observables offer significant benefits over other techniques for event handling, asynchronous programming, and handling multiple values.

In angluarJs2+, `rxjs` which ships Observable has been installed when initializing the angularjs project.

## Case Studies
### Consume observable object in Http post method
```
import { Http } from '@angular/http';
...

@Component({ 
  selector: 'app-login',
  templateUrl: './login.component.html',
  styleUrls: ['./login.component.css']
})
export class LoginComponent implements OnInit {
  constructor(private _http: Http){}
  ...
  
  this._http.post(environment.auth_login_url, {name: this.username, pwd: this.password})
	.subscribe(res=>{
	  ((data)=>{
	    this.router.navigateByUrl("servlist");
	    ...
      
	  })(res["_body"]);
	});

  ...  
 }
```
**If `subscribe` is not invoked, the post method will NOT run**

### Create your own observable object
The case that needs to create your own observable object normally is caused by composing an object which needs more than one service calls.
```
import { Component, OnInit } from '@angular/core';
import { Observable, Observer } from 'rxjs/Rx';
import { AccountService, useService, Service } from 'http-micro-service-front';
import * as _ from 'lodash';

interface OwnedProject {
  project: string,
  members: string[]
}


@Component({
  selector: 'app-projects',
  templateUrl: './projects.component.html',
  styleUrls: ['./projects.component.css']
})
export class ProjectsComponent implements OnInit {

  constructor(private account: AccountService,
    private _service: Service) {
      this._queryOwnedProjects = useService(this._service, "users_query_service:owned_projects");
      this._queryProject = useService(this._service, "users_service:projects");
    }

  _queryOwnedProjects: any = null;
  _queryProject: any = null;
  
  ownedProjects: Observable<OwnedProject[]> = null;
  ngOnInit() {
    this.ownedProjects = new Observable<OwnedProject[]>((observer:Observer<OwnedProject[]>)=>{
	
      let getOwnedProjects:()=>Promise<string[]> = ()=>{
        return new Promise<string[]>(resolve=>{
	  if (this.account.hasToken())
	  {
            this._queryOwnedProjects("get")({ id: this.account.getName()})
	      .subscribe(res=>{
		resolve(res.projects);
	    });
	  }
	  else
	    resolve([]);  
	});
      }

      let getProjectMembers:(string)=>Promise<OwnedProject> = (project)=>{
        return new Promise<OwnedProject>(resolve=>{
	  this._queryProject("get")({id: buildProjectKey(project)})
	    .subscribe(res=>{
	      resolve({project: project, members:res.users});
	    });
	});
      }

      let  buildProjectKey: (string)=>string = (project)=>{
        return this.account.getName() + "$$" + project;
      }
      
      getOwnedProjects()
      .then(projects=>{
        return Promise.all(_.map(projects, project => getProjectMembers(project)));
      })
      .then(ownedProjects=>{
        observer.next(ownedProjects);
	observer.complete();**
      });
    });
  }

}


```
The code consumes `this.ownedProjects`.
```
<tr *ngFor="let obj of ownedProjects | async; let i = index;">
  <td>{{ obj.project }}</td>
  <td><ul><li *ngFor="let member of obj.members">{{ member }};</li></ul></td>
</tr>
```


## References
- http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html
- https://angular.io/guide/observables
- http://www.angulartypescript.com/angular-2-rxjs-observable/
- https://www.npmjs.com/package/rxjs

[Home](README.md)
