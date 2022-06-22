# FrontEnd Interview Cheat Sheet

# Table of Content- [Web](#web)
  - [Storage](#storage)
  - [API](#api)
  - [Performance](#performance)
  - [Security](#security)
  - [Testing](#testing)
  - [Accessibility](#accessibility)
- [Language/Frameworks](#language-frameworks)
  - [HTML](#html)
  - [CSS](#css)
  - [Javascript](#javascript)
  - [React](#react)



# <a id="web">Web</a>
- When user types in URL into browser:
1. Enter url into web browser
2. DNS (domain name services) translates human-friendly domain name to computer friendly IP address (IP4 vs IP6)
3. browser sends HTTP request to server
4. server sends back HTTP response
5. browser renders HTML
6. brwoser sends request for additional objects embeded in HTML (images, CSS, JS)
7. Once page is loaded, browser sends further async requests as needed

## <a id="storage">Storage</a>
- **Cookies**: small files on user's computer read by teh server-side (Remember: serve cookies)
  - storage capacity 4kb
  - accessed by either web server or client's computer
  - basically key and value store (hash table)
  - must be parsed to read
  - use:
    - carry info from 1 session on website to another session
    - carry info between sessions on related websites
    - if we stored this data on server without cookies, then user would have to login on each visit
    - cookies can be made to persist for arbitrary amount of time
- **LocalStorage**: stores data on client's computer (Remember: printing shop has local clients)
  - save key/value pairs inw eb browser
  - no expirationd ate
   can only be accessed by JS and HTML5
   - 5MB storage capacity (more than cookies 4kb)
   - data not sent to server for every HTTP request -> dec traffic btw client and server


## <a id="api">API</a>
- **RESTful services**
  - implement CRUD using HTTP methods (GET, PUT, POST, DELETE)
  - transfer XML or JSON or both
  - stateless

## <a id="performance">Performance</a>
To reduce app load time
  - bundle and minify all CSS and JS files
  - **Content Delivery Network(CDN)** - redirects traffic to closest server, which caches content (web videos or bulky media) -> decrease latency of HTTP request if servers are geographically closer to the user and inc download speed
  - **Cache** - reduce DB calls
  - optimize images to less than screen resolution and save as compressed file
  - 1 sprite image for all icon images of app and then use background-position to display it -> reduce HTTP requests
  - test perforamnce with Lighthouse
  - pre-load/pre-fetch data
  - code-splitting
  - service worker offline
  - lazy load
  - autopager
  - infinite scroll
  - SSR/initial data feed
  - list virtualization - limit adding DOM elements to only what's viewable- 
- Images
  - compress images
  - lazy load images
  - progressive images - blurry to clear
  - use SVG for icons
  - CSS sprites -> reduce # of network requests
  - caching through CDN


## <a id="security">Security</a>
- **Cross-Origin Resource Sharing (CORS)** enables resources like JS, fonts, images on a webpage to be requested from an outside domain (recommended standard)
  - `Same-Origin Policy` blocks web apps from making HTTP requests from different origins -> prevents attackers that plant scripts on various websites to make requests to sites with personal data (Ex: ads in Google ads)
  - if server doesn't respond with specific cross-origin headers, browser will not allow JS to access the response
  - uses **CORS headers** to give browser permission to let web app at 1 domain access resources from different domain
- **Cross Site Scripting (XSS)** - security vulnerability where hackers try to inject HTML/Javascript ot steal confidential info from cookies and pass that info to themselves
  - solution: sanitize inputs, escape, don't use eval, update libraries
- **URL manipulation** - security vulnerability where hackers pass info in query string parameters and alter info to get authentication of servers and steal critical data


## <a id="testing">Testing</a>
- **Unit Testing**: validate taht each individual unit of software does what it is intended to do
- **E2E Testing**: test flow of app from start-to-finish, usually from user perspective
- test app in different browsers using tools like BrowserStack

## <a id="accessibility">Accessibility</a>
- alt text
- color contrast
- headers for structure
- form inputs
  - have labels
  - can tab through inputs and keyboard accessible
- allow enlarging fonts
- audio/video/image description
- readable urls
- aria-roles
- tables only for tabular data 

# <a id="language-frameworks"></a>Language Frameworks
## <a id="html">HTML</a>
- put `<script>` tags in `<head>` tags with async/defer
  - async - run scripts and render HTML without blocking
  - defer - run scripts after the other
  - before, `<script>` was put at end of `<body>` tag to not block rendering of HTML b/c we assumed scripts would modify the DOM

## <a id="CSS">CSS</a>
- [CSS Grid](https://css-tricks.com/snippets/css/complete-guide-grid/)
- CSS Specificity
  1. !important - 10000
  2. Inline Style - 1000
  3. ID - 100
  4. Class, Pseudoclass - 10
  5. Element, Pseudo-element - 1


## <a id="javascript">Javascript</a>
- null is an object, so checking typeof as an object is sometimes not enough
```
var bar = null;
typeof bar === "object"; // true
solution: (bar !== null) && typeof bar === "object"); // false
```
- arrays are objects
- if you want false for nulls, arrays, fucntions but true for object:
```
var bar = [];
Array.isArray(bar) OR (bar !== null) && (bar.constructor === "object")
```
- JS supports named and anonymous functions
- `var` vs `let`
  - **var**: function scope
  - **let**: block scope, introduced in ES6 (Ex: for loop - use let)
  ```
  for (var i = 0; i < 5; i++) {
    setTimeout(function() { console.log(i); }, i * 1000)
  }
  // Output: 5,5,5,5 
  // because setTimeout function will be executed after entire for loop runs when i is already 5
  Soln: use `let` (block scope)
  ```
- undefined vs null
  - **undefined**: variable is declared but no value has been assigned
  - **null**: value of null is explicitly set
  - `typeof undefined;  // undefined`
  - `typeof null;  // "object"`
- **Promises**
  - JavaScript promise object can be in one of three states: `pending`, `resolved`, or `rejected`. While the value is not yet available, the Promise stays in the `pending` state. Afterwards, it transitions to one of the two states: `resolved` or `rejected` . A resolved promise stands for a successful completion. Due to errors, the promise may go in the rejected state.
```
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Result");
  }, 200);
});

promise.then(
  (res) => {
    console.log(res);
  },
  (err) => {
    alert(err);
  }
);
```
- Eventing system
```
class Events {
  on(eventName, callback) {...}
  trigger(eventName) {...}
  off (eventName) {...}
}
// events: onClick, onHover, onExit
// can add functions to happen on eventName
```
- **Falsey values** include `false`, `0`, empty strings, `null`, `undefined`, and `NaN`. All other values are truthy.
- **prototypical inheritance**: every object has prop called prototype that you can add methods to it and when you create another object from the newly created, it will automatically inherit its parent's property
- **Closure**: inner function has access to outer function's variables and hold it's environments
- Why do libraries wrap entire ocntent of JS source file in function block? jQuery and Node do this
  - creates closure around entire contents of file
  - creates private namespace -> avoids potential name classes between different JS modules and libraries
- **use strict**: voluntary way to enforce stricter parsing and error handling on your JS code at runtime
  - code errors that woudl ahve been ignored or fail silently will now generate errors or throw exceptions -> debugging easier
  - prevent accidental globals
    - w/o strict mode: assigning value to undeclared variable automatically creates global variable int aht name
  - eliminates this coersion
    - w/o strict mode: if `this` is null/undefined, then it goes up to using global `this`
    - w/ strict mode: use `this` of null/undefined -> error
  - disallow duplicate parameter  `Ex: function foo(val1, val1, val2)`
- **Hoisting**: JS default behavior of moving declarations (but not initializations) to top of scope or function
  ```
  x = 5; //assign
  console.log(x);
  var x; // declare

  SAME AS:
  var x;
  x = 5;
  console.log(x)
  ```
  - initializations (assigning values) are not hoisted
  ```
  var x = 5;
  console.log(x + " " + y); // "5 undefined"
  var y = 7; // declaration var y is hoisted to the top but y=7 is not
  ```
  - best practice: declare all variables to top of scope
    - use strict mode doesn't allow varaibles to be used if not decalred first
- **NaN (Not a Number)**
  - `typeof (NaN === "number") // true`
  - is not equal to itself `NaN === NaN  // false`
  - use: `Number.isNaN(3)`
- **Event Loop**
  - runs items on **call stack**(LIFO) first before `message queue`
  - setTimeout goes to `message queue`
  - **Event/Message/Callback Queue** (FIFO): contains events that gets triggered after onEvents (click, hover, mousemove) and setTimeout
- **this**
  - The `this` keyword is used inside a function. The `this` keyword is merely a reference to another object. What the `this` keyword refers to depends on the way the function is implemented.
    - Here are the 3 scenarios to remember:
      1. Scenario #1: this inside a function
      The this keyword points to global object.
      2. Scenario #2: this inside a method
      The this keyword points to the object the
      method is in.
      3. Scenario #3: When function is run with
      call, bind or apply
      - When a function is called using the .call(param) .bind(param) or .apply(param) method, the frst param become the object that the this keyword refers to.
    ```
    var name = "Fatema";

    function fun(){
      // some code here
      console.log(this.name);
    }
    const user = {
      name: "Marium",
      yearOfBirth: 1999,
      calcAge: function(){
        const currentYear = (new Date()).getFullYear();
        return currentYear - this.yearOfBirth;
      }
    }

    fun(); // 'this' is global. Logs "Fatema"
    user.calcAge(); // 'this' is the user object
    fun.call(user); // 'this' is the user object. Logs "Marium"
    ```
  - Important Note:
    - In the browser, global is the window object.
    - In Node.js, global is the global object
- Write a `sum` method which will work properly when invoked using either syntax below.
  ```
  console.log(sum(2,3));   // Outputs 5
  console.log(sum(2)(3));  // Outputs 5
  ```
  2 Solutions:
  ```
  Method 1: 
  function sum(x) {
    if (arguments.length == 2) {
      return arguments[0] + arguments[1];
    } else {
      return function(y) { return x + y; };
    }
  }

  Method 2: 
  function sum(x, y) {
    if (y !== undefined) {
      return x + y;
    } else {
      return function(y) { return x + y; };
    }
  }
  ```

### <a id="js-string-methods">String Methods</a>
- substring
- substr()
- .concat()
- .charAt()
- .indexOf()
- .startsWith()
- .endsWith()
- .split()
- .slice()

### <a id="js-array-methods">Array Methods</a>
- .pop()
- .push()
- .shift()
- .unshift()
- .indexOf()
- .splice() - remove item by index position
- .slice() - can make shallow copy
.filter()
.map()
.find()
.every()
.some()
.sort()
.reduce()
.forEach()

###  <a id="js-number-methods">Number Methods</a>
- .toFixed()
- .toPrecision()
- .toString()

### Math Methods
```
Math.pow(2, 3) // 8
Math.sqrt(16) // 4
Math.min(7, 8, 6) // 6
Math.max(7, 8, 6) // 8
Math.floor(123.45) // 123
Math.ceil(123.45) // 124
Math.round(123.45) // 123
Math.random() // 0.45..
```

###  <a id="js-date-methods">Date Methods</a>
```
const d = new Date('9/17/1988');
d.getDay()
d.getFullYear()
d.getMonth()
Date.now()
Milliseconds since Jan 1, 1970
```