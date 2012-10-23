****************************************
IPEP-5: Javascript structure and testing
****************************************

:Author:
    Bussonnier matthias <bussonniermatthias@gmail.com>
:Status:
    Draft
:Created:
    2012-10-23
:Updated:
    2012-10-23

Abstract
========

The current Javascript implementation of the notebook lacks a lot of good programming practice in documentation and testing. This is a proposal to see what can be enhanced about this. 

Alternate javascript dialect
============================

Before analyzing the refactor we can make about javascript, we should considere the use of languages that compiles to javascript. The goal is not efficiency, even if some languages like [Dart](http://www.dartlang.org/) might compile to faster than javascript by doing their own garbage collection, 
but to increase readability and ease of writing. 

A good candidate id [CoffeScript](http://coffeescript.org/) which is supported in many framework.

## Advantages

 * Much more compact than javascript. 
 * Most of the time more readable.
 * list comprehension, iterable, function default argument !
 * "Real" inheritance. 
 * can sometime be directly embeded as `text/coffescript`so no server side compilation.
 * compilation can merge all coffe files into one and minify it.
 * can be used side-by-side with js.

## Drawbacs

 * Syntax sometime too close to python which leads to stupid mistake
   * no `:` after for
   * `if not xxx?` instead `if xxx is not nul`
 * might need extra compile time
 * line error number does not match ( but generated js is perfectly readable)
   
## Proposed Usage

  * use a coffescript foolder
  * a javascript folder
  * add a build target that compiles all the coffescript files into a `compiled_js` folder.

General Javascript Structure
============================
To force better reusability and testability of JS, this IPEP proposes the use of `require` (equivalent of python import), as well as the stronger distinction between model and view. Especially the use of jQuery should be avoided as much as possible in model part. This would allow separate testing of each component.

## use BackBone/underscore
[Backbone](http://backbonejs.org/) and [underscore](http://underscorejs.org/) are respectively a MVC framework and utility belt for javascript (first require the second). They have the advantage over jQuery that they are not made to work with the DOM and can be used server side in [node](http://nodejs.org/) for example. This should allow to move/replicate the notebook document model from the client to the server side without recoding it. And allow Headless testing.

## Use of require
Backbone seem to already make assumption about the project file structure to enforce model and view separation. To better force reusability this IPEP suggest the systematic use of require.

### CommonJS require
CommonJS require take the form `myvar = require('mymodule')`, mainly use in server-side script. 
It is the equivalent of python `import mymodule as myvar`. It is synchrone meaning that call will block on the line until mymodule is loaded. 

### RequireJS require
This require takes a callbacks and does not return `require('mymodule', function(...){....})`. It is both compatible with server side and client side libraries, and is asynchronous (better for clien side). Though it is a little more complex to use, and does not always work out of the box with all library. 

Require ships with a tool that seem to be able to convert from commonJS to RequireJS version, as well as concatenate and minifies the given script.


Javascript/Coffescript Documentation
====================================

Sphinx equivalent for JS seem to be [jsdoc3](https://github.com/jsdoc3/jsdoc) which use comment above the function to generate documentation. It seem to have the advantages of being able to document *config object*,  which is not possible by only looking at the function signature.

Need digging.

JS Testing.
===========

Separate component with node. 

Interface with phantomJS.


Miscs
=====

Maintaining external js libraries could be done via tools like [bower](http://twitter.github.com/bower/) and [npm](https://npmjs.org/) which should ease the transition between versions
