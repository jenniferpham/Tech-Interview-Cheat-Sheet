# FrontEnd Interview Cheat Sheet

# Table of Content
- [Web](#web)
  - [Storage](#storage)
  - [API](#api)
    - [Status Codes](#status-codes)
    - [Rest API](#rest-api)
    - [GraphQL](#graphql)
  - [Performance](#performance)
  - [Security](#security)
  - [Authentication](#auth)
  - [Testing](#testing)
  - [Accessibility](#accessibility)
- [Language/Frameworks](#language-frameworks)
  - [HTML](#html)
  - [CSS](#css)
  - [Javascript](#javascript)
  - [React](#react)
- [References](#references)



# <a id="web">Web</a>
- When user types in URL into browser:
    1. Enter url into web browser
    2. DNS (domain name services) translates human-friendly domain name to computer friendly IP address (IP4 vs IP6)
    3. browser sends HTTP request to server
    4. server sends back HTTP response
    5. browser renders HTML
    6. brwoser sends request for additional objects embeded in HTML (images, CSS, JS)
    7. Once page is loaded, browser sends further async requests as needed
- **JSON** is a format that encodes objects in a string
    - **Serialization**: object -> string
    - **Deserialization**:   string -> object
    ```
    {foo: [1, 4, 7, 10], bar: "baz"}
    //serializing into JSON will convert it into a string, which can be stored or sent anywhere
    '{"foo":[1,4,7,10],"bar":"baz"}'
    // receiver can then deserialize this string to get back the original object. 
    ```
- send data from a web page to a server without a page refresh
    - Make an API request
   - `img` tag technique, and can work across domains.
    - Data can be sent via **query parameters** on a small 1x1 pixel blank image, i.e. `http://www.ad-company.com/blank.jpg?data=value`.
    - Data is limited to: URL length limit depending on the browser. Web standard is 2048 characters.
    - `Script` and `Link` tags are used to make a URL request to a server
    - `Iframe` form can POST to an invisible iframe of a different domain, effectively sending data cross sites
    - CORS headers
    - POST requests on search
Follow up 1: How do you get a return response for updating the page?

Follow up 2: What if the server was a different domain?



## <a id="storage">Storage</a>
- **Cookies**: small files on user's computer read by the server-side (Remember: serve cookies)
  - storage capacity 4kb
  - accessed by either web server or client's computer
  - basically key and value store (hash table)
  - must be parsed to read
  - **secure cookie** (set `secure` flag on cookie)
    - can only be transmitted over an encrypted connection (i.e. HTTPS). They cannot be transmitted over unencrypted connections (i.e. HTTP). 
    - makes the cookie less likely to be exposed to cookie theft via eavesdropping
  - **HttpOnly cookie** flag (set `HttpOnly` flag on cookie)
    - HttpOnly flag makes cookies inaccessible to client-side scripts, like JavaScript. Those cookies can only be edited by a server that processes the request.
    - often added to cookies that may contain sensitive information about the user
    - eliminates the threat of cookie theft via cross-site scripting (XSS)
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
- **SessionStorage**


## <a id="api">API</a>
### <a id="status-codes">Status Codes</a>
- POST
    - **200 OK** - It’s the basic status code to tell the client everything went good. Since we don’t create endpoint accessible resource when creating an access token, we can use 200 as a status for that action.
    - **201 Created** - The most fitting for Create operations. This code should signal backend-side resource creation and come along with a Location header that defines the most specific URL for that newly created resource. It’s also a good idea to include appropriate representation of the resource or at least one or more URLs to that resource in the response body.
    - **202 Accepted** - Often used for asynchronous processing. This code tells the client that the request was valid, but its processing will finish sometime in the future. The response body should include an URL to the finished resource with some information about when it will be available, or an URL to some monitoring endpoint that tells the client when the resource is available.
- PUT/PATCH
    - 200 OK
    - 202 Accepted - asynchronous
    - 204 No Content - A proper code for updates that don’t return data to the client, for example when just saving a currently edited document.
- DELETE
    - 200 OK - Some people think a delete function of any kind should return the deleted element, so a representation of the deleted element can be included in the response body.
    - **204 No Content** - The most fitting status code for this case. It’s better to reduce traffic and simply tell the client the deletion is complete and return no response body (as the resource has been deleted).
    - 202 Accepted - If the deletion is asynchronous and takes some time, which is the case in distributed systems, it can be appropriate to return this code with some information or URL to tell the client when it will be deleted.
- GET
    - **200 OK** - Most of the read actions will be answered with a 200 OK status.
    - 206 Partial Content - This code is useful for lists of content that are too big to be delivered in one response. It has to be used with a Range header field in the request. Usually, this header field defines the byte-range the backend should send to the client, but the unit can be freely assigned as long as both sides understand it.
    - 308 Permanent Redirect - This tells the client to use another URL to access the resource and not use the current URL anymore. It’s helpful when we have multiple endpoints for one resource, but don’t want to implement reading from all of them.
    - 304 Not Modified - Is used like 200 but without a body, so the client will be redirected to its locally cached representation from a previous read.


### <a id="rest-api">REST API</a>
- **RESTful services**
  - implement CRUD using HTTP methods (GET, PUT, POST, DELETE)
  - transfer XML or JSON or both
  - stateless
### <a id="graphql">GraphQL</a>
```
// A query to fetch all the pets from the app might look like this:
query GetAllPets {
  pets {
    name
    petType
  }
}

// a mutation that adds a new pet might look a little something like this:
mutation AddNewPet ($name: String!, $petType: PetType) {
  addPet(name: $name, petType: $petType) {
    id
    name
    petType
  }
}
```
- **Apollo Client cache**: Apollo Client stores the results of your GraphQL queries in a local, normalized, **in-memory cache**. This enables Apollo Client to respond almost immediately to queries for already-cached data, without even sending a network request.
- Subscribe to data
Ex: Let's say we want our GraphQL server to push an update to our client as soon as a new comment is added to the post. First we need to define the subscription that Apollo Client will execute when the COMMENTS_QUERY returns:
```
const COMMENTS_SUBSCRIPTION = gql`
  subscription OnCommentAdded($postID: ID!) {
    commentAdded(postID: $postID) {
      id
      content
    }
  }
`;

--------

function CommentsPageWithData({ params }) {
  const { subscribeToMore, ...result } = useQuery(
    COMMENTS_QUERY,
    { variables: { postID: params.postID } }
  );

  return (
    <CommentsPage
      {...result}
      subscribeToNewComments={() =>
        subscribeToMore({
          document: COMMENTS_SUBSCRIPTION,  // subscription to execute
          variables: { postID: params.postID },  // variables to include when executing the subscription

          // function that tells Apollo Client how to combine the query's currently cached result (prev) with the subscriptionData that's pushed by our GraphQL server. The return value of this function completely replaces the current cached result for the query.
          updateQuery: (prev, { subscriptionData }) => {   
            if (!subscriptionData.data) return prev;
            const newFeedItem = subscriptionData.data.commentAdded;

            return Object.assign({}, prev, {
              post: {
                comments: [newFeedItem, ...prev.post.comments]
              }
            });
          }
        })
      }
    />
  );
}

---
export class CommentsPage extends Component {
  componentDidMount() {
    this.props.subscribeToNewComments();
  }
}
```

## <a id="performance">Performance</a>
To reduce app load time
  - bundle and minify all CSS and JS files
  - **Content Delivery Network(CDN)** - redirects traffic to closest server, which caches content (web videos or bulky media) -> decrease latency of HTTP request if servers are geographically closer to the user and inc download speed
  - **Cache** - reduce DB calls
  - **optimize images** to less than screen resolution and save as compressed file
  - 1 **sprite image** for all icon images of app and then use background-position to display it -> reduce HTTP requests
  - test perforamnce with **Lighthouse**
  - pre-load/pre-fetch data
  - **code-splitting**
  - **service worker** offline
  - lazy load
  - autopager
  - **infinite scroll**
  - SSR/initial data feed
  - **List virtualization**: limit adding DOM elements to only what's viewable
    - So many elements in the DOM can cause two problems: slow initial rendering and laggy scrolling
    - render to the DOM only what is visible to the user and unload them when they are not visible by replacing them with new ones
    - when scrolling, the new list items render when they are in view and replace the old items that exit the viewport
    - Libraries: `react-window` and `react-virtualized`
  - **Pagination**
      - split our large dataset into chunks ( or pages ) that we can gradually fetch and display to the user, thus reducing the load on the database
      - works best on websites where users are looking for specific pieces of content, not just browsing content. 
      - **Infinite scroll** is better suited for the exploration of content, where users are browsing aimlessly for something interesting, not good for goal-oriented tasks
      - Pro:
        - can save your spot and go back to it
      - Con:
        - extra action to click
      - Types
        1. **Offset Pagination**
          - `limit` - Number of rows to fetch from the database
          - `offset` - Number of rows to skip. Offset is like a page number, but with a bit of math around it (offset = (page-1) * limit)
          -offset in databases is implemented in a way that loops through rows to know how many should be skipped. That means that the higher our offset is, the longer our database query will take.
        2. **Cursor Pagination**
          - `limit` - Same as before, amount of rows we want to show on one page
          - `cursor` - ID of a reference element in the list. This can be the first item if you're querying the previous page and the last item if querying the next page
          - `cursorDirection` - If user clicked Next or Previous (after or before)
          - solve all issues that offset pagination has — performance, missing data and data duplication
          - Queries that use a `cursor` instead of `offset` are more performant because the WHERE query helps skip unwanted rows, while OFFSET needs to iterate over them, resulting in a full-table scan. Skipping rows using WHERE can get even faster if you set up proper indexes on your IDs. The index is created by default in case of your primary key.
    - **Infinite scrolling**: web-design technique that loads content continuously as the user scrolls down the page, eliminating the need for pagination
      - good for browsing lists and feeds or discovery, without extra clicks/taps
      - not good if you have a call to action at bottom. Endless scrolling is not recommended for goal-oriented finding tasks.
      - API implemented similar to pagination
      - **Pagination** works best on websites where users are looking for specific pieces of content. **Infinite scroll** is better suited for the exploration of content, where users are browsing aimlessly for something interesting. 
      - Cons
        - can’t bookmark their location and come back to it later like pagination
        - lack of footer
        - irrelevant scrollbar
- Images
  - compress images
  - lazy load images
  - progressive images - blurry to clear
  - use SVG for icons
  - CSS sprites -> reduce # of network requests
  - caching through CDN


## <a id="security">Security</a>
- **Cross-Origin Resource Sharing (CORS)** enables resources like JS, fonts, images on a webpage to be requested from an outside domain (recommended standard)
    - uses **CORS headers** to give browser permission to let web app at 1 domain access resources from different domain
    - if server doesn't respond with specific cross-origin headers, browser will not allow JS to access the response
    ```
    Access-Control-Allow-Origin: *  // wildcard - any
    Access-Control-Allow-Origin: <origin>  // 1 specific origin
    ```
    - Limiting the possible Access-Control-Allow-Origin values to a set of allowed origins requires code on the server side to check the value of the Origin request header, compare that to a list of allowed origins, and then if the Origin value is in the list, set the Access-Control-Allow-Origin value to the same value as the Origin value.
  - `Same-Origin Policy` blocks web apps from making HTTP requests from different origins -> prevents attackers that plant scripts on various websites to make requests to sites with personal data (Ex: ads in Google ads)
- **Cross Site Scripting (XSS)** - security vulnerability where hackers try to inject HTML/Javascript ot steal confidential info from cookies and pass that info to themselves
  - solution: sanitize inputs, escape, don't use eval, update libraries
- **Cross site request forgery (CSRF)**
    - an attack that sends malicious requests from an authenticated user to an application. A successful CSRF attack can be devastating for both the business and user.
    - A CSRF token is a unique, secret, unpredictable value that is generated by the server-side application and transmitted to the client in such a way that it is included in a subsequent HTTP request made by the client.
- **URL manipulation** - security vulnerability where hackers pass info in query string parameters and alter info to get authentication of servers and steal critical data
- **JSONP** requests data by loading a `<script>` element andenables sharing of data bypassing **same-origin policy**
- **Cache Control** are HTTP headers that control caching in browsers and shared caches
    - `max-age` - use this cached response until # of ms later
    - `no store` - means don't cache
    - `no cache` -  always check for content updates while reusing stored content

## <a id="auth">Authentication</a>
- **Single Sign On**
    1. A user browses to the application or website they want access to, aka, the Service Provider (Ex: Glidewell).
    2. The Service Provider sends a token that contains some information about the user, like their email address, to the SSO system, aka, the Identity Provider, as part of a request to authenticate the user.
    3. The Identity Provider (Ex: Okta) first checks to see whether the user has already been authenticated, in which case it will grant the user access to the Service Provider application and skip to step 5.
    4. If the user hasn’t logged in, they will be prompted to do so by providing the credentials required by the Identity Provider. This could simply be a username and password or it might include some other form of authentication like a One-Time Password (OTP).
    5. Once the Identity Provider validates the credentials provided, it will send a token back to the Service Provider confirming a successful authentication.
    6. This token is passed through the user’s browser to the Service Provider by **storing browser cookies**.
    - A cookie with the **Secure** attribute is only sent to the server with an encrypted request over the HTTPS protocol, can't be sent over HTTP and makes it harder for middle-person to manipulate it
    7. The token that is received by the Service Provider is validated according to the trust relationship that was set up between the Service Provider and the Identity Provider during the initial configuration.
    - Json Web Token (JWT) has a digital signature to ensure that it wasn't changed in transit
    8. The user is granted access to the Service Provider.


## <a id="testing">Testing</a>
- **Unit Testing**: validate taht each individual unit of software does what it is intended to do
- **E2E Testing**: test flow of app from start-to-finish, usually from user perspective
- test app in different browsers using tools like BrowserStack
- **Test Driven Development (TDD)**

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
- **CSS Grid vs Flexbox**
    - flexbox is one-dimensional, while CSS Grid is two-dimensional. 
    - Flexbox lays out items along either the horizontal or the vertical axis, so you have to decide whether you want a row-based or a column-based layout.
    - Grid allows you to create two-dimensional layouts where you can precisely place grid items into cells defined by rows and columns.
- CSS Specificity
  1. !important - 10000
  2. Inline Style - 1000
  3. ID - 100
  4. Class, Pseudoclass - 10
  5. Element, Pseudo-element - 1
- **CSS Box Model** - content (text/images), padding, border, margin
    - **innerWidth**: includes content + padding (no border and no margin)
    - **outerWidth**: includes content + padding + **border** (no margin)
- hide element
    - `display: none` - remove from flow of the document
    - `visibility: hidden` - still takes up space in the document but not visible



- CSS Grid vs Flexbox 


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
- **Event Delegation**: Instead of adding an event listener to each and every similar element, we can add an event listener to a parent element and call an event on a particular target using the `.target` property of the event object. -> more efficient
    - use `event.target` to access the tag that was clicked
    - capturing and bubbling
        1. `<ul>` element is clicked.
        2. The event goes in the **capturing** phase.
        3. It reaches the target (`<li>` in our case).
        4. It switches to the **bubbling** phase.
        5. When it hits the `<ul>` element, it runs the event listener.
        6. Inside the listener function event.target is the element that was clicked.
        7. Event.target provides us access to the `<li>` element that was clicked.
- **Event bubbling and capturing** are two ways of event propagation in the HTML DOM API, when an event occurs in an element inside another element, and both elements have registered a handle for that event. The event propagation mode determines in which order the elements receive the event.
    - With **bubbling** (bubble up), the event is first captured and handled by the innermost element and then propagated to outer elements.
    - With **capturing** (trickle down), the event is first captured by the outermost element and propagated to the inner elements.
- Difference between: `function Person(){}`, `var person = Person()`, and `var person = new Person()`?
    - `var person = Person()` invokes the Person as a function, and not as a constructor.Typically, the constructor does not return anything, hence invoking the constructor like a normal function will return undefined 
    - `var person = new Person()` creates an instance of the Person object using the new operator, which inherits from Person.prototype.
```
function Person(name) {
  this.name = name
}

```
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
```
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('foo');
  }, 300);
});

myPromise
  .then(handleResolvedA, handleRejectedA)
  .then(handleResolvedB, handleRejectedB)
  .then(handleResolvedC, handleRejectedC);
```
    - Handling a rejected promise in each .then() has consequences further down the promise chain. Sometimes there is no choice, because an error must be handled immediately. In such cases we must throw an error of some type to maintain error state down the chain. On the other hand, in the absence of an immediate need, it is simpler to leave out error handling until a final .catch() statement. A .catch() is really just a .then() without a slot for a callback function for the case when the promise is resolved.
```
myPromise
  .then(handleResolvedA)
  .then(handleResolvedB)
  .then(handleResolvedC)
  .catch(handleRejectedAny);
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
  - `input: 10 + 20 + "30" -> output: "3030"`
- `input: "10" + 20 + 30 -> output: "102030"`
- Ways to debug
    - console.log()
    - debugger keyword
    - Chrome dev tools
- JS cons
    - no support for multithreading and multiprocessing
    - reading and writing of files not allowed
    - no support for networking apps
- **BOM- Browser Object Model**
    - default object in browser is the **window**
    - window object provides properties like
        - document
        - history (`history.back()`, `history.forward()`, `history.go(4)`)
        - screen
        - navigator
        - location
        - innerHeight
        - innerWidth
- `encodeURIComponent` should be used to encode a URI Component - a string that is supposed to be part of a URL. 
    - encodes special characters including: , / ? : @ & = + $ #
    - will not encode: ~!*()'
- `encodeURI` should be used to encode a URI or an existing URL. 
    - will not encode: ~!@#$&*()=:/,;?+'
    - URLs can only have certain characters from the standard 128 character ASCII set. Reserved characters that do not belong to this set must be encoded.
```
let uri = "https://w3schools.com/my test.asp?name=ståle&car=saab";
let encoded = encodeURIComponent(uri); // https%3A%2F%2Fw3schools.com%2Fmy%20test.asp%3Fname%3Dst%C3%A5le%26car%3Dsaab
```
- **Prototypical Inheritance**
    - JS does not provide a class implementation per se (the class keyword is introduced in ES2015, but is syntactical sugar, JavaScript remains prototype-based).
    - When it comes to inheritance, JavaScript only has one construct: objects. Each object has a private property which holds a link to another object called its prototype. That prototype object has a prototype of its own, and so on until an object is reached with null as its prototype. By definition, null has no prototype, and acts as the final link in this prototype chain.
    - Nearly all objects in JavaScript are instances of Object, which has null as its prototype.

### <a id="js-string-methods">String Methods</a>
- substring(startIndex, endIndexNotIncluding)  // can also take negative startIndex from end of string
- substr(from, length)
- .concat()
- .charAt()
- .indexOf()
- .startsWith()
- .endsWith()
- .repeat()
- .trim() - shave
- .split()
- .slice(startIndex, endIndex) // The returns copied string excludes the end index
    ```
    const str = 'Everything is not what it seems'
    str.slice(3) // copy everything after and including the 3rd index, 'r'
    // returns 'rything is not what it seems'

    str.slice(-5) // copy elements from the last index to the left 5 indices
    // returns 'seems'

    str.slice(-17, -6) // copy specified elements based on the indices counting from the end of the string
    // returns 'not what it'

    console.log(str) // str is not mutated
    // 'Everything is not what it seems'
    ```

### <a id="js-array-methods">Array Methods</a>
- .pop()
- .push()
- .shift()
- .unshift()
- .indexOf()
- .slice(startIndex, endIndex) // returns copied array excludes the end index
    ```
    const arr = ['alex', 'justin', 'max', 'harper', 'mason']

    arr.slice(2) // copy everything after and including the 2nd index, 'max'
    // returns ['max', 'harper', 'mason']

    arr.slice(2, 4) // copy everything after the second index up to the 3rd index
    // returns ['max', 'harper']

    arr.slice(2, 5) // copy everything after the second index up to the 4th index
    // returns [ 'max', 'harper', 'mason' ]
    ```
- .splice(startIndex, deleteCount [optional], newElem [optional], newElem [optional], ...) - mutates array and removes item by index position and can replace items
    -  Remember: splice has an extra letter, 'p', compared to slice. Because of the extra letter, I associate the additional letter to splice's use of adding or removing from the original array. And because splice can add and remove stuff to the original array, that means that it also mutates the original array.
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

## <a id="react">React</a>
- fetch data
```
// useRandomQuote.js
import { useEffect, useState } from "react";

const useRandomQuote = (i) => {
  const [quote, setQuote] = useState(""); // useState structure

  useEffect(() => {
    const fetchKanyeQuote = async () => {
      try {
        const res = await fetch("https://api.kanye.rest/");
        const data = await res.json();

        setQuote(data.quote); // setter function to update initial state val
      } catch (error) {
        console.error(error);
      }
    };

    fetchKanyeQuote();
  }, [i]);

  return quote; // remember to add this line
};

export default useRandomQuote;
```
- fetch data with loading and error state
```
import React, { useState, useEffect } from 'react';

function App() {
  const [data, setData] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [hasError, setHasError] = useState(false);

  useEffect(() => {
    const fetchData = async () => {
      setIsLoading(true);
      setHasError(false);
      try {
        const res = await fetch(
          'https://hn.algolia.com/api/v1/search?query=react',
        );
        const json = await res.json();
        setData(json.hits);
      } catch (error) {
        setHasError(true);
      }
      setIsLoading(false);
    };
    fetchData();
  }, [setData]);

  return (
    <React.Fragment>
      {hasError && <p>Something went wrong.</p>}
      {isLoading ? (
        <p>Loading ...</p>
      ) : (
        <ul>
          {data.map(item => (
            <li key={item.ObjectId}>
              <a href={item.url}>{item.title}</a>
            </li>
          ))}
        </ul>
      )}
    </React.Fragment>
  );
}

export default App;
```
- functional component that uses data from custom hook
```
import { StatusBar } from "expo-status-bar";
import React, { useState } from "react";
import {
  StyleSheet,
  Text,
  View,
  ImageBackground,
  TouchableOpacity,
} from "react-native";
import useRandomQuote from "./useRandomQuote";

export default function App() {
  const backgroundImage = { uri: "https://wallpapercave.com/wp/wp9837865.png" };

  const [i, setI] = useState(0);
  const kanyeQuote = useRandomQuote(i);

  return (
    <View style={styles.container}>
      <ImageBackground
        source={backgroundImage}
        style={styles.backgroundImage}
        resizeMode="cover"
      >
        <Text style={styles.titleText}>Kanye West Quotes</Text>
        <View
          style={{
            backgroundColor: "rgba(52, 52, 52, 0.8)",
            width: "100%",
          }}
        >
          <Text style={styles.quoteText}>"{kanyeQuote}"</Text>
        </View>
        <TouchableOpacity style={styles.button} onPress={() => setI(i + 1)}>
          <Text style={styles.buttonText}>More Kanye</Text>
        </TouchableOpacity>
        <StatusBar style="auto" />
      </ImageBackground>
    </View>
  );
}
```
- composing components
```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}
```
- async/await fetch data
```
async function fetchFunction() {
  try{
	const response = await fetch(`http://url.com`);
	const json = await response.json();
  }
  catch(err) {
    throw err;
    console.log(err);
  }
}
```
- improve user experience or performance (React)
    - **Optimistic UI** is a pattern that you can use to simulate the results of a mutation and update the UI even before receiving a response from the server. 
        - instead of waiting for response from server, we can display that it was updated on UI, and save message in **queue** like SQS. When it is ready to be run, it can trigger a function or lambda to update the item in the database.
    - **lazy loading images** we can wait until each of the images is about to appear in the viewport before we render them in the DOM
        - Similar to the concept of windowing mentioned above, lazy loading images prevents the creation of unnecessary DOM nodes, boosting the performance of our React application.
    - **useCallback** hook to remember function
    - **useMemo** hook to remember calculation based on same input and output
    - **code-splitting** 
        - By default, when a React application renders in a browser, a “bundle” file containing the entire application code loads and serves to users at once. This file generates by merging all the code files needed to make a web application work.
        - bundling is useful because it reduces the number of HTTP requests a page can handle. However, as an application grows, the file sizes increase, thus increasing the bundle file. At a certain point, this continuous file increase slows the initial page load, reducing the user’s satisfaction.
        - React allows us to split a large bundle file into multiple chunks using dynamic import() followed by lazy loading these chunks on-demand using the React.lazy
        ```
        // before
        import Home from "./components/Home";
        import About from "./components/About";
        
        // after
        const Home = React.lazy(() => import("./components/Home"));
        const About = React.lazy(() => import("./components/About"));
        ```
- Detect if user is on mobile device on React
    - check the `User-Agent` request header of the navigator -> not reliable and can be spoofed or change in future versions of browsers
    - `Navigator.maxTouchPoints`:  read-only property of the Navigator interface returns the maximum number of simultaneous touch contact points are supported by the current device -> used to detect if device has touch screen or not
```
import { useState, useEffect } from "react";

const DeviceDetector = () => {
  const [deviceType, setDeviceType] = useState("");

  useEffect(() => {
    let hasTouchScreen = false;
    if ("maxTouchPoints" in navigator) {
      hasTouchScreen = navigator.maxTouchPoints > 0;
    } else if ("msMaxTouchPoints" in navigator) {
      hasTouchScreen = navigator.msMaxTouchPoints > 0;
    } else {
      const mQ = window.matchMedia && matchMedia("(pointer:coarse)");
      if (mQ && mQ.media === "(pointer:coarse)") {
        hasTouchScreen = !!mQ.matches;
      } else if ("orientation" in window) {
        hasTouchScreen = true; // deprecated, but good fallback
      } else {
        // Only as a last resort, fall back to user agent sniffing
        var UA = navigator.userAgent;
        hasTouchScreen =
          /\b(BlackBerry|webOS|iPhone|IEMobile)\b/i.test(UA) ||
          /\b(Android|Windows Phone|iPad|iPod)\b/i.test(UA);
      }
    }
    if (hasTouchScreen) {
      setDeviceType("Mobile");
    } else {
      setDeviceType("Desktop");
    }
  }, []);

  return <div>I am rendered on: {deviceType}</div>;
};

export default DeviceDetector;
```
    - detect width device
    - use NPM library `react-device-detect`
```
// Installation:
npm install react-device-detect --save

// The code:
import React, { useState } from "react";
import "./App.css";
import { isMobile, browserName } from "react-device-detect";

function App() {
  return (
    <div style={styles.container}>
      {isMobile ? <h2>Mobile</h2> : <h2>Desktop</h2>}
    </div>
  );
}

// just a little styling
const styles = {
  container: {
    display: 'flex',
    justifyContent: 'center',
    paddingTop: '15px'
  }
}

export default App;
```
- **lazy loading**: a component or a part of code must get loaded when it is required aka **code splitting and data fetching**.
    - React bundles the complete code and deploys all of it at the same time.
    - a huge application and will cost a lot of unnecessary data transfer which can lead to slow loading of the website.
    - Use when certain code/features will not be accessible to all the users or the user does not access it frequently -> best to load them when the user requests for it -> improves user experience and initial loading time.
    - `React suspense` does lazy loading
```
import React, { Suspense } from "react";
const Customer = React.lazy(() => import("./Customer.js"));
const Admin = React.lazy(() => import("./Admin.js"));

//Instead of regular import statements, we will use the above approach for lazy loading

export default (props) => {
	if (props.user === "admin") {
		return (
			// fallback component is rendered until our main component is loaded
			<Suspense fallback={<div>Loading</div>}>
				<Admin />
			</Suspense>
		);
	} else if (props.user === "customer") {
		return (
			<Suspense fallback={<div>Loading</div>}>
				<Customer />
			</Suspense>
		);
	} else {
		return <div> Invalid User </div>;
	}
};
```
```
import React, { lazy, Suspense } from ‘react’;
import ReactDOM from ‘react-dom’;
import ‘./index.css’;

const Artists = lazy(() => import(‘./Artists’))
const Performers = lazy(() => import(‘./Performers’))

// Suspense wraps multiple imports and shows fallback
class App extends React.Component {
 render(){
  return(
   <div className=”App”>
    <Suspense fallback={<h1>Still Loading…</h1>}>
     <Artists />
     <Performers />
    </Suspense>
   </div>
  );
 }
}
ReactDOM.render(<App />, document.getElementById(‘root’));
```

# <a id="references">References</a>
CSS:
https://www.edureka.co/blog/interview-questions/css-interview-questions/ 
CSS specificity: https://css-tricks.com/specifics-on-css-specificity/ 
Flexbox https://css-tricks.com/snippets/css/a-guide-to-flexbox/ 
HTML: 
https://www.javatpoint.com/html-interview-questions 
https://www.edureka.co/blog/interview-questions/top-50-html-interview-questions-and-answers/ 
https://career.guru99.com/top-50-html-interview-questions/ 
JS: 
https://www.toptal.com/javascript/interview-questions
https://www.guru99.com/javascript-interview-questions-answers.html 
Slice vs splice https://sophiali.dev/javascript-slice-splice 
JS Event Loop - https://felixgerschau.com/javascript-event-loop-call-stack/ 
Sync vs async https://www.freecodecamp.org/news/synchronous-vs-asynchronous-in-javascript/ 
JS memory: https://felixgerschau.com/javascript-memory-management/#the-memory-heap-and-stack 
Map and set: https://www.taniarascia.com/understanding-map-and-set-javascript/ 
Race conditions: https://www.lorenzweiss.de/race_conditions_explained/    https://www.nodejsdesignpatterns.com/blog/node-js-race-conditions/ 
Cheat sheet https://cheatography.com/davechild/cheat-sheets/javascript/ 
https://github.com/mbeaudru/modern-js-cheatsheet 
React:
https://www.sitepoint.com/react-interview-questions-solutions/ 
https://medium.com/@vigowebs/frequently-asked-react-js-interview-questions-and-answers-36f3dd99f486 
https://www.toptal.com/react/interview-questions 
Optimizing performance in React: https://blog.logrocket.com/optimizing-performance-react-application/ 
Redux
Example of component with useEffect hook, useState, fetch data, custom hook: https://echobind.com/post/custom-react-hook-guide-build-a-kanye-quote-app-with-react-native 
Fetch data: https://www.freecodecamp.org/news/fetch-data-react/ 
React Native
https://www.bestinterviewquestion.com/react-native-interview-questions 
https://www.educative.io/blog/top-react-native-interview-questions 
Lazy loading in React: https://www.loginradius.com/blog/engineering/lazy-loading-in-react/
