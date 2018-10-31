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
```
**If `subscribe` is not invoked, the post method will NOT run**


## References
- http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html
- https://angular.io/guide/observables
- http://www.angulartypescript.com/angular-2-rxjs-observable/
- https://www.npmjs.com/package/rxjs

[Home](README.md)
