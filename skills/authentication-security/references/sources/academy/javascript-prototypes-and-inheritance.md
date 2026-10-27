# JavaScript prototypes and inheritance

> Converted source material; attribution: PortSwigger Web Security Academy. Snapshot processed 2026-10-27.
> Source SHA-256: `e03497996af483c254bec97adb570357970213a8fff8f223d6163801d02515b6`.
> Conversion: Local English OCR, all 5 text segments. Source opinions and historical examples are preserved, not automatically endorsed.
> OCR literals, payloads, and complex table alignment are not character-verified throughout. Use the curated guidance and exact-text sources for implementation; do not execute OCR examples without checking the publisher.


> JavaScript prototypes and inheritance

Prototype pollution languages. In this section, we'll provide a basic overview of how this works, which should give you enough of an understanding to follow our learning materials on prototype pollution vulnerabilities.

## What is an object in JavaScript?

A JavaScript object is essentially just a collection of key : value pairs known as "properties". For example, the following object could represent a user:

```text
const user —
```

"wiener" , username : user ld: 01234, isAdmin: false

You can access the properties of an object by using either dot notation or bracket notation to refer to their respective keys: // "wiener" user . username user [ 'user ld'] // 01234

As well as data, properties may also contain executable functions. In this case, the function is known as a "method".

```text
const user
```

"wiener' username : userld: 01234, exampleMethod: function ( ) { // do something

The example above is an "object literal", which means it was created using curly brace syntax to explicitly declare its properties and their initial values. However, it's important to understand that almost everything in JavaScript is an object under the hood. Throughout these materials, the term "object" refers to all entities, not just object literals.

## What is a prototype in JavaScript?

Every object in JavaScript is linked to another object of some kind, known as its prototype. By default, JavaScript automatically assigns new objects one of its built-in prototypes. For example, strings are automatically assigned the built-in String . prototype . You can see some more examples of these global prototypes below:

---

Obj ect . get PrototypeOf (myObj ect) ; // Object . prototype

```text
let myString —
```

Obj ect . get PrototypeOf (myString) ; // String . prototype

```text
let myArray =
```

// Array . prototype Obj ect . get PrototypeOf (myArray) ;

```text
let myNumber =
```

1; Obj ect . get PrototypeOf (myNumber) ; // Number . prototype

Objects automatically inherit all of the properties of their assigned prototype, unless they already have their own property with the same key. This enables developers to create new objects that can reuse the properties and methods of existing objects. The built-in prototypes provide useful properties and methods for working with basic data types. For example, the String . prototype object has a toLowerCase ( ) method. As a result, all strings automatically have a ready-to-use method for converting them to lowercase. This saves developers having to manually add this behavior to each new string that they create.

## How does object inheritance work in JavaScript?

Whenever you reference a property of an object, the JavaScript engine first tries to access this directly on the object itself. If the object doesn't have a matching property, the JavaScript engine looks for it on the object's prototype instead. Given the following objects, this enables you to reference myObJ ect . propertyA , for example:

existingObject propertyA propertyB

is prototype of I

myObject propertyC

You can use your browser console to see this behavior in action. First, create a completely empty object:

```text
let myObj ect —
```

o;

Next, type myObj ect followed by a dot. Notice that the console prompts you to select from a list of properties and methods:

---

top Filter Console was cleared undefined

```text
let myobject =
```

undefined

## myobject .1_defineGetter

defineGetter definesetter lookupGetter lookupsetter —proto constructor hasownproperty i sprototypeOf Even though there are no properties or methods defined for the object itself, it has inherited some from the built-in Obj ect . proto type

## The prototype chain

Note that an object's prototype is just another object, which should also have its own prototype, and so on. As virtually everything in JavaScript is an object under the hood, this chain ultimately leads back to the top-level Object . prototype , whose prototype is simply null

Object.prototype

is prototype of

String.prototype Array.prototype Number.prototype

is prototype of I is prototype of I is prototype of I

Crucially, objects inherit properties not just from their immediate prototype, but from all objects above them in the prototype chain. In the example above, this means that the username object has access to the properties and methods of both String . prototype and Object . prototype

## Accessing an object's prototype using proto

Every object has a special property that you can use to access its prototype. Although this doesn't have a formally standardized name, proto is the de facto standard used by most browsers. If you're familiar with object-oriented languages, this property serves as both a getter and setter for the object's prototype. This means you can use it to read the prototype and its properties, and even reassign them if necessary. As with any property, you can access proto using either bracket or dot notation:

username . proto username [ proto

---

// String . prototype username . proto // Obj ect . prototype username . proto proto proto proto proto // null username .

## Modifying prototypes

Although it's generally considered bad practice, it is possible to modify JavaScript's built-in prototypes just like any other object. This means developers can customize or override the behavior of built-in methods, and even add new methods to perform useful operations. For example, modern JavaScript provides the trim ( ) method for strings, which enables you to easily remove any leading or trailing whitespace. Before this built-in method was introduced, developers sometimes added their own custom implementation of this behavior to the String . prototype object by doing something like this: String . prototype. removeWhitespace —

```text
function ( ) {
```

// remove leading and trailing whitespace

Thanks to the prototypal inheritance, all strings would then have access to this method:

```text
let searchTerm =
```

e X amp e . searchTerm. removeWhitespace ( ) ; // "example"

What next? Now that you have a basic understanding of how prototypes and inheritance work in JavaScript, let's take a look at how implementation flaws can lead to a security vulnerability known as prototype pollution:

- What is prototype pollution?

---
