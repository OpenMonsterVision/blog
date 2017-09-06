---
title: "My First Post"
date: 2017-09-06T12:24:42-05:00
draft: true
---
body
```javascript
function sendMessage(message, callback) {
    if (!message) return callback(false);
    return callback(true);
}

sendMessage('some message', function (status) {
    if (status) return 'message sent';
    return 'message not sent';
});


// function with optional callback
//
function doThing(optionalCallback) {
    console.log('doThing()');

    // if callback provided we can use otherwise return a value
    //
    if (optionalCallback && (typeof optionalCallback === 'function')) {
        console.log('callback exists and is fn');
        return optionalCallback();
    } else {
        return console.log('no callb ack so just return this log');
    }
}

doThing();

doThing(function () {
    console.log('doThing() callback()');
});


// create a function to check if param is callback for reuse.
//
function isFn(fn) {
    console.log('ifFn() check to see if fn is who it claims to be');
    if (fn && (typeof fn === 'function')) return true;
    return false;
}

function doThing2(optionalCallback) {
    console.log('doThing2() optionalCallback()');

    if (isFn(optionalCallback)) {
        return optionalCallback();
    } else {
        return console.log('no callb ack so just return this log');
    }
}


doThing2();

doThing2(function () {
    console.log('doThing2() callback()');
});

```
