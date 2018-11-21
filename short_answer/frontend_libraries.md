# Front End Libraries and Frameworks

## What is [Bootstrap](https://getbootstrap.com/) ?

Bootstrap is a front-end framework containing HTML, CSS, and JavaScript.  
It allows us to quickly build up web applications.  

## What is the relation between the grid system and RWD?

The grid system is one of the easiest ways to achieve responsive web design.  
It helps align page elements based on sequenced columns and rows.  

## Find another library which is similar to Bootstrap

- [Pure.css](https://purecss.io/)
- [Materialize](http://materializecss.com/)
- [Semantic UI](https://semantic-ui.com/)
- [Bulma](https://bulma.io/)

## What is [jQuery](https://api.jquery.com/)?

jQuery is a JavaScript library designed to simplify the client-side scripting of HTML.  
It looks like these:

```js
/* DOM Traversal and Manipulation */

$( "button.continue" ).html( "Next Step..." );
// Get the <button> element with the class 'continue' and change its HTML to 'Next Step...'


/* Ajax */

$.ajax({
  url: "/api/getWeather",
  data: {
    zipcode: 97201
  },
  success: function( result ) {
    $( "#weather-temp" ).html( "<strong>" + result + "</strong> degrees" );
  }
});
// Call a local script on the server /api/getWeather with the query parameter zipcode=97201 and replace the element #weather-temp's html with the returned text.
```

Reference: [You might not need jQuery](http://youmightnotneedjquery.com/)

## What is the relation between jQuery and vanilla JS?

**Vanilla JS** is JavaScript - the language - itself and jQuery is a JavaScript library.  
Imagine that you are building a Lego city. Vanilla JS is like Lego pieces while jQuery is like bridges, houses, roads which are easy to use and speed up the building process.  

jQuery is good for small projects. When it comes to big ones, it's better to build up your own library. By doing so, you can control the quality.