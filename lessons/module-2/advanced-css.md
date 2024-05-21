---
title: "Advanced CSS: Variables, Nesting, & Imports"
length: 120
tags: css, practice
module: 2
---

## Learning Goals
* Understand the benefits of using variables in CSS
* Master the concept of nesting in CSS
* Learn how to use imports to modularize CSS files

## Introduction
In this lesson, we will delve into advanced CSS techniques that go beyond basic styling. These techniques include the use of variables, nesting, and imports, which are essential for maintaining scalable and organized CSS codebases.

## Variables in CSS
CSS variables, also known as custom properties, allow you to store and reuse values throughout your stylesheets. This promotes consistency and makes it easier to update styles across your project.

#### Variable Declaration Syntax
```css
:root {
  --primary-color: #007bff;
  --secondary-color: #6c757d;
}

.subtitle {
  color: var(--primary-color);
  background-color: var(--secondary-color);
}
```

#### Example Usage
```html
<h1 class="subtitle">Hello, CSS Variables!</div>
```

In this example, we define primary and secondary colors as CSS variables and apply them to an HTML element. This way, if we need to change the color scheme of our website, we only need to update the variable values in one place.

<section class="note">
### Other Ways of Declaring Variables

While it is mostly common to define variables at the root level, you can also define them within a specific selector.  This can be helpful if you want to define a variable that is only used within a specific selector.  Note that variables will cascade down the CSS file similar to how other CSS styles cascade down the file.  If you define a variable at the root level, it will be available to all selectors within the file.  If you define a variable within a selector, it will only be available to selectors within that specific selector.  Consider the following:

**HTML**:
```html
<div class="container">
  <div class="header">Header</div>
  <div class="content">Content</div>
</div>
```

**CSS**:
```css
:root {
  --main-color: #3498db;
  --padding: 10px;
}

.container {
  --main-color: #2ecc71; /* Overriding the root variable */
  padding: var(--padding);
  background-color: var(--main-color);
}

.container .header {
  --main-color: #e74c3c; /* Overriding the container variable */
  padding: var(--padding);
  background-color: var(--main-color);
}

.container .content {
  padding: var(--padding);
  background-color: var(--main-color); /* Inherits from .container making the value #2ecc71 */
}
```
</section>

## Nesting in CSS
Nesting allows you to define styles for nested elements in a more intuitive and readable way. This is particularly useful when working with complex HTML structures.

#### Nesting Syntax
```css
.navbar {
  background-color: #333;
  
  ul {
    list-style-type: none;
    
    li {
      display: inline-block;
      
      a {
        color: #fff;
        text-decoration: none;
      }
    }
  }
}
```

#### Example Usage
```html
<nav class="navbar">
  <ul>
    <li><a href="#">Home</a></li>
    <li><a href="#">About</a></li>
    <li><a href="#">Services</a></li>
  </ul>
</nav>
```

In this example, we define styles for a navigation bar and its nested list items and links. The nesting structure mirrors the HTML structure, making it easier to understand and maintain the CSS code.

<section class="answer">
### CSS Without Nesting

Here is what the CSS would look like without nesting:
```css
.navbar {
  background-color: #333;
}

.navbar ul {
  list-style-type: none;
}

.navbar ul li {
  display: inline-block;
}

.navbar ul li a {
  color: #fff;
  text-decoration: none;
}
```
</section>

<section class="note">
### Use nesting with CAUTION

Be aware that having _too_ much nesting can be a problem - resulting in hard to maintain CSS that is overly specific (remember specificity?). Try to avoid excessive levels of nesting unless absolutely necessary.
</section>

## Imports for Modularization
CSS imports allow you to split your stylesheets into smaller, modular files, making your codebase more maintainable and organized.

#### Import Syntax
```css
@import 'reset.css';
@import 'variables.css';
@import 'layout.css';
@import 'components/buttons.css';
/* Additional imports */
```

By importing separate CSS files, you can compartmentalize styles for different components or sections of your website, improving code readability and facilitating collaboration.

## Summary
In this lesson, we explored advanced CSS techniques including the use of variables, nesting, and imports. These techniques empower developers to write more maintainable, scalable, and organized CSS code. By mastering these concepts, you'll be better equipped to tackle complex styling challenges and build more efficient web applications.

Now, let's put your knowledge into practice with some exercises.

<section class="call-to-action">
### Exercise

Let's get some practice with these concepts working with a static e-commerce website .  Clone down this [repo](https://github.com/turingschool-examples/advanced-css-ecommerce-dashboard){:target="_blank"} following the setup instructions and work through the iterations in the instructions to refactor the site using what you've learned!

Once you have finished the exercise, you can compare your solution to the solution in the `solution` branch.
</section>

<section class="checks-for-understanding">
### Checks For Understanding

* What is the purpose of using variables in CSS?
* Why is nesting useful for organizing CSS code?
* Explain the benefits of modularizing stylesheets using imports.
</section>

## Further Resources
- [MDN Web Docs: CSS Variables](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties){:target="_blank"}
- [MDN Web Docs: Using CSS nesting](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting/Using_CSS_nesting){:target="_blank"}
- [MDN Web Docs: @import](https://developer.mozilla.org/en-US/docs/Web/CSS/@import){:target="_blank"}