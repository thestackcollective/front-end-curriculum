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

- `lexical scope` also known as static scope. Lexical scope in JavaScript means that the accessibility of a variable is determined by its location in the source code and the structure of nested functions.
- `closure` a function that has a reference to its outer/lexical environment

## Some Context

Closures are an important concept in programming that can help developers to create more modular and reusable code. Typically variables are declared within a specific scope, such as a function, and are then used only within that scope and any nested scopes. Closures, on the other hand, allow a function to access and manipulate variables that are declared outside of its own scope.

In addition to creating more efficient and maintainable code, closures can also improve security by preventing unintended access to sensitive data. They are particularly useful in functional programming, where functions are treated as first-class citizens and can be passed as arguments or returned as values. Understanding how scope, more specifically **lexical scope**, and closures work can help design more effective and flexible software solutions.

## Lexical Scope

Though we haven't put a name to it until now, our conversations around scope and the scope chain are directly related to what we call **lexical scoping**.

Lexical scoping means that our scope is determined by its location within the code at the time of its declaration.  Due to variables like `let` and `const` being **statically** bound, meaning the relationships between variables, functions, and their values are determined in the `creation-phase` (*also known as build time*), our code is more predictable and reliable in behavior. You can see lexical scope by looking directly at code below:

```js
function eatSnack() {
  let hunger = 25;

  getHangry();

  function getHangry() {
    console.log('I am sooooooo HANGRY!');
  }
}

eatSnack();
```

<section class="answer">
### Explaining It Further 

In our example above, the lexical scope for our `getHangry` function is the scope (and any variables) that are contained within `eatSnack`.

All inner functions (since functions create new scope) are statically (lexically) bound during the Creation Phase to the parent context in which the inner function was physically defined in the source/program code. Said another way, the `getHangry()` function is defined inside the definition of the `eatSnack()` function - because of this, we can't call `getHangry()` outside of the `eatSnack()` function due to its scope being limited within.
</section>


## Closures 

[Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) are expressions (usually functions) which can work with variables set within a certain context. In other words, a closure is formed when a function is defined inside of another function (one function nested inside of another function). This allows the inner function to access the outer function's variables via the scope chain. 

Let's build on a definition together. At the most basic level, **a closure is when an inner function is defined inside of another function**.

This is syntactically and factually correct, but it doesn't tell us much about what's significant here. So a more thorough definition might be:  **a closure is when an inner function has access to its outer function's variables**.

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

1. After `greet` has been declared and invoked, it creates a variable `firstName` and declaring a function, it immediately invokes `displayName`.
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

var createGreeting = greet(); // createGreeting is now a function that can be invoked
createGreeting(); // will run the displayName function and log 'Alan'
```

- What is the value of `createGreeting`?  Are you able to acess the `displayName` function from here?
</section>

<section class="answer">
### Breaking It Down Further

Note that when we invoke `greet`, we get a function back (`displayName`). In many languages, as soon as `greet` is finished running, our `firstName` variable would be completely removed from memory.

In JavaScript this isn't the case. Because there is an inner function here `displayName` -- JavaScript is smart enough to know that this function can still be called (we're going to invoke it with `createGreeting()`) and therefore it still needs access to the `firstName` variable to log "Alan".
</section>

<section class="note">
### What is a closure?

This is why "What is a closure" has become such a big question in JavaScript interviews.  Often people coming from other languages are really surprised by this different behavior since normaly the `firstName` variable would not be able to be reference.

So our newer, better definition of a closure could now be: **When an inner function has access to the outer function's variables and can remember the environment in which it was created.** (So not only does our inner function have access to the outer function's variables, but it also remembers their values, even if the outer function has finished executing!)
</section>

## Closures for Protecting Variables

This still might not seem like an incredibly useful feature of JavaScript, but there are some more practical use-cases for closures. Before we move on, What we want to point out about this example, in order to better understand closures, is that:

* `firstName` is not available anywhere outside of the `greet` function. We would say that the `greet` function "closes over" this variable
* the inner function, `displayName` can reference (or even modify) the `firstName` variable because it is inside the scope of the outer function `greet`

So one use-case for closures is to **protect variables from any sort of outside manipulation (we're not talking about security here)**. Other languages often have some really nice way of implementing private or protected variables, but JavaScript doesn't. So if this is something we want to achieve, we would use a closure.


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

In the previous example, you'll notice we could still technically change those grades and snoop on them if we wanted to. This is why we say JavaScript doesn't have a true fashion for creating private variables. As developers, we can hide a variable declaration inside a function in order to imply that no one should be fussing with that variable - but we can still gain access to that value. So closures aren't really going to help if you have truly sensitive data that nobody should be able to see.
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
***Hint:*** `rentPrice` should return a series of functions for updating our confidential rent. Take note of the dot notation used to invoked these private methods. 

</section>

## Conclusion

 Closures are functions that capture and remember the environment in which they were created, and can be used to create private variables. Implementation of closures can result in efficient, modular, and maintainable code, and avoid common pitfalls such as global namespace pollution, code duplication, and side effects. While implementation of closures is **NOT** expected in your projects moving forward, they will become even more relevant when working with React functional components in the future. They are also used in more advanced topics such as *higher order functions* and *memoization*, which you'll explore more in the future.

<section class="checks-for-understanding">
### Checks for Understanding 

- What is lexical scope?
- What is a closure and how can it be helpful?
- What is an example of a scenario where closures could be useful in code?
- Can you think of an example in your current or recent project where a closure would have been useful?
</section>

<section class="call-to-action">
### A Real World Example

Next week, we'll have a follow up session to explore how a closure can be used in a larger application setting.  If you're curious to explore sooner, you can clone down and explore this [IdeaBox repo](https://github.com/Kalikoze/fp-ideabox){:target="\__blank"} that implements a closure along with a number of other functional programming practices.
</section>

### Additional Resources
* [practical uses for closures](https://medium.com/@dis_is_patrick/practical-uses-for-closures-c65640ae7304){:target="\__blank"}
* [closures in depth](https://javascript.plainenglish.io/heres-that-resource-on-javascript-closures-you-were-looking-for-95e82b8108f2){:target="\__blank"}
* [memoization](https://www.geeksforgeeks.org/javascript-memoization/){:target="\__blank"}
* [simple closures analogy](https://blog.codeanalogies.com/2018/10/19/javascript-closures-explained-by-mailing-a-package/){:target="\__blank"}