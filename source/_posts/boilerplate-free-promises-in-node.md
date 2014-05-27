title: 'Boilerplate-free promises in Node'
comments: false
date: 2014-05-27 10:53:00
tags:
- node
- javascript
- bluebird
- promises
---
One of the few frustrations I've had as I've been writing more Node-based JavaScript lately is that deeply-nested code comes quickly. Async-by-default is a powerful philosophy, but callback hell is real.

It's well-established that the promise pattern helps to reduce nested code, but you still end up writing a lot of boilerplate just to handle a promise:

```javascript
var Promise = require('bluebird');

// before
function renameSomething (callback) {
    fs.rename('foo', 'bar', callback);
}

// after
function renameSomething_promise () {
    return new Promise(function (resolve, reject) {
        fs.rename('foo', 'bar', function (err) {
            if (err) {
                reject();
            } else {
                resolve();
            }
        });
    });
}
```

Half the power of promises is that they're chainable, so you end up writing a ton of short functions like the above, and the `reject`/`resolve` boilerplate gets old fast.

Then I discovered that the [Bluebird promise implementation](https://github.com/petkaantonov/bluebird/) has an excellent utility function called [`promisify`](https://github.com/petkaantonov/bluebird/blob/master/API.md#promisification). It works on any async function that follows the Node convention of having the callback be the last argument, which takes an error as its first argument.

```javascript
var Promise = require('bluebird');

function renameSomethingPromise () {
    var rename = Promise.promisify(fs.rename);
    return rename('foo', 'bar');
}
```

This will return a new `Promise`, resolve or reject it based on the callback error argument, and pass back whatever value is returned in the following argument. All with the addition of only one line of code per async function (or less if you're reusing promisified functions).

`Promise.promisify` can do more, like [converting an entire module](https://github.com/petkaantonov/bluebird/blob/master/API.md#promisepromisifyallobject-target---object) and resolving promises where the callback returns multiple success arguments.

Node's community leaders [have already stated](https://github.com/petkaantonov/bluebird/blob/master/API.md#promisepromisifyallobject-target---object) that callbacks will remain the de facto standard for asynchronous code, so this trick is bound to come in handy for a good while.
