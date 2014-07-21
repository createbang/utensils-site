---
title: API Reference

toc_footers:
  - <a href='http://github.com/createbang/utensils'>Repository on GitHub</a>
  - <a href='https://www.npmjs.org/package/utensils'>Package on NPM</a>
  - <a href='/docs/utensils.html'>Annotated Source</a>

search: true
---

# Introduction

Utensils provides a rich set of design pattern abstractions for web application development.

Most modern web applications perform similar types of operations, but there is a significant lack of structure for those operations in the JavaScript environment. This library is meant to provide a toolkit of opinionated, useful, and interoperable components to make common tasks in JavaScript web applications quick and easy.

# Installation

Utensils is available on npm as a package:

$ `npm install utensils`

And on bower as a component:

$ `bower install utensils`

# Value

## extend

```js
var Grade = Utensils.Value.extend();
```

To create a Model class of your own, you extend Backbone.Model and provide instance properties, as well as optional classProperties to be attached directly to the constructor function.

## argumentName

```js
var Grade = Utensils.Value.extend({
  argumentName: 'percentage'
});

var grade = new Grade(0.7);
grade.percentage; // 0.7
```

The name of the value that represents the raw data for the object.  This value is used as the key for the value when it is stored on the instance of the object.

Default | Type | Description
----------- | ----------- | -----------
`'value'` | `String` | Key for the object input

## valueOf

Returns the raw input.

```js
var Grade = Utensils.Value.extend();
var grade = new Grade(0.7);
grade.valueOf(); // 0.7
```

The method `valueOf()` has special meaning in JavaScript.  It is used for all comparisons except for equality/inequality, since equality is by reference and not by value in JavaScript.

```js
var grade1 = new Grade(0.7);
var grade2 = new Grade(0.6);

grade1 > grade2 // true
```

## isEqualTo

Returns equality of values between two value objects.

```js
var grade1 = new Grade(0.6);
var grade2 = new Grade(0.6);

grade1.isEqualTo( grade2 ) // true
```

This method is also available as a property on the constructor for a more functional approach

```js
Grade.isEqualTo( grade1, grade2 ) // true
```

## isNotEqualTo

Returns inequality of values between two value objects.

```js
var grade1 = new Grade(0.7);
var grade2 = new Grade(0.6);

grade1.isNotEqualTo( grade2 ) // true
```

This method is also available as a property on the constructor for a more functional approach

```js
Grade.isNotEqualTo( grade1, grade2 ) // true
```

# Service



# Form



# Validator



# Query



# Presenter



# Policy



# Decorator