# notStrictEqual()

## notStrictEqual( actual, expected, message )

**Description:** A non-strict comparison assertion, checking for inequality.

#### notStrictEqual( actual, expected, message )

**actual**  
Type: [Object](http://api.jquery.com/Types#Object)  
Object or Expression being tested

**expected**  
Type: [Object](http://api.jquery.com/Types#Object)  
Known comparison value

**message**  
Type: [String](http://api.jquery.com/Types#String)  
A short description of the assertion

The `notStrictEqual` assertion uses the strict inverted comparison operator (`!==`) to compare the actual and expected arguments. When they aren't equal, the assertion passes; otherwise, it fails. When it fails, both actual and expected values are displayed in the test result, in addition to a given message.

[`equal()`](http://api.qunitjs.com/equal) can be used to test equality.

[`strictEqual()`](http://api.qunitjs.com/strictEqual) can be used to test strict equality.

## Example:

#### The simplest assertion example:

```js
test( "a test", function() {
  notStrictEqual( 1, "1", "String '1' and number 1 don't have the same value" );
});
```