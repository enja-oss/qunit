# throws()

## throws( block, expected, message )

**Description:** Assertion to test if a callback throws an exception when run.

#### throws( block, expected, message )

**block**  
Type: [function](http://api.jquery.com/Types/#Function)()  
Function to execute

**expected**  
Type: [Object](http://api.jquery.com/Types#Object)  
Known comparison value

**message**  
Type: [String](http://api.jquery.com/Types#String)  
A short description of the assertion

When testing code that is expected to throw an exception based on a specific set of circumstances, use throws() to catch the error object for testing and comparison.

## Example:

#### Assert the correct error message is received for a custom error object.

```js
test( "throws", function() {
 
  function CustomError( message ) {
    this.message = message;
  }
 
  CustomError.prototype.toString = function() {
    return this.message;
  };
 
  throws(
    function() {
      throw "error"
    },
    "throws with just a message, no expected"
  );
 
  throws(
    function() {
      throw new CustomError();
    },
    CustomError,
    "raised error is an instance of CustomError"
  );
 
  throws(
    function() {
      throw new CustomError("some error description");
    },
    /description/,
    "raised error message contains 'description'"
  );
});
```