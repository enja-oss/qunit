# strictEqual()

## strictEqual( actual, expected, message )

**Description:** A strict type and value comparison assertion.

#### strictEqual( actual, expected, message )

**actual**  
Type: [Object](http://api.jquery.com/Types#Object)  
Object or Expression being tested

**expected**  
Type: [Object](http://api.jquery.com/Types#Object)  
Known comparison value

**message**  
Type: [String](http://api.jquery.com/Types#String)  
A short description of the assertion

The `strictEqual()` assertion provides the most rigid comparison of type and value with the strict equality operator (`===`).

[`equal()`](http://api.qunitjs.com/equal) can be used to test equality.

[`notStrictEqual()`](http://api.qunitjs.com/notStrictEqual) can be used to explicitly test strict inequality.

## Example:

#### Compare the value of two primitives, having the same value and type.

```js
test( "strictEqual test", function() {
  strictEqual( 1, 1, "1 and 1 are the same value and type" );
});
```