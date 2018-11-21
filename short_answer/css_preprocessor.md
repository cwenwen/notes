# CSS Preprocessor and CSS Specificity

## What is CSS preprocessor? Do we have to use it?

A CSS preprocessor is a scripting language that extends CSS by allowing developers to write code in one language and then compile it into CSS.  

It allows developers to create more readable and maintainable CSS styles by its features such as mixin, nesting selector, inheritance selector, and so on. 

These are commonly used CSS preprocessors:

- [SASS](https://sass-lang.com/)
- [LESS](http://lesscss.org/)
- [Stylus](http://stylus-lang.com/)

## Explain the CSS specificity hierarchy

If there are two or more conflicting CSS rules that point to the same element, the browser follows some rules to determine which one is most specific and therefore wins out.  

Every selector has its place in the specificity hierarchy. There are four categories which define the specificity level of a selector:  

- 0 - 0 - 0 - 0 - 1  
  **Elements and pseudo-elements**  
  `h1`, `div`, `:before`, `:after`

- 0 - 0 - 0 - 1 - 0  
  **Classes, attributes and pseudo-classes**
   `.classes`, `[attributes]`, `:hover`, `:focus`

- 0 - 0 - 1 - 0 - 0  
  **IDs**  
  `#navbar`

- 0 - 1 - 0 - 0 - 0  
  **Inline styles**  
  `<h1 style="color: #ffffff;">`

- 1 - 0 - 0 - 0 - 0  
  `!important`

Read more: [CSS Specificity](http://cssspecificity.com/)