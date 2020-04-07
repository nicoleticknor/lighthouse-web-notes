# Lecture notes from Monday

## TDD, Testing in General, and Aggregate Random notes
* Benefits of TDD: 
  * Refactor without fear of breaking other code
  * Don't have to keep everything in your head; the tests will pass or they won't

- const assert = require('assert') <--without the relative path ./ or ../ etc, it will look in the node_modules folder for the package
- when naming files, you can do lots of . if you want. But the thing after the final . will be the file extension (file type) such as .js or .text
so file.js.txt is a text file
- module.exports = {func1, func2} to export multiple funcs from one file (if value = key, we don't need to put "func1: func1," because of ES6 shorthand)
- then you will have a require('../whole file name') and you will need to make a destructuring { func 1 } = require etc
  - you can also make another const and the call funcs on the back of it, such as const helpers = require('../whole file name'); and then call helpers.func1
  - we can also export whatever else we want in module.exports = {}; 
      ```javascript
      const maxNumStudents = 10;

      module.exports = {
        func1, 
        func2,
        maxNumStudents
      };
      ```
### Testing Tips

- make sure you write good tests so that there's not a cheater way of passing that's not actually doing the thing intended
    - for example, return thisIsAString will pass expect thisIsAString, but it won't camel case anything, so also expect to make "loopyLighthouse" a passing result (for example)

## npm
- it's like the Twitch of node packages
- to gauge a package's credibiltiy, look at number of weekly downloads
- also important is the last published date - too long ago and there will be security holes since last it was patched
- run npm init to get the package.json file prompts:
  - for get repository, create a new repo if you haven't already, and add the SSH url to that line
  - keywords are how devs can find packages on npm's search
  - within devDependencies, the ^ karat in front of the version number means it is forward-compatible with versions released after that one

- check what version you're running of a package (or any process, really) with the --version flag
- don't commit node_modules. Put them in your .gitignore file. I just realized I did this for lotide, and did not commit the package-lock.json.
- in the command line when installing packages, use --save-dev if production env doesn't depend on the package (i.e. something like mocha or chai, which are testing tools)


## Mocha

- the describe funcs is a way to group test blocks also create headings in the run test output. 
  - describe can be nested within describe, but the last callback describe takes is "it()" which takes a callback that is your test code block
  - if the code inside the it function throws an error, the test fails. if the code inside the it function DOES NOT throw an error, the test passes
    - this means that an empty code block will pass a test. Sooooo don't open callbacks until you're ready to populate them
    - This is how we can rely on our assertion library (because it will throw errors if assertions don't pass). 
    - Mocha will let us keep running the file even if we have a thrown error inside an if block

Google Maps - random google maps view for loading page

## CHAI

- chai is an assertion library. Mocha does all the testing for us, chai is just where we can call some fun fun properties of chai.
- Chai gives us more assertions than Node.js has itself.  SO MANY NEW ASSERTIONS. Node.js only has the bare min, about 20 assertion funcs
  - This is why not everyone / every project using Mocha also uses Chai; they may not need those assertions
  - but with Chai, we can use more targeted assertions. This is analagous to using reduce instead of a loop and a separate declaration statement
- When we use chai, we don't need to const assert = require('assert'), instead we do the require('chai'), because we aren't using Node's assert anymore
  - we are using chai's assert, so must also include const assert = chai.assert

