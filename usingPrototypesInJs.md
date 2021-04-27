function doSomething(){}
console.log( doSomething.prototype );
//  It does not matter how you declare the function, a
//  function in JavaScript will always have a default
//  prototype property.
//  (Ps: There is one exception that arrow function doesn't have a default prototype property)
var doSomething = function(){};
console.log( doSomething.prototype );
As seen above, doSomething() has a default prototype property, as demonstrated by the console. After running this code, the console should have displayed an object that looks similar to this.

{
    constructor: ƒ doSomething(),
    __proto__: {
        constructor: ƒ Object(),
        hasOwnProperty: ƒ hasOwnProperty(),
        isPrototypeOf: ƒ isPrototypeOf(),
        propertyIsEnumerable: ƒ propertyIsEnumerable(),
        toLocaleString: ƒ toLocaleString(),
        toString: ƒ toString(),
        valueOf: ƒ valueOf()
    }
}
We can add properties to the prototype of doSomething(), as shown below.

function doSomething(){}
doSomething.prototype.foo = "bar";
console.log( doSomething.prototype );
This results in:

{
    foo: "bar",
    constructor: ƒ doSomething(),
    __proto__: {
        constructor: ƒ Object(),
        hasOwnProperty: ƒ hasOwnProperty(),
        isPrototypeOf: ƒ isPrototypeOf(),
        propertyIsEnumerable: ƒ propertyIsEnumerable(),
        toLocaleString: ƒ toLocaleString(),
        toString: ƒ toString(),
        valueOf: ƒ valueOf()
    }
}
We can now use the new operator to create an instance of doSomething() based on this prototype. To use the new operator, call the function normally except prefix it with new. Calling a function with the new operator returns an object that is an instance of the function. Properties can then be added onto this object.

Try the following code:

function doSomething(){}
doSomething.prototype.foo = "bar"; // add a property onto the prototype
var doSomeInstancing = new doSomething();
doSomeInstancing.prop = "some value"; // add a property onto the object
console.log( doSomeInstancing );
This results in an output similar to the following:

{
    prop: "some value",
    __proto__: {
        foo: "bar",
        constructor: ƒ doSomething(),
        __proto__: {
            constructor: ƒ Object(),
            hasOwnProperty: ƒ hasOwnProperty(),
            isPrototypeOf: ƒ isPrototypeOf(),
            propertyIsEnumerable: ƒ propertyIsEnumerable(),
            toLocaleString: ƒ toLocaleString(),
            toString: ƒ toString(),
            valueOf: ƒ valueOf()
        }
    }
}
As seen above, the __proto__ of doSomeInstancing is doSomething.prototype. But, what does this do? When you access a property of doSomeInstancing, the browser first looks to see if doSomeInstancing has that property.

If doSomeInstancing does not have the property, then the browser looks for the property in the __proto__ of doSomeInstancing (a.k.a. doSomething.prototype). If the __proto__ of doSomeInstancing has the property being looked for, then that property on the __proto__ of doSomeInstancing is used.

Otherwise, if the __proto__ of doSomeInstancing does not have the property, then the __proto__ of the __proto__ of doSomeInstancing is checked for the property. By default, the __proto__ of any function's prototype property is window.Object.prototype. So, the __proto__ of the __proto__ of doSomeInstancing (a.k.a. the __proto__ of doSomething.prototype (a.k.a. Object.prototype)) is then looked through for the property being searched for.

If the property is not found in the __proto__ of the __proto__ of doSomeInstancing, then the __proto__ of the __proto__ of the __proto__ of doSomeInstancing is looked through. However, there is a problem: the __proto__ of the __proto__ of the __proto__ of doSomeInstancing does not exist. Then, and only then, after the entire prototype chain of __proto__'s is looked through, and there are no more __proto__s does the browser assert that the property does not exist and conclude that the value at the property is undefined.

Let's try entering some more code into the console:

function doSomething(){}
doSomething.prototype.foo = "bar";
var doSomeInstancing = new doSomething();
doSomeInstancing.prop = "some value";
console.log("doSomeInstancing.prop:      " + doSomeInstancing.prop);
console.log("doSomeInstancing.foo:       " + doSomeInstancing.foo);
console.log("doSomething.prop:           " + doSomething.prop);
console.log("doSomething.foo:            " + doSomething.foo);
console.log("doSomething.prototype.prop: " + doSomething.prototype.prop);
console.log("doSomething.prototype.foo:  " + doSomething.prototype.foo);
This results in the following:

doSomeInstancing.prop:      some value
doSomeInstancing.foo:       bar
doSomething.prop:           undefined
doSomething.foo:            undefined
doSomething.prototype.prop: undefined
doSomething.prototype.foo:  bar