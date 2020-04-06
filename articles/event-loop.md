<!-- @format -->

### Event Loop

Overview

JavaScript has a concurrency model based on an event loop, which is responsible for executing the code, collecting and processing events, and executing queued sub-tasks.<sup>[1]</sup> 

![Javascript Runtime Example](https://mdn.mozillademos.org/files/17124/The_Javascript_Runtime_Environment_Example.svg)

Stack
- Function calls form a stack of frames containing the functions arguments and local variables
- When a function on the stack invokes another function, a second frame is created and pushed onto the stack with that functions arguments and variables
- When a function returns its frame is popped off the stack
- When the last frame returns the stack is empty

Heap
- Objects are allocated to a large unstructured region of memory called the heap

Queue
- A Javascript runtime uses a messages queue (aka event queue) which is a list of messages to be processed
- Each message has an associated function which gets called in order to handle the message
- When messages are handled they are removed from the queue and its corresponding function is called with the message as the input parameter

Event Loop
- The event loop waits for a message to arrive (synchronously)
- Each message is processed completely before any other message is started
- When a function runs entirely before any other code runs
- The downside is if a message takes too long to complete the application is unable to handle user actions like click or scroll

Adding Messages
- Messages are added anytime an event fires with a corresponding event listener
- If there isn't an associated event listener then the event is lost
- A click event with a handler will add a message
- If a setTimeout is called and there are no other messages in the queue it will run right away or else it will have to wait for the previous message (callback) to finish
- Calling a setTimeout with a delay of 0 doesn't mean it will fire in 0 ms that depends on how many messages are currently in the queue

Several Runtimes Communicating Together
- A web worker or a cross-origin iframe have their own stack, heap, and message queue. Two runtimes can only communiate together by sending messages via a `postMessage`

```javascript
function foo(b) {
  let a = 10
  return a + b + 11
}

function bar(x) {
  let y = 3
  return foo(x * y)
}

console.log(bar(7)) //returns 42
```

```javascript
(function() {

  console.log('this is the start');

  setTimeout(function cb() {
    console.log('Callback 1: this is a msg from call back');
  }); // has a default time value of 0

  console.log('this is just a message');

  setTimeout(function cb1() {
    console.log('Callback 2: this is a msg from call back');
  }, 0);

  console.log('this is the end');

})();

// "this is the start"
// "this is just a message"
// "this is the end"
// "Callback 1: this is a msg from call back"
// "Callback 2: this is a msg from call back"
```

Recommendations

### Examples

Assigning a value to an undeclared variable implicitly puts it in the global
scope.

### Sources
[1]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop 'Concurrency model and the event loop'
[MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop) 

[2]: https://www.sitepoint.com/back-to-basics-javascript-hoisting/ "Back To Basics"
[Back To Basics](https://www.sitepoint.com/back-to-basics-javascript-hoisting/)

[3]: https://scotch.io/tutorials/understanding-hoisting-in-javascript
[Understanding Hoisting in Javascript](https://scotch.io/tutorials/understanding-hoisting-in-javascript)
