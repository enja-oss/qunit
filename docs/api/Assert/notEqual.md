# notEqual()

## notEqual( actual, expected, message )

**Description:**  A non-strict comparison assertion, checking for inequality.

#### notEqual( actual, expected, message )

**actual**  
Type: [Object](http://api.jquery.com/Types#Object)  
Object or Expression being tested

**expected**  
Type: [Object](http://api.jquery.com/Types#Object)  
Known comparison value

**message**  
Type: [String](http://api.jquery.com/Types#String)  
A short description of the assertion

The `notEqual` assertion uses the simple inverted comparison operator (`!=`) to compare the actual and expected arguments. When they aren't equal, the assertion passes; otherwise, it fails. When it fails, both actual and expected values are displayed in the test result, in addition to a given message.

[`equal()`](http://api.qunitjs.com/equal) can be used to test equality.

[`notStrictEqual()`](http://api.qunitjs.com/notStrictEqual) can be used to test strict inequality.

## Example:

#### The simplest assertion example:

```js
test( "a test", function() {
  notEqual( 1, "2", "String '2' and number 1 don't have the same value" );
});
```