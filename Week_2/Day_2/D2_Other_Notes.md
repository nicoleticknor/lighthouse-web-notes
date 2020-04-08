#Day 2: Asynchronous Programming

## Topic 1: setTimeout function

### hello_timeout(/Week_2/Day_1/hello_timeout)
* delay set with an anonymous callback function that takes index and setTimeout
  * within the loop / forEach, callback is the anonymous function with nested setTimeout
### typewriter
* process.stdout.write reminds me of process.argv. 

## Topic 2: Closures

* Because we can't return x in an asyncronous process, we have to return a function that we can then invoke later that returns x 
  * this is referred to as a callback function
  * Using the callback function, we can store x for later use. 
  * By later calling that (callback) function, we can produce x when we need it and store it in a variable, on whatever delay we decide. 
* Example from https://eloquentjavascript.net/03_functions.html#h_hOd+yVxaku
```js
function wrapValue(n) {
  const local = n;
  return () => local; // it returns a a function that creates
};

/*so by invoking that function later with some value for n (specifically, data we acquired somewhere else asyncronously, passed in), we can assign n to a variable by way of that function. They call it a "binding", which is cool.

*/
const wrap1 = wrapValue(1);
console.log(wrap1()); // -> 1

// AND importantly - we can do other stuff to n. We can call whatever function operations on it we want inside the callback

function multiplier(factor) {
  return number => number * factor;
};

const twice = multiplier(2);
console.log(twice(5)); // -> 10
```

## # Topic 3: Recursion
