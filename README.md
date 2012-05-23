# es5-ext - ECMAScript5 extensions

Methods, functions and objects that are not part of the standard, written with
EcmaScript conventions in mind.

## Installation

Can be used in any environment that implements EcmaScript 5th edition.  
Many extensions will also work with ECMAScript 3rd edition, if they're not let [es5-shim](https://github.com/kriskowal/es5-shim) be your aid.

To use it with node.js:

	$ npm install es5-ext

For browser, you can create custom toolset with help of
[modules-webmake](https://github.com/medikoo/modules-webmake)

## Usage

__es5-ext__ mostly offer methods (not functions) which can directly be
assigned to native object's prototype:

	Function.prototype.curry = require('es5-ext/lib/Function/prototype/curry');

	Array.prototype.flatten = require('es5-ext/lib/Array/prototype/flatten');

	String.prototype.startsWith = require('es5-ext/lib/String/prototype/starts-with');

However, extending native prototypes is controversial and in general discouraged,
most will agree that __it's ok only if we own the context__ (see
[extending-javascript-natives](http://javascriptweblog.wordpress.com/2011/12/05/extending-javascript-natives/)
for more views on that matter).  
So when you don't want to extend native prototypes you can use methods as
functions:

	var util = {};
	var call = Function.prototype.call;

	util.curry = call.bind(require('es5-ext/lib/Function/prototype/curry'));

	util.flatten = call.bind(require('es5-ext/lib/Array/prototype/flatten'));

	util.startsWith = call.bind(require('es5-ext/lib/String/prototype/starts-with'));

As with native ones most methods are generic and can be run on any object.
In more detail:

* `Array.prototype` methods can be run on any object (any
value that's neither _null_ nor _undefined_),
* `Date.prototype` methods should be called only on `Date` instances.
* `Function.prototype` methods can be called on any callable objects (not
necessarily functions)
* `Number.prototype` & `String.prototype` methods can be called on any value, in
case of Number it’ll be degraded to number, in case of string it’ll be
degraded to string.

API doesn't provide any methods for `Object.prototype` as extending such in any case should be avoided. All `Object` utils are provided as fuctions.

# API

## Global extensions

### global

Object that represents global scope

### guid()

Returns globally unique identifier, it is string starting with digit and followed by any characters from _0-9_ and _a-z_ range. Length of string is 9 characters but may increase over time.  
Simple and friendly implementation for common web application purpose. It's format is different from [official GUID format](http://en.wikipedia.org/wiki/Globally_unique_identifier)

### isPrimitive(x)

Whether given value is a primitive

### reserved

List of EcmaScript 5th edition reserved keywords.  
Additionally under _keywords_, _future_ and _futureStrict_ properties we have lists grouped thematically.

### validValue(x)

Throws error if given value is `null` or `undefined`, otherwise returns `true`.

## Array Constructor extensions

### generate([length[, …fill]])

Generate an array of pregiven _length_ built of repeated arguments.

### of([…items])

_In EcmaScript 6th Edition draft_  
Create an array from given arguments.

## Array Prototype extensions

### binarySearch(compareFn)

In __sorted__ list search for index of item for which _compareFn_ returns value closest to _0_.  
It's variant of binary search algorithm

### clear()

Clears the array

### commonLeft([…lists])

Returns first index at which _lists_ start to differ

### compact()

Returns a copy of the list with all falsy values removed.

### contains(searchElement)

Whether list contains the given value.

### copy()

Returns a copy of the list

### diff(other)

Returns the array of elements that are present in context list but not present in other list.

### eIndexOf(searchElement[, fromIndex])

[_egal_](http://wiki.ecmascript.org/doku.php?id=harmony:egal) version of `indexOf` method

### exclusion([…lists]])

Returns the array of elements that are found only in context list or lists given in arguments.

### find(query[, thisArg])

Return first element for which given function returns true

### first()

Returns value for first declared index

### firstIndex()

Returns first declared index of the array

### flatten()

Returns flattened version of the array

### forEachRight(cb[, thisArg])

`forEach` starting from last element

### group(cb[, thisArg])

Group list elements by value returned by _cb_ function

### indexesOf(searchElement[, fromIndex])

Returns array of all indexes of given value

### intersection([…lists])

Computes the array of values that are the intersection of all lists (context list and lists given in arguments)

### last()

Returns value for last declared index

### lastIndex()

Returns last declared index of the array

### remove(value)

Remove value from the array

### someRight(cb[, thisArg])

`some` starting from last element

### uniq()

Returns duplicate-free version of the array

## Boolean Constructor extensions

### isBoolean(x)

Whether value is boolean

## Date Constructor extensions

### isDate(x)

Whether value is date instance

## Date Prototype extensions

### copy(date)

Returns a copy of the date object

### daysInMonth()

Returns number of days of date's month

### floorDay()

Sets the date time to 00:00:00.000

### floorMonth()

Sets date day to 1 and date time to 00:00:00.000

### floorYear()

Sets date month to 0, day to 1 and date time to 00:00:00.000

### format(pattern)

Formats date up to given string. Supported patterns:

* `%Y` - Year with century, 1999, 2003
* `%y` - Year without century, 99, 03
* `%m` - Month, 01..12
* `%d` - Day of the month 01..31
* `%H` - Hour (24-hour clock), 00..23
* `%M` - Minute, 00..59
* `%S` - Second, 00..59
* `%L` - Milliseconds, 000..999

## Error Constructor extensions

### isError(x)

Whether value is error.  
It returns true for all Error instances and Exception host instances (e.g. DOMException)

## Error Prototype extensions

### throw()

Throws error

## Function Constructor extensions

### arguments([…args])

Returns arguments object

### context()

Returns context in which function was called

_context.call(x)  =def  x_

### i(x)

Identity function. Returns first argument

_i(x)  =def  x_

### insert(name, value)

Returns a function that will set _name_ to _value_ on given object

_insert(name, value)(obj)  =def  object\[name\] = value_

### invoke(name[, …args])

Returns a function that takes an object as an argument, and applies object's
_name_ method to arguments.  
_name_ can be name of the method or method itself.

_invoke(name, …args)(object, …args2)  =def  object\[name\]\(…args, …args2\)_

### isArguments(x)

Whether value is arguments object

### isFunction(arg)

Wether value is instance of function

### k(x)

Returns a constant function that returns pregiven argument

_k(x)(y)  =def  x_

### noop()

No operation function

### pluck(name)

Returns a function that takes an object, and returns the value of its _name_
property

_pluck(name)(obj)  =def  obj[name]_

### remove(name)

Returns a function that takes an object, and deletes object's _name_ property

_remove(name)(obj)  =def  delete obj[name]_

## Function Prototype extensions

### chain(fn0[, fn1[, ...]])
### curry([n])
### flip()
### lock([arg0[, arg1[, ...])
### match()
### memoize([, length[, resolvers]])
### not()
### partial([arg0[, arg1[, ...])
### s(fn)
### silent([arg0[, arg1[, ...])
### wrap(fn)

## Math Object extensions

### sign(n)

## Number Constructor extensions

### getAutoincrement(start, step)
### getPad(length[, precision])
### isNaN(arg)
### isNumber(arg)
### toInteger(arg)
### toUint32(arg)

## Number Prototype extensions

### isLessOrEqual(n)
### isLess(n)
### subtract(n)

## Object Constructor extensions

### assertCallable(arg)
### bindMethods(obj[, context[, source]])
### compact(obj)
### clear(obj)
### clone(obj)
### compare(arg1, arg2)
### copy(obj[, deep])
### count(obj)
### descriptor
### diff(obj1, obj2)
### extend(obj, [properties])
### every(obj, cb[, thisArg[, compareFn]])
### filter(obj, cb[, thisArg])
### flatten(obj)
### forEach(obj, cb[, thisArg[, compareFn]])
### get(obj, key)
### getCompareBy(name)
### getPropertyNames()
### getSet(value)
### is(x, y)
### isCallable(arg)
### isCopy(obj1, obj2)
### isEmpty(obj)
### isList(arg)
### isObject(arg)
### isPlainObject(arg)
### keyOf(obj, searchValue)
### map(obj, cb[, thisArg])
### mapKeys(obj, cb[, thisArg])
### mapToArray(obj[, cb[, thisArg[, compareFn]]])
### merge(obj, arg)
### mergeProperties(obj, arg)
### override(obj, properties)
### plainCreate(obj[, properties])
### plainExtend(obj[, properties])
### set(obj, key, value)
### some(obj, cb[, thisArg[, compareFn]])
### toArray(obj)
### unset(obj, key)
### values(obj)

## RegExp Constructor extensions

### isRegExp(arg)

## String Constructor extensions

### getFormat(map)
### getIndent(indentString)
### getPad(fill[, n])
### getPrefixWith(prefix)
### isString(arg)

## String Prototype extensions

### caseInsensitiveCompare(str)
### contains(searchString)
### dashToCamelCase()
### endsWith()
### isNumeric()
### last()
### repeat()
### startsWith()
### trimCommonLeft(str0[, str1[, ...]])

## Tests

	$ npm test
