# [Cookbook](http://qunitjs.com/cookbook/)

## [Introduction](http://qunitjs.com/cookbook/#introduction)

Automated testing of software is an essential tool in development. Unit tests are the basic building blocks for automated tests: each component, the unit, of software is accompanied by a test that can be run by a test runner over and over again without any human interaction. In other words, you can write a test once and run it as often as necessary without any additional cost.

In addition to the benefits of good test coverage, testing can also drive the design of software, known as test-driven design, where a test is written before an implementation. You start writing a very simple test, verify that it fails (because the code to be tested doesn't exist yet), and then write the necessary implementation until the test passes. Once that happens, you extend the test to cover more of the desired functionality and implement again. By repeating those steps, the resulting code looks usually much different from what you'd get by starting with the implementation.

Unit testing in JavaScript isn't much different from in other programming languages. You need a small framework that provides a test runner, as well as some utilities to write the actual tests.

## [Automating Unit Testing](http://qunitjs.com/cookbook/#automating-unit-testing)

### Problem

You want to automate testing your applications and frameworks, maybe even benefit from test-driven design. Writing your own testing framework may be tempting, but it involves a lot of work to cover all the details and special requirements of testing JavaScript code in various browsers.

### Solution

While there are other unit testing frameworks for JavaScript, you've decided to check out QUnit. QUnit is jQuery's unit test framework and is used by a wide variety of projects.

To use QUnit, you only need to include two QUnit files on your HTML page. QUnit consists of `qunit.js`, the test runner and testing framework, and `qunit.css`, which styles the test suite page to display test results:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>QUnit basic example</title>
  <link rel="stylesheet" href="/resources/qunit.css">
</head>
<body>
  <div id="qunit"></div>
  <div id="qunit-fixture"></div>
  <script src="/resources/qunit.js"></script>
  <script>
    test( "a basic test example", function() {
      var value = "hello";
      equal( value, "hello", "We expect value to be hello" );
    });
  </script>
</body>
</html>
```

Opening this file in a browser gives the result shown below:

<iframe src=​"http://qunitjs.com/resources/example-cookbook-1-basics.html" style=​"width:​100%;​height:​300px;​border:​0px">​

The only markup necessary in the `<body>` element is a `<div> `with `id="qunit-fixture"`. This is required for all QUnit tests, even when the element itself is empty. This provides the fixture for tests, which will be explained in the section called ["Keeping Tests Atomic"](#keeping-tests-atomic).

The interesting part is the `<script>` element following the `testrunner.js` include. It consists of a call to the `test` function, with two arguments: the name of the test as a string, which is later used to display the test results, and a function. The function contains the actual testing code, which involves one or more assertions. The example uses two assertions, `ok()` and `equal()`, which are explained in detail in [the section called "Asserting Results"](#asserting-results).

Note that there is no `document-ready` block. The test runner handles that: calling `test()` just adds the test to a queue, and its execution is deferred and controlled by the test runner.

### Discussion

The header of the test suite displays the page title, a green bar when all tests passed (a red bar when at least one test failed), a bar with a few checkboxes to filter test results and a blue bar with the `navigator.userAgent` string (handy for screenshots of test results in different browsers).

Of the checkboxes, "Hide passed tests" is useful when a lot of tests ran and only a few failed. Checking the checkbox will hide everything that passed, making it easier to focus on the tests that failed (see also [the Efficient Development section below](#efficient-development)).

Checking "Check for Globals" causes QUnit to make a list of all properties on the `window` object, before and after each test, then checking for differences. If properties are added or removed, the test will fail, listing the difference. This helps to make sure your test code and code under test doesn't accidentally export any global properties.

The "No try-catch" checkbox tells QUnit to run your test outside of a try-catch block. When your test throws an exception, the testrunner will die, unable to continue running, but you'll get a "native" exception, which can help tremendously debugging old browsers with bad debugging support like Internet Explorer 6 (JavaScript sucks at rethrowing exceptions).

Below the header is a summary, showing the total time it took to run all tests as well as the overall number of total and failed assertions. While tests are still running, it will show which test is currently being executed.

The actual contents of the page are the test results. Each entry in the numbered list starts with the name of the test followed by, in parentheses, the number of failed, passed, and total assertions. Clicking the entry will show the results of each assertion, usually with details about expected and actual results. The "Rerun" link at the end will run that test on its own.

## [Asserting Results](http://qunitjs.com/cookbook/#asserting-results)

### Problem

Essential elements of any unit test are assertions. The author of the test needs to express the results expected and have the unit testing framework compare them to the actual values that an implementation produces. 

### Solution

QUnit provides three assertions. 

#### ok( truthy [, message ] )

The most basic one is `ok()`, which requires just one argument. If the argument evaluates to true, the assertion passes; otherwise, it fails. In addition, it accepts a string to display as a message in the test results: 

```js
    test( "ok test", function() {
    
      ok( true, "true succeeds" );
    
      ok( "non-empty", "non-empty string succeeds" );
    
      ok( false, "false fails" );
    
      ok( 0, "0 fails" );
    
      ok( NaN, "NaN fails" );
    
      ok( "", "empty string fails" );
    
      ok( null, "null fails" );
    
      ok( undefined, "undefined fails" );
```

#### equal( actual, expected [, message ] )

The `equal` assertion uses the simple comparison operator (`==`) to compare the actual and expected arguments. When they are equal, the assertion passes; otherwise, it fails. When it fails, both actual and expected values are displayed in the test result, in addition to a given message: 

```js
   test( "equal test", function() {
    
      equal( 0, 0, "Zero; equal succeeds" );
    
      equal( "", 0, "Empty, Zero; equal succeeds" );
    
      equal( "", "", "Empty, Empty; equal succeeds" );
    
      equal( 0, 0, "Zero, Zero; equal succeeds" );
    
      equal( "three", 3, "Three, 3; equal fails" );
    
      equal( null, false, "null, false; equal fails" );
```

Compared to `ok()`, `equal()` makes it much easier to debug tests that failed, because it's obvious which value caused the test to fail. 

When you need a strict comparison (`===`), use `strictEqual()` instead. 

#### deepEqual( actual, expected [, message ] )

The `deepEqual()` assertion can be used just like `equal()` and is a better choice in most cases. Instead of the simple comparison operator (`==`), it uses the more accurate comparison operator (`===`). That way, `undefined` doesn't equal `null`, `0`, or the empty string (`""`). It also compares the content of objects so that `{key: value}` is equal to `{key: value}`, even when comparing two objects with distinct identities.

`deepEqual()` also handles NaN, dates, regular expressions, arrays, and functions, while `equal()` would just check the object identity: 

```js
    test( "deepEqual test", function() {
    
      var obj = { foo: "bar" };
    
      deepEqual( obj, { foo: "bar" }, "Two objects can be the same in value" );
```

In case you want to explicitly not compare the content of two values, `equal()` can still be used. In general, `deepEqual()` is the better choice. 

## [Synchronous Callbacks](http://qunitjs.com/cookbook/#synchronous-callbacks)

### Problem

Occasionally, circumstances in your code may prevent callback assertions to never be called, causing the test to fail silently. 

### Solution

QUnit provides a special assertion to define the number of assertions a test contains. When the test completes without the correct number of assertions, it will fail, no matter what result the other assertions, if any, produced. 

Usage is plain and simple; just call `expect()` at the start of a test, with the number of expected assertions as the only argument: 

```js
    test( "a test", function() {
    
      function calc( x, operation ) {
    
        return operation( x );
    
      var result = calc( 2, function( x ) {
    
        ok( true, "calc() calls operation function" );
    
      equal( result, 4, "2 square equals 4" );
```

Alternatively, the expectation count can be passed as the second parameter to test(): 

```js
    test( "a test", 2, function() {
    
      function calc( x, operation ) {
    
        return operation( x );
    
      var result = calc( 2, function( x ) {
    
        ok( true, "calc() calls operation function" );
    
      equal( result, 4, "2 square equals 4" );
```

Practical Example: 

```js
    test( "a test", 1, function() {
    
      var $body = $( "body" );
    
      $body.on( "click", function() {
    
        ok( true, "body was clicked!" );
    
      $body.trigger( "click" );
```

### Discussion

`expect()` provides the most value when actually testing callbacks. When all code is running in the scope of the test function, `expect()` provides no additional value--any error preventing assertions to run would cause the test to fail anyway, because the test runner catches the error and fails the unit. 

## [Asynchronous Callbacks](http://qunitjs.com/cookbook/#asynchronous-callbacks)

### Problem

While `expect()` is useful to test synchronous callbacks (see the section called "Synchronous Callbacks"), it falls short when Asynchronous callbacks. Asynchronous callbacks conflict with the way the test runner queues and executes tests. When code under test starts a timeout or interval or an Ajax request, the test runner will just continue running the rest of the test, as well as other tests following it, instead of waiting for the result of the asynchronous operation. 

### Solution

Instead of wrapping your assertions in a `test()`, use `asyncTest()` and call `start()` when your test block is complete and ready to resume: 

```js
    asyncTest( "asynchronous test: one second later!", function() {
    
      setTimeout(function() {
    
        ok( true, "Passed and ready to resume!" );

Practical Example: 
    
    asyncTest( "asynchronous test: video ready to play", 1, function() {
    
      var $video = $( "video" );
    
      $video.on( "canplaythrough", function() {
    
        ok( true, "video has loaded and is ready to play" );
```

## [Testing User Actions](http://qunitjs.com/cookbook/#testing-user-actions)

### Problem

Code that relies on actions initiated by the user can't be tested by just calling a function. Usually an anonymous function is bound to an element's event, e.g., a click, which has to be simulated. 

### Solution

You can trigger the event using jQuery's `trigger()` method and test that the expected behavior occurred. If you don't want the native browser events to be triggered, you can use `triggerHandler()` to just execute the bound event handlers. This is useful when testing a click event on a link, where `trigger()` would cause the browser to change the location, which is hardly desired behavior in a test. 

Let's assume we have a simple key logger that we want to test: 

```js
    function KeyLogger( target ) {
    
      if ( !(this instanceof KeyLogger) ) {
    
        return new KeyLogger( target );
    
      this.target = target;
    
      this.target.off( "keydown" ).on( "keydown", function( event ) {
    
        self.log.push( event.keyCode );
```

We can manually trigger a keypress event to see whether the logger is working: 

```js
    test( "keylogger api behavior", function() {
    
          $doc = $( document ),
    
          keys = KeyLogger( $doc );
    
      event = $.Event( "keydown" );
    
      equal( keys.log.length, 1, "a key was logged" );
    
      equal( keys.log[ 0 ], 9, "correct key was logged" );
```

### Discussion

If your event handler doesn't rely on any specific properties of the event, you can just call `.trigger(eventType)`. However, if your event handler does rely on specific properties of the event, you will need to create an event object using `$.Event` and set the necessary properties, as shown previously.

It's also important to trigger all relevant events for complex behaviors such as dragging, which is comprised of mousedown, at least one mousemove, and a mouseup. Keep in mind that even some events that seem simple are actually compound; e.g., a click is really a mousedown, mouseup, and then click. Whether you actually need to trigger all three of these depends on the code under test. Triggering a click works for most cases. 

If thats not enough, you have a few framework options that help simulating user events: 

  * [syn](https://github.com/bitovi/syn) "is a synthetic event library that pretty much handles typing, clicking, moving, and dragging exactly how a real user would perform those actions". Used by FuncUnit, which is based on QUnit, for functional testing of web applications.
  * [JSRobot](http://tinymce.ephox.com/jsrobot) - "A testing utility for web-apps that can generate real keystrokes rather than simply simulating the JavaScript event firing. This allows the keystroke to trigger the built-in browser behaviour which isn't otherwise possible."
  * [DOH Robot](http://dojotoolkit.org/reference-guide/1.8/util/dohrobot.html) "provides an API that enables testers to automate their UI tests using real, cross-platform, system-level input events". This gets you the closest to "real" browser events, but uses Java applets for that.
  * [keyvent.js](https://github.com/gtramontina/keyvent.js) - "Keyboard events simulator."

## [Keeping Tests Atomic](http://qunitjs.com/cookbook/#keeping-tests-atomic)

### Problem

When tests are lumped together, it's possible to have tests that should pass but fail or tests that should fail but pass. This is a result of a test having invalid results because of side effects of a previous test: 

```js
    test( "2 asserts", function() {
    
      var $fixture = $( "#qunit-fixture" );
    
      $fixture.append( "<div>hello!</div>" );
    
      equal( $( "div", $fixture ).length, 1, "div added successfully!" );
    
      $fixture.append( "<span>hello!</span>" );
    
      equal( $( "span", $fixture ).length, 1, "span added successfully!" );
```

The first `append()` adds a `<div>` that the second `equal()` doesn't take into account. 

### Solution

Use the `test()` method to keep tests atomic, being careful to keep each assertion clean of any possible side effects. You should only rely on the fixture markup, inside the `#qunit-fixture` element. Modifying and relying on anything else can have side effects: 
    
```js
    test( "Appends a div", function() {
    
      var $fixture = $( "#qunit-fixture" );
    
      $fixture.append( "<div>hello!</div>" );
    
      equal( $( "div", $fixture ).length, 1, "div added successfully!" );
    
    test( "Appends a span", function() {
    
      var $fixture = $( "#qunit-fixture" );
    
      $fixture.append("<span>hello!</span>" );
    
      equal( $( "span", $fixture ).length, 1, "span added successfully!" );
```

QUnit will reset the elements inside the `#qunit-fixture` element after each test, removing any events that may have existed. As long as you use elements only within this fixture, you don't have to manually clean up after your tests to keep them atomic. 

### Discussion

In addition to the `#qunit-fixture` fixture element and the filters explained in the section called "Efficient Development", QUnit also offers a `?noglobals` flag. Consider the following test: 

```js
    test( "global pollution", function() {
    
      window.pollute = true;
    
      ok( pollute, "nasty pollution" );
```

In a normal test run, this passes as a valid result. Running the `ok()` test with the [noglobals flag](http://jquery-cookbook.com/examples/18/06-keeping-tests-atomic/globals.html?noglobals) will cause the test to fail, because QUnit detected that it polluted the window object. 

There is no need to use this flag all the time, but it can be handy to detect global namespace pollution that may be problematic in combination with third-party libraries. And it helps to detect bugs in tests caused by side effects. 

## [Grouping Tests](http://qunitjs.com/cookbook/#grouping-tests)

### Problem

You've split up all of your tests to keep them atomic and free of side effects, but you want to keep them logically organized and be able to run a specific group of tests on their own. 

### Solution

You can use the `module()` function to group tests together: 

```js
    test( "a basic test example", function() {
    
      ok( true, "this test is fine" );
    
    test( "a basic test example 2", function() {
    
      ok( true, "this test is fine" );
    
    test( "a basic test example 3", function() {
    
      ok( true, "this test is fine" );
    
    test( "a basic test example 4", function() {
    
      ok( true, "this test is fine" );
```

All tests that occur after a call to `module()` will be grouped into that module. The test names will all be preceded by the module name in the test results. You can then use that module name to select tests to run (see the section called "Efficient Development"). 

In addition to grouping tests, `module()` can be used to extract common code from tests within that module. The `module()` function takes an optional second parameter to define functions to run before and after each test within the module: 
    
```js
    ok( true, "one extra assert per test" );
    
      }, teardown: function() {
    
        ok( true, "and one extra assert after each test" );
    
    test( "test with setup and teardown", function() {
```

You can specify both setup and teardown properties together, or just one of them. 

Calling `module()` again without the additional argument will simply reset any setup/teardown functions defined by another module previously. 

## [Efficient Development](http://qunitjs.com/cookbook/#efficient-development)

### Problem

Once your testsuite takes longer then a few seconds to run, you want to avoid wasting a lot of time just waiting for test results to come in. 

### Solution

QUnit has a bunch of features built-in to make up for that. The most interesting ones require just a single click to activate. Toggle the "Hide passed tests" checkbox at the top, and QUnit will only show you tests that failed. That alone doesn't make a difference in speed, but already helps focusing on failing tests. 

It gets more interesting if you take another QUnit feature into account, which is enabled by default and usually not noticable. Whenever a test fails, QUnit stores the name of that test in `sessionStorage`. The next time you run a testsuite, that failing test will run before all other tests. The output order isn't affected, only the execution order. In combination with the "Hide passed tests" checkbox you will then get to see the failing test, if it still fails, at the top, as soon as possible. 

### Discussion

The automatic reordering happens by default. It implies that your tests need to be atomic, [as discussed previously](). If your tests aren't, you'll see random non-deterministic errors. Fixing that is usually the right approach. If you're really desperate, you can set `[QUnit.config](//api.qunitjs.com/QUnit.config/).reorder = false`. 

In addition to the automatic reordering, there are a few manual options available. You can rerun any test by clicking the "Rerun" link next to that test. That will add a "testNumber=N" parameter to the query string, where "N" is the number of the test you clicked. You can then reload the page to keep running just that test, or use the browser's back button to go back to running all tests. 

Running all tests within a module works pretty much the same way, except that you choose the module to run using the select at the top right. It'll set a "module=N" query string, where "N" is the encoded name of the module, for example "?module=testEnvironment%20with%20object". 

## [Further Tutorials](http://qunitjs.com/cookbook/#more-tutorials)

If you want to read more on unit testing JavaScript (not specific to QUnit), check out the book [Test-Driven JavaScript Development](http://tddjs.com/). 

_*This section was first published, under a non-exclusive license, as the last chapter in the jQuery Cookbook, authored by Scott González and Jörn Zaefferer. As QUnit changed since the book was printed, this version is more up-to-date._
