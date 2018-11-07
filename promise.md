# Promise
[Home](README.md)

A Promise is an object representing the eventual completion or failure of an asynchronous operation.

## Case Study
### Call in sequence
typscript
```
getUser(): Promise<User>{
    return new Promise<User>((resolve)=>{
      this.http.get(...)
      .subscribe(res=>{
        ...
        resolve(user_data);
      };
}

updateUser() {
  getUser()
  .then(user_data=>{
    //do something on user_data;
    return user_data; //can return normal data
  })
  .then(user_data=>{
    //update the user_data
    return new Promise<User>((resolve)=>{
      this.http.patch(user_data).subscribe(res=>resolve(user_data));
    }); //can return the promise object
  })
  .then(user_data=>{
    //update the user data on the UI;
  });
}
```
## API
methods:
- `Promise.all`: It returns a single Promise that resolves when all of the promises in the iterable argument have resolved or when the iterable argument contains no promises. It rejects with the reason of the first promise that rejects.
- `Promise.prototype`.then: It returns a Promise.
- `Promise.prototype.reject`: It returns a Promise object that is rejected with the given reason.
- `Promise.prototype.finally`: It returns a Promise. When the promise is settled, whether fulfilled or rejected, the specified callback function is executed. This provides a way for code that must be executed once the Promise has been dealt with to be run whether the promise was fulfilled successfully or rejected.
- `Promise.prototype.race`: It returns a promise that resolves or rejects as soon as one of the promises in the iterable resolves or rejects, with the value or reason from that promise.
- `Promise.prototype.resolve`: It returns a Promise object that is resolved with the given value

If you find the promise object doesn't work in your target browser/environment, please refer to the links below.
- https://stackoverflow.com/questions/22516959/how-to-determine-if-a-promise-is-supported-by-the-browser
- https://stackoverflow.com/questions/36016327/how-to-make-promises-work-in-ie11

## Reference
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises
- http://www.cnblogs.com/rubylouvre/p/3495286.html
- https://developers.google.com/web/fundamentals/primers/promises
- http://liubin.org/promises-book/

[Home](README.md)
