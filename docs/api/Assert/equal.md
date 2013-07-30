# equal()

## equal( actual, expected, message )

**Description:** A non-strict comparison assertion, roughly equivalent to JUnit `assertEquals`.

#### equal( actual, expected, message )

**actual**  
Type: [Object](http://api.jquery.com/Types#Object)  
Object or Expression being tested

**expected**  
Type: [Object](http://api.jquery.com/Types#Object)  
Known comparison value

**message**  
Type: [String](http://api.jquery.com/Types#String)  
A short description of the assertion

The `equal` assertion uses the simple comparison operator (`==`) to compare the actual and expected arguments. When they are equal, the assertion passes; otherwise, it fails. When it fails, both actual and expected values are displayed in the test result, in addition to a given message.

[`notEqual()`](http://api.qunitjs.com/notEqual/) can be used to explicitly test inequality.  

[`notStrictEqual()`](http://api.qunitjs.com/notStrictEqual/) can be used to test strict inequality.

## Example:

#### Example: The simplest assertion example:

```js
test( "a test", function() {
  equal( 1, "1", "String '1' and number 1 have the same value" );
});
```

#### Example: A slightly more thorough set of assertions:

```js
test( "equal test", function() {
  equal( 0, 0, "Zero; equal succeeds" );
  equal( "", 0, "Empty, Zero; equal succeeds" );
  equal( "", "", "Empty, Empty; equal succeeds" );
  equal( 0, 0, "Zero, Zero; equal succeeds" );
 
  equal( "three", 3, "Three, 3; equal fails" );
  equal( null, false, "null, false; equal fails" );
});
```