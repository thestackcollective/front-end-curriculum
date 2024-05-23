---
title: Closures
tags: javascript, scope, closure, iife, lexical scope
module: 2
---

## Learning Goals

* Describe and speak to lexical scoping in JavaScript
* Understand and implement closures
* Identify scenarios where closures can be useful in code, and use closures to solve specific programming challenges

## Vocab

- `lexical scope` in JavaScript means that the accessibility of a variable is determined by its location in the source code and the structure of nested functions.
- `closure` a function that has a reference to its outer/lexical environment

## Some Context

Closures are an important concept in programming, helping create modular and reusable code. Variables are typically declared within a specific scope, such as a function, and are used only within that scope and any nested scopes. Closures allow a function to access and manipulate variables outside its own scope.

Closures can also improve security by preventing unintended access to sensitive data. They are particularly useful in functional programming, where functions are treated as first-class citizens. Understanding scope, especially **lexical scope**, and closures can help design more effective software solutions.

## Lexical Scope

**Lexical scoping** means that the scope of a variable is determined by its location within the code at the time of its declaration. The scope is defined by the surrounding block structure. This makes the code more predictable and reliable because the scope is fixed at the time of writing and does not change during execution.

```js
function eatSnack() {
  let hunger = 25;

  getHangry();

  function getHangry() {
    console.log(`I am sooooooo HANGRY! My hunger level is ${hunger}.`);
  }
}

eatSnack();
```

<section class="answer">
### Explaining It Further 

In the example above, the lexical scope of the `getHangry` function includes the scope and variables within `eatSnack`.

Inner functions are bound to their parent context during the Creation Phase, based on where they are defined in the source code. Since `getHangry` is defined inside `eatSnack`, it cannot be called outside of `eatSnack` due to its limited scope.
</section>


## Closures 

[Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures){:target="\__blank"} are functions that capture and remember their lexical environment. Another way you can describe it is **a closure is when a nested function can access variables from its outer function**.

<section class="call-to-action">
### Closure Example 
 
```js
function greet() { 
  const firstName = 'Alan';

  function displayName() {
    console.log(firstName);
  }

  displayName();
} 

greet(); 
```


With a partner, walk through the code execution above.
</section>

<section class="answer">
### How is this code executing? What details did you note?

1. After `greet` has been declared and invoked, it creates a variable `firstName` and declares a function.  The `displayName` function is then immediately invoked.
1.  When we get to the console log for `firstName`, the JS interpreter first looks in its current scope (within `displayName`) for a `firstName` variable that it can grab the value from.
2. It doesn't find it, so it traverses up the scope chain to the parent scope (`greet`) and again looks for a `firstName` variable to reference.
3. It finds it here, with a value of `Alan`, so the log will say `Alan`.

To sum it up, this example proves that functions can define functions.  This also demonstrates lexical scoping due to our `displayName` function having access to the `firstName` variable, even though it's executed later.
</section>

## Expanding On Our Closure Definition
Now let's modify this example a bit. Instead of invoking `displayName` right away within our `greet` function, we'll return it:

<section class="call-to-action">
### Updated Example:

```js
function greet() { 
  const firstName = 'Alan'; 

  function displayName() {
    console.log(firstName);
  }

  return displayName;
} 

var createGreeting = greet();
```

- What is the value of `createGreeting`?  Are you able to access the `displayName` function from here?
</section>

<section class="answer">
### Breaking It Down Further

Note that when we invoke `greet`, we get a function back (`displayName`). In many languages, as soon as `greet` is finished running, our `firstName` variable would be completely removed from memory.

In JavaScript this isn't the case. Because there is an inner function here `displayName` -- JavaScript is smart enough to know that this function can still be called (we're going to invoke it with `createGreeting()`) and therefore it still needs access to the `firstName` variable to log "Alan".
</section>

<section class="note">
### "What is a closure?"

This is why the question, "What is a closure?", has become such a big question in JavaScript interviews.  Often people coming from other languages are really surprised by this different behavior since normally the `firstName` variable would not be able to be referenced.

