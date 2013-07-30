# notDeepEqual()

## notDeepEqual( actual, expected, message )

**Description:** An inverted deep recursive comparison assertion, working on primitive types, arrays, objects, regular expressions, dates and functions.

#### notDeepEqual( actual, expected, message )

**actual**  
Type: [Object](http://api.jquery.com/Types#Object)  
Object or Expression being tested

**expected**  
Type: [Object](http://api.jquery.com/Types#Object)  
Known comparison value

**message**  
Type: [String](http://api.jquery.com/Types#String)  
A short description of the assertion

The `notDeepEqual()` assertion can be used just like `equal() `when comparing the value of objects, such that `{ key: value }` is equal to `{ key: value }`. For non-scalar values, identity will be disregarded by `notDeepEqual`.

[`deepEqual()`](http://api.qunitjs.com/deepEqual) can be used to explicitly test deep, strict equality.

## Example:

#### Compare the value of two objects.

```js
test( "notDeepEqual test", function() {
  var obj = { foo: "bar" };
 
  notDeepEqual( obj, { foo: "bla" }, "Different object, same key, different value, not equal" );
});
```