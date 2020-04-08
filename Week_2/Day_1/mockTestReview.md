# Aggregating Random Notes

- c-style loop is a throwback to the C language
- the spread operator will duplicate the original: 
        ```javascript
        const ary = [1, 2, 3];
        const newAry = [...ary];
        console.log(newAry); // [1, 2, 3]
        ```
- industry standard is to comment on the line above the code that the comment refers to
- if you're setting up a primitive variable that will be populated within a method or loop or something, set it first to null (or zero). That way it's not undefined, so it's easier to track where the issue is coming from in testing