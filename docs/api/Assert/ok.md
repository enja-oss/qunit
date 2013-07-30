# ok()

## ok( state, message )

**Description:** A boolean assertion, equivalent to CommonJS's assert.ok() and JUnit's assertTrue(). Passes if the first argument is truthy.

#### ok( state, message )

**state**  
Type: [Expression](http://api.jquery.com/Types/#Expression)  
Expression being tested

**message**  
Type: [String](http://api.jquery.com/Types#String)  
A short description of the assertion

The most basic assertion in QUnit, ok() requires just one argument. If the argument evaluates to true, the assertion passes; otherwise, it fails. If a second message argument is provided, it will be displayed in place of the result.

## Example:

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
});
```