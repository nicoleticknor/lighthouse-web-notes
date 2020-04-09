# Day 3: Asynchronous Programming Continued

## Random Notes Section

* Typing /** into the code editor will nicely format your comment block
* For loops don't need semi-colons

## But First! Reviewing Recursion

### Notes from reading [Eloquent JavaScript's article](https://eloquentjavascript.net/03_functions.html#h_hOd+yVxaku):

* Whenever you have a loop, you can use recursion, but not vice versa
  * Just because you can use recursion, doesn't mean you should. It may be more difficult to read, and take longer to execute, than a loop.
* Recursion is often better than a loop in problems with branching data, like MPJ's taxonomy object example
* Discreet quantum physics example! Take the number 1 and then repeatedly do one of two things: either add 5, or multiply 3. 
  * The possibility of either add 5 OR multiply 3 creates branching, like a NPV node in a financial decision tree.
```js
  //function takes a target number to reach with the operations
function findSolution(target) {
  //function takes in the latest increment via those operations, and the spelled-out record of what operations those were
  function find(current, history) {
    //function compares the latest increment via those operations with the target
    if (current == target) {
      //if the number we are at is the same as the target number, we are done with the operations, and the function returns the spelled-out record of what operations took place
      //this is the base case
      return history;
    } else if (current > target) {
      //if the operations caused the number we are at to be greater than the target, function returns null (which I'm assuming is if there is no possible solution)
      //is this another form of a base case??
      return null;
    } else {
      //but if we haven't reached the target yet, we keep going with more operations. 
      //note that find() call itself twice - if the first call returns something that is not null, it is returned, otherwise, the second return string is called. 
      //i don't know about return x || y yet though
      return find(current + 5, `(${history} + 5)`) ||
        find(current * 3, `(${history} * 3)`);
    }
  }

  //this gives us a starting point for the function, where current is 1 and history is the starting point of record, "1".
  //this being passed in to find is how the chain of events starts in the first place
  return find(1, "1");
}

console.log(findSolution(13));

/*so here's how the branching looks for findSolution(13)

find(1, "1")
  find(6, "(1 + 5)")
    find(11, "((1 + 5) + 5)")
      find(16, "(((1 + 5) + 5) + 5)")
        too big
      find(33, "(((1 + 5) + 5) * 3)")
        too big
    find(18, "((1 + 5) * 3)")
      too big
  find(3, "(1 * 3)")
    find(8, "((1 * 3) + 5)")
      find(13, "(((1 * 3) + 5) + 5)")

      at this point, current == target, so the history string is returned
*/
```


## Closures (again)

Notes from reading [Mozilla's documentation of Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures):
* "A closure is the combination of a function and the lexical environment within which that function was declared". 
  * the "closure" of the first function init is a combo of const name and function displayName. 
```js
function init() {
  const name = "Mozilla"; //name is a local variable created by init
  // displayName() is the inner function, aka a Closure
  function displayName() {
    console.log(name); // closure uses the variable declared in the parent func
  }
  displayName(); //calls the closure to acquire name 
}

init(); // calls the parent func, which establishes name, and then invokes closure to acquire name.  
//This works similarly to just having stated return "Mozilla", but uses a callback instead.
```
  * myFunc (initially declared as a constant; that's possible because functions are a datatype) references a specific instance of displayName - the one that is created when makeFunc() is invoked. That passes 'Mozilla' to displayName as the instantiation
  * 
```js
function makeFunc() {
  const name = 'Mozilla' //same as before
  function displayName() { //same closure as before
    console.log(name);
  }
  return displayName; // this returns the function (not an invocation of it)
  //THIS IS AMAZING. 
  //We are now able to store a function that was previously ONLY ACCESSIBLE INSIDE THE PARENT FUNCTION as a variable in the global scope, as we do below. 
  //Also - our closure includes a variable that was previously ONLY ACCESSIBLE INSIDE THE PARENT FUNCTION, which has now also been brought into the global scope. Phase shifting yo
}
// makeFunc produces a function displayName. we are storing that production ability in a variable called myFunc
const myFunc = makeFunc(); 
//invoking myFunc will trigger the returned function to invoke, performing its action (which is console.log the local variable "name")
myFunc(); 
```

* The above instantiation is fixed because const name is declared inside the parent. HOWEVER, we can extract the instantiation command to the parent into the global scope, by passing it in to the parent as an argument. 
  * This is awesome, because now we can store multiple instantiations of the parent function as variable, and invoke them later with whatever arguments we need to
  * extra cool if the inner function itself takes a parameter, because then we can invoke a desired parent instantiation with an argument that gets passed in to the inner func

```js 
// here we're passing in a variable x to a parent function
function makeAdder(x) {
  //that parent function's job is to return a function that requires some variable y
  return function (y) {
    //that inner function's job is to return x + y
    return x + y;
  };
}

//we can store instantiations of the inner function as variables!!!!!
const add5 = makeAdder(5);
const add10 = makeAdder(10);

//and then we can invoke the inner function by passing in a y variable for that instantiation of the parent:

//so add5 is the instance of makeAdder where x = 5, and below we are invoking that where y = 2
console.log(add5(2)); // -> 7

//add10 is the instance of makeAdder where x = 10, and below we are invoking that where y = 3
console.log(add10(3)); // -> 13
```
* This becomes practical (essential, even) for event-based programming, where the result (the code we want to execute in response to an event) is contained in a callback
  * the MDN example uses some HTML, so I will pick this up again in a few days/week or so.

## Tech Interview #1 Feedback

* Prep for an interview by:
  * trying some CodeWars stuff
  * coding on pen and paper
* Approach the problem with the simplest functional solution first. THEN worry about refactoring and edge cases and all that. 
* Being able to speak my mind while coding / working through an algorithm is one of my relative strengths.
* Don't forget to use the linter (if it's there)

## Snek!

* That was fun.

## HTTP

* HTTP is the language used on the back of a TCP connection. They both happen to be text-based.
  * TCP is the protocol, HTTP is the language
* for websites, domain name and IP are interchangeable. So host: 'domain'
* most websites using HTTP are on port 80; HTTPS are on port 443 (except websites trying to hide)
* HTTP uses methods GET, POST, etc so that the client and server can transact

## Page Downloader Challenge Exercise

## Pair Programming Word Search

* THERE'S AN ARRAY.INCLUDES METHOD!!!!!!!!!!!!!!

## Fetcher

* Read [Callback Hell](http://callbackhell.com/)
* Stretch and some edge cases incomplete
* Libraries used: 
  * npm request (deprecated)
  * Node's file system
  * Node's readline
  * Node's process
