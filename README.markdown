Node DDP Client
===============

A callback style DDP ([Meteor](http://meteor.com/)'s Distributed Data Protocol) node client.

 * Added support for `onClose` and `onError` so that it is easy to reconnect when the connection is closed for some reason.
 * `onMessage()` message filter based on eventedmind work (https://github.com/EventedMind/node-ddp-client/)
 * Based on Tom Coleman (https://github.com/oortcloud/node-ddp-client)
 * Based _heavily_ on alansikora's [node-js_ddp-client](https://github.com/alansikora/node-js_ddp-client), and meteor's python client. Uses a more callback style approach.

The client implements the pre1 version of DDP. It is unfinished at this point, but should do most of what you want it to do.

Installation
============

```
  npm install ddp
```

Example
=======

Please see the example in `examples/example.js`. Or here for reference:

```js
var DDPClient = require("ddp");

var ddpclient = new DDPClient({
    host: "localhost", 
    port: 3000,
    /* optional: */
    auto_reconnect: true,
    auto_reconnect_timer: 500
  });

ddpclient.connect(function() {
  console.log('connected!');
  
  ddpclient.call('test-function', ['foo', 'bar'], function(err, result) {
    console.log('called function, result: ' + result);
  })
  
  ddpclient.subscribe('posts', [], function() {
    console.log('posts complete:');
    console.log(ddpclient.collections.posts);
  })
},
function(error) {
	console.log("Error: %s", error);
	setTimeout(function() { connect(); }, 1000);
},
function(code, msg) {
	console.log("Close: [%s] %s", code, msg);
	connect();
}
);
```

Unimplemented Features
====
* Server to Client messages
  * 'addedBefore'
  * 'movedBefore'
  * 'error'
  * 'updated'
* EJSON support



Thanks
======

Many thanks to Alan Sikora, and also Mike Bannister(@possibilities) for the initial work on a node ddp client.

Contributions:
 * Chris Mather (@eventedmind)
 * Thomas Sarlandie (@sarfata)
