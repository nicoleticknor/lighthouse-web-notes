# D2 Lecture: Asynchronous Control Flow

## Random Notes Section

* Comments are a great place for "why did you choose this operation/value/whatever" rather than just what the code is doing 
  * the What can be deduced from the code itself
  * but as a beginner i need both
* NB in an HTML file we can generate lorum ipsum through the lorum command. Do that then you can rename it to a txt file
* check out file systems in nodejs.org - you can make a log file! Etc
* Andy calls the (..) stuff in a method a "signature"

## Breakout on setTimeout

* even if the delay is zero, it will still pass to the next console.log
    ```js
    console.log('before the setTimeout');

    const delay = 0;

    setTimeout(() => {
      console.log('inside the setTimeout');
    }, delay);

    console.log('after the setTimeout');

    // in what order will the above console.log's fire? --> before, after, inside
    ```

* the loop and setTimeout register, but the value of i has incremented to 3 (because it's outside the loop) before the console.log happens:
    ```js
    let i;

    for (i = 0; i < 3; i++) {
      setTimeout(() => {
        console.log(i);
      }, 100);
    }

    // what will the above code output? --> 3\n3\n3
    ```

## Async JS

* JS is "single-threaded" and synchronous (one line executes only after the previous one is done)
  * therefore, long-running processes are an issue
* Event Loop - do some reading on this concept. 
https://miro.medium.com/max/1200/1*eAuxvVSlAofOSiBaAED0mA.jpeg

## setTimeout is an object
```js
const returnVal = setTimeout(() => {
  console.log('hello world');
}, 0);
console.log(returnVal);
```

### take a look at what happens if you wrap something in setTimeout delay = 0
* do this later. 

## Blocking vs. Non-Blocking

### Blocking Code
* Code that takes a long time to run and stops other code from executing is blocking
  * Blocking code is code that must finish running before the script can continue. 
    * This Example takes a while to execture so it blocks the consolelog
      ```js
      for (let i = 0; i < 100000; i++) {
        const date = new Date();
        console.log(date);
      }

      console.log('I am here!');
      ```
* you can use blocking code to see how long your code takes to process
      ```js
      const stop = 100000;

      const startTime = new Date().getTime();

      for (let i = 0; i < stop; i++) {
        const date = new Date();
        console.log(date);
      }

      const endTime = new Date().getTime();
      const duration = endTime - startTime;

      console.log(`That process took ${duration}ms`);
      ```
  * this took about 7 seconds to complete in Andy's terminal. VSCode's terminal is significantly slower than your machine's terminal
  * Magic number - hardcoded value like 100000 in this example which is not referencing anything. 
    * Clear this up by declaring a constant

### Non-Blocking Code
* Code that takes a long time to run and doesn't stop the code around it, is non-blocking
  * I still am not clear on whether the Event loop is seperate from the synchronicity
    * Is it a seperate stack of information that will fire at the given time, regardless of where other processes are running? 
    * What if we put a blocking function in the event loop? 

## Callbacks

### NOTE: That callbacks don't always mean async code
Examples:
  ```js
  array.map()
  array.filter()
  array.forEach()
  ```

### Return Value Example
```js
const higherOrderFn = (cb) => {
  const data = {
    name: 'Alice'
  };
  console.log('before the timeout');
  setTimeout(() => {
    data.name = 'Bob';
    cb(data);
    //return here won't work because data.name is still Alice until 1000 ms
    // return data;
  }, 1000);
  console.log('after the timeout');
  //return here won't work because it's still Alice
  // return data;
};

const callback = (val) => {
  console.log('inside the callback the value is:', val);
  //return here won't work either, because it will fire before the 1000 delay 
  //return val; 
};

//returns don't work. we need to store the invocation as a variable 
const returnValue = higherOrderFn(callback); 
console.log('returnValue:', returnValue);
```

#### NB Re-watch the Lecture from 10:55 - 11am cuz I was focused on my example above and missed it

### Multiple Timeouts Example

* Sleep Sort - the least efficient sort. but won't handle a negative number
    ```js
    const delays = [13, 24, 182, 5000, 3, 4242, 1000];

    for (const delay of delays) {
      setTimeout(() => {
        console.log(delay);
      }, delay);
    }
    ```

## setInterval
* set interval runs every X ms
  * it's also an object
* we need a way to stop it - 
  * set it to a variable (a "handle") and then pass that to clearInterval()
    ```js
    const interval = setInterval(() => {
      console.log('hello world');
    }, 1000);
    //clearInterval clears from the event loop. We won't get anything back with this because clearInterval happens before the event in the event loop
    clearInterval(interval);

    //so instead we can put an if with the clearInterval inside
    let counter = 0;
    const interval = setInterval(() => {
      counter++;
      console.log('hello world ' + counter);

      if (counter > 10) {
        clearInterval(interval);
      }
    }, 300);
    ```
    * NB we will get hello world 11 out of this - figure out why
  * you can also clear it within a setTimeout

## File System Functions

* for when we need to interact with an API in order to interact with a file system outside of our process
* we need async for this because we need to wait for the file to read before we can do anything with it
* accessing / doing stuff with file systems is a node.js thing
  * therefore look it up on nodejs.org rather than MDN
* these methods are on the back of require('fs')
* these methods are async by build/nature (unless they are sync by build)
* utf8 is the most common encoding for our files
* Example:
  ```js
  const fs = require('fs');

  //this is the sync process
    //note that my-doc.txt is something we made in the same root directory
  const fileData = fs.readFileSync('./my-doc.txt', { encoding: 'utf8' });

  console.log(fileData);

  //this is the async process
  fs.readFile('./my-doc.txt', { encoding: 'utf8' }, (err, data) => {
    //an error could be that the file doesn't exist, etc
    //the error refers to anything that will happen when trying to read the file. it has nothing to do with the content of the file
    if (err) throw err;
    // console.log(data);
    console.log('finished async read');
  });

  console.log('down here!');

  // how we would call this as a callback:
  const higherOrderFn = (cb) => {
  // if error
  cb(err); //bc data is optional

  // if not
  cb(null, data); // because you still need to pass it err, data if you want to get data
}
  ```