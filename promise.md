# Promise
[Home](README.md)

A Promise is an object representing the eventual completion or failure of an asynchronous operation.

## API
methods:
- `Promise.all`: It returns a single Promise that resolves when all of the promises in the iterable argument have resolved or when the iterable argument contains no promises. It rejects with the reason of the first promise that rejects.
- `Promise.prototype`.then: It returns a Promise.
- `Promise.prototype.reject`: It returns a Promise object that is rejected with the given reason.
- `Promise.prototype.finally`: It returns a Promise. When the promise is settled, whether fulfilled or rejected, the specified callback function is executed. This provides a way for code that must be executed once the Promise has been dealt with to be run whether the promise was fulfilled successfully or rejected.
- `Promise.prototype.race`: It returns a promise that resolves or rejects as soon as one of the promises in the iterable resolves or rejects, with the value or reason from that promise.
- `Promise.prototype.resolve`: It returns a Promise object that is resolved with the given value

## Case Study
### Chain invoke

### Error handling

###

## Reference
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises
- http://www.cnblogs.com/rubylouvre/p/3495286.html
- https://developers.google.com/web/fundamentals/primers/promises

[Home](README.md)