So our newer, better definition of a closure could now be: **When an inner function has access to the outer function's variables and can remember the environment in which it was created.** (*So not only does our inner function have access to the outer function's variables, but it also remembers their values, even if the outer function has finished executing!*)
</section>

## Closures for Protecting Variables

While closures might not seem immediately useful, they have practical applications. Some key points to understand from the previous example is:

* `firstName` is only accessible within the `greet` function, meaning `greet` "closes over" this variable.
* The inner function `displayName` can access (or modify) `firstName` because it is within the scope of `greet`.

One practical use of closures is to **protect variables from external manipulation**. Unlike other languages with built-in private or protected variables, JavaScript uses closures to achieve this.


<section class="call-to-action">
### Practice

Take the following example:
```js
function analyzeGrades() {
  // We keep these variables private inside this closure
  let privateGrades = [93, 95, 88, 0, 55, 91];
  
  return {
    changeGrades() {
      privateGrades = privateGrades.map(grade => {
        return grade + 5;
      });
    },
    
    viewGrades() {
      return privateGrades;
    }
  }
};
  
console.log(privateGrades)
console.log(analyzeGrades)
console.log(analyzeGrades())

let instructor = analyzeGrades();
instructor.changeGrades(); // What is this doing here?
console.log(instructor.viewGrades());
```

1. First predict what each `console.log` will output.
2. Then add a new function, `addGrade`, to this closure that updates our `privateGrades` variable, adding a new grade to the array.
</section>

<section class="note">
### Defining closures one final time!

Our most thorough definition of a closure is now **when an inner function has access to the outer function's variables and can remember the environment in which it was created. The outer function's variables are protected by the closure and can only be manipulated by code defined within that function.**

However, closures don't provide true privacy. While they can hide variables, those variables can still be accessed if someone really tries. Thus, closures aren't suitable for protecting truly sensitive data.
</section>


<section class="answer">
### Practice in small groups
Consider the following situation. I want to create function that helps manage rent prices, but keeps the actual rent amount private through closure. 

```js
const rentPrice = (initialRent) => {
  // your code goes here
}

var rent = rentPrice(8000);
  
// Update data using private methods 
console.log(rent.getRent()); // returns 8000
console.log(rent.incRent(2000));
console.log(rent.decRent(1500));
console.log(rent.decRent(1000)); 
console.log(rent.incRent(2000)); 
console.log(rent.getRent()); // returns 9500
```
***Hint:*** `rentPrice` should return a series of functions for updating our confidential rent. Take note of the dot notation used to invoke these private methods. 

</section>

## Conclusion

Closures are functions that capture and remember the environment in which they were created, allowing access to outer function variables. They enable the creation of private variables and can lead to efficient, modular, and maintainable code.  While implementation of closures is **NOT** expected in your projects moving forward, they will become increasingly relevant, especially in advanced topics such as *higher-order functions* and *memoization*.

<section class="checks-for-understanding">
### Checks for Understanding 

- What is lexical scope?
- What is a closure and how can it be helpful?
- What is an example of a scenario where closures could be useful in code?
- Can you think of an example in your current or recent project where a closure would have been useful?
</section>

<section class="call-to-action">
### A Real World Example

Next week, we'll have a follow up session to explore how a closure can be used in a larger application setting.  If you're curious to explore sooner, you can clone down and explore this [IdeaBox repo](https://github.com/Kalikoze/fp-ideabox){:target="\__blank"} that implements a closure.  This project also follows a number of other functional programming conventions including keeping functions pure.
</section>

### Additional Resources
* [practical uses for closures](https://medium.com/@dis_is_patrick/practical-uses-for-closures-c65640ae7304){:target="\__blank"}
* [closures in depth](https://javascript.plainenglish.io/heres-that-resource-on-javascript-closures-you-were-looking-for-95e82b8108f2){:target="\__blank"}
* [memoization](https://www.geeksforgeeks.org/javascript-memoization/){:target="\__blank"}
* [simple closures analogy](https://blog.codeanalogies.com/2018/10/19/javascript-closures-explained-by-mailing-a-package/){:target="\__blank"}