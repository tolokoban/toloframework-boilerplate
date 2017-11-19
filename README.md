# toloframework-boilerplate
Convert Permissive JSON files (jsnx) into toloframework modules for views.

In MVVM pattern, views only know about ViewModel also known as DataContext.
**toloframework-boilerplate** helps you to create such views by writing all the boilerplate for you.

## Usage
```js
var Boilerplate = require("./toloframework-boilerplate");
var code = Boilerplate.generateCodeFrom( viewDescription );
```

The `viewDescription` argument is an object with this mandatory attribute: `viewDescription["0"] === 'view'`.

## Examples
Permissive JSON:
```js
{view ["Hello world!"]}
```
Generated code:
```js
var Tag = require("tfw.binding.tag");

module.exports = function() {
  var e1 = new Tag("div");
  var e2 = document.createTextNode("Hello world!");
  e1.appendShild( e2 );
  Object.defineProperty( this, "$", {
    value: e1, writable: false, enumerable: true, configurable: false } );
}
```
Permissive JSON:
```js
{view P ["Hello world!"]}
```
Generated code:
```js
var Tag = require("tfw.binding.tag");

module.exports = function() {
  var e1 = new Tag("P");
  var e2 = document.createTextNode("Hello world!");
  e1.appendShild( e2 );
  Object.defineProperty( this, "$", {
    value: e1, writable: false, enumerable: true, configurable: false } );
}
```
----
Permissive JSON:
```js
{view class: tfw-view-checkbox
      class.reversed: reversed
      class.selected: value
      event.pointerdown: {toggle value} [
  {DIV class: text, textContent: {link text}}
  {DIV class: base [{DIV class: point}]}
]}
```
Generated code:
```js
var Tag = require("tfw.binding.tag");
var PropertyManager = require("tfw.binding.property-manager");

module.exports = function(dataContext) {
  var that = this;
  var e1 = new Tag({ class: "tfw-view-checkbox" });
  var e2 = new Tag({ class: "text" });
  var e3 = new Tag({ class: "base" });
  var e4 = new Tag({ class: "point" });
  e3.add( e4 );
  e1.add( e2, e3 );
  Object.defineProperty( this, "$", {
    value: e1, writable: false, enumerable: true, configurable: false } );    
  if( typeof dataContext !== 'object' ) return;
  e1.addEventListener("pointerdown", function() {
    that.dataContext.value = that.dataContext.value ? false : true;
  });  
  PropertyManager(v).on("reversed", function(value) {
    if( value ) e1.classList.add("reversed");
    else e1.classList.remove("reversed");
  });
  PropertyManager(v).on("selected", function(value) {
    if( value ) e1.classList.add("selected");
    else e1.classList.remove("selected");
  });
  PropertyManager(v).on("textContent", function(value) {
    e2.$.textContent = value;
  });
}
```

