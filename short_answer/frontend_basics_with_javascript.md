# Front End Basics with JavaScript

##  What is DOM?

DOM (document object model) is a web API constructed as a tree of Objects.  
We can use JavaScript to manipulate HTML elements with DOM.

## What is Ajax

Ajax (asynchronous JavaScript and XML) is a web technique that a web page can be seprated to different parts, each of them can be updated not at the same time after the page has loaded, and without reloading the whole page. We send a request and use a callback function to wait for the response.  

## List some HTTP methods

`GET` and `POST` are commonly used.  

We use `GET` to get data from a specified resource, placing parameters at the end of the url after a question mark, using '&' to connect one another:
```
?game_id='123'&first='100'
```

`POST` usually send data through forms.  
It's a better way when you don't want the data to be saved in the browser history.  

Other HTTP methods:

Method | Description
--- | ---
HEAD | Same as GET but returns only HTTP headers and no document body
PUT | Uploads a representation of the specified URI
DELETE | Deletes the specified resource
OPTIONS | Returns the HTTP methods that the server supports
CONNECT | Converts the request connection to a transparent TCP/IP tunnel

## Tell the difference between GET and POST

GET | POST
--- | ---
Can be bookmarked | No
Can be cached	| No
It has length limit (as url has) | No
Parameters remain in browser history | No, which is safer

## What is RESTful API?

RESTful (representational state transfer) is a style to design APIs.

- Client-server architecture: separating the client side and the server side. Both of them can be developed respectively and it has nothing to do with the API.
- Stateless on the server side: it is the client side that keeps the state.
- Cacheability: reusable response which is good for efficiency.
- Uniform interface: standardize the data-interchange format such as HTTP protocol, JSON, XML... etc.
- Layered system: we don't know and don't have to know about the operation behind the part we directly interact with. 

## What is JSON?

JavaScript Object Notation is a format used when exchanging data between the client side and the server side.  
It is a JavaScript object including objects and arrays. It's all strings in its content. Comments are not allowed in JSON. 

## What is JSONP?

Because of the same-origin policy, we can only get response from the same domain.  

JSON with Padding can solve this. It does not use the XMLHttpRequest object. It uses the `<script>` tag instead.

## How to makes a cross-origin HTTP request?

- If you just want to send a `GET` request, you can use JSONP.

- CORS (cross-origin resource sharing) - if the server add this on the response header, it allows all domains:

```
Access-Control-Allow-Origin: *
```

## It will not include cookies by default when sending AJAX requests. How to use cookie from AJAX?

AJAX calls only send Cookies if the url you're calling is on the same domain as your calling script. This may be a Cross Domain Problem.  

#### Client side

With `withCredentials` setting to `true`, the browser will pass cookies (credentials) to the server.  

```js
xhrFields: { withCredentials: true }
```

Here is a full example of what the basic AJAX request should look like in jQuery:

```js
$.ajax({
  url : 'http://my-domain.com/corsrequest',
  data : data,
  dataType: 'json',
  type : 'POST',
  xhrFields: {
    withCredentials: true
  },
  crossDomain: true,
  contentType: "application/json",
  ...
```

#### Server side

This header needs to be set to true in the scenario:

```
Access-Control-Allow-Credentials: true
```

And the header `Access-Control-Allow-Origin` can not be `*`.  
It needs to be set as so:

```
Access-Control-Allow-Origin: http://my-domain.com
```