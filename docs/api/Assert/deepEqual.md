# deepEqual()

## deepEqual( actual, expected, message )

**Description:** A deep recursive comparison assertion, working on primitive types, arrays, objects, regular expressions, dates and functions.

#### deepEqual( actual, expected, message )

**actual**  
Type: [Object](http://api.jquery.com/Types#Object)  
Object or Expression being tested

**expected**  
Type: [Object](http://api.jquery.com/Types#Object)  
Known comparison value

**message**  
Type: [String](http://api.jquery.com/Types#String)  
A short description of the assertion

The `deepEqual()` assertion can be used just like `equal()` when comparing the value of objects, such that `{ key: value }` is equal to `{ key: value }`. For non-scalar values, identity will be disregarded by `deepEqual`.

[`notDeepEqual()`](http://api.qunitjs.com/notDeepEqual) can be used to explicitly test deep, strict inequality.

## Example:

#### Compare the value of two objects.

```js
test( "deepEqual test", function() {
  var obj = { foo: "bar" };
 
  deepEqual( obj, { foo: "bar" }, "Two objects can be the same in value" );
});
```