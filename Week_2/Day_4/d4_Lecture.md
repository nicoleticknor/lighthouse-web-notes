[# Week 2 Day 4 Lecture - Promises](https://web.compass.lighthouselabs.ca/activities/356)

## Random Notes

[Classwork Problems](https://github.com/nicoleticknor/w2d4_classwork)
[My Classwork Response](/Users/nicoleticknor/lighthouse/w2/d4/w2d4_classwork/01_classwork.js)

## Revisiting Callbacks

- Re-watch video about higher order functions

* Dealing with multiple async functions that depend on the previous can be done with a few approaches:
  * multiple async funcs in a row could be called nested in one after the other (a Callback Hell-ish way) (see lecture notes distributed)
    * this isn't awesome, hence the "hell" reference
  * Another approach is "chaining" functions together, starting with the first async function, then followed by higher order functions that perform what we need to do 
      * which means that the next one in the chain won't trigger until the first one is complete (leverages the nature of blocking code)
      * this will only work if we use a Promise for the first func in the chain
        * then we call the second func in the chain on the back of the promise constant
          * That means that the second function in the chain will not be triggered until Promise is resolved (see Promises below)
        * the first function will return a Promise, and we will store that in a variable (const promise = fs.readfile(data, encoding)
          * we normally can't store fs methods in variables because the variable will execute synchronously before the async fs method has returned the value we want

## Promises

* need const fs = require('fs)'.promises
* a promise is a Class Object that has states
  * "Promise { <pending> }" before it's populated
  * Promise object will resolve or reject at a later time, as it shifts states from pending
* By using a Promise, you don't need to pass in a CB to the fs function (because it will return a promise (YAY RETURN IS BACK) instead of calling a callback)
* Create Promise using:
```js
const p1DataPromise = new Promise ((resolve, reject) => {
  resolve(5); // the thing you pass into resolve is the value the callback (.then) gets passed
  reject('bad stuff happened')
});
//then see what it resolved to by calling .then on the back of it (note that resolvedValue is 5 in this example)
p1DataPromise.then((resolvedValue) => console.log('resolvedVal', resolvedValue));
```
* console.logging the promise before it's resolved (synchronously; not by calling .then on it) will return Promise { 5 } 
* console.logging the resolvedValue after .then has been called will return 5 (it's not a promise object anymore; it's the value itself which we can then do stuff with)
  * the .then function returns the resolved value that promise was promising

### Unhappy Path 

* What happens if stuff goes sideways?
  * The promise object will now look like Promise { <rejected> 'bad stuff happened' }
  * we get an unhandled promise message from the promise object if we try to log the resolved value
```js
const p1DataPromise = new Promise ((resolve, reject) => {
  resolve(5);
  reject('bad stuff happened'); //imagine this case this time
});
p1DataPromise.then((resolvedValue) => console.log('resolvedVal', resolvedValue)); //this will log the unresolved promise note
```

* we resolve this with a catch statement
  * read this .then.catch in an if/else flow
```js
p1DataPromise.then((resolvedValue) => console.log('resolvedVal', resolvedValue));
.catch(err => console.log('err', err));
```

### Putting it All Together
* So how do we use Promise to solve our [classwork problem?](/Users/nicoleticknor/lighthouse/w2/d4/w2d4_classwork/01_classwork.js)

```js
const readFilePromise = function(fileName, enc = 'utf8') { // cool new way to default a parameter if no argument is passed
  return new Promise((res, red) => { //need to return the promise!!
    fs.readFile(fileName, enc, (err, data) => { //just pass in enc and it will use enc = 'utf8' as the argument
      if (err) {
        rej(err)
      }
      res(data); //if we want more than one outcome passed into resolve, we need to wrap it in an object such as ({ file: data, filelength: data.length})
    })
  })
}
const p1Promise = readFilePromise('data/p1.txt') //this has to return the promise to store as p1Promise
p1Promise.then(fileData => { /* whatever we want */ }); 
```

## Multiple Promises at Once 

### Promise.all

* If we don't care the order of resolution, we don't need to chain things together and do things sequentially
* We can use Promise.all instead
  * waits for all promises in an array to be resolved
  * returns a new promise that resolves to an array with eelemnts corresponding to the initial elements
  * if one fails, the whole array fails

```js
const array = [
  asyncfunc1(), 
  asyncfunc2(), 
  Promise.reject('some error'), //adding this error in
  asyncfunc3()
  ];
const resolvedArray = Promise.all(array); //console.logging this will produce an array of all the resolved values if they are all resolved. If one fails, the whole thing will fail, and consolelogging it will produce the Promise { <pending> } 
resolvedArray.then((results) => { /* whatever */ });.catch(err => console.log(err));
```

### Promise.race

* Instead of waiting for them all to resolve, resolves once the first one is resolved
* If you're just looking for the first place to return that data you're looking for (like if you're asking multiple search engines for the same search results and don't care which search engine returns first)

```js
const array = [
  asyncfunc1(), 
  asyncfunc2(), 
  asyncfunc3()
  ];
const resolvedArray = Promise.race(array); /
resolvedArray.then((results) => { /* whatever */ });.catch(err => console.log(err));
```

## Closing Activities

Attempt the questions [here](https://gist.github.com/hafbau/d6a023b7aff7f0dae80c11d4c23ec026) 
