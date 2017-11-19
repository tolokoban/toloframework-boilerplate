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
{view [
  {P ["Hello world!"]}
]}
```
Generated code:
```js
var Tag = require("tfw.binding.tag");

module.exports = function() {
  var e1 = new Tag("p", ["textContent"]);
  e1.textContent = "Hello world!";
  Object.defineProperty( this, "$", {
    value: e1, writable: false, enumerable: true, configurable: false } );
}
```
----
Permissive JSON:
```js
{view [
  {DIV class: tfw.view.checkbox
       class.reversed: {link reversed}
       class.selected: {link value} [
    {DIV class: text, textContent: {link text}}
    {DIV class: base [{DIV class: point}]}
  ]}
]}
```
Generated code:
```js
var Tag = require("tfw.binding.tag");

module.exports = function() {
  var e1 = new Tag("p", ["textContent"]);
  e1.textContent = "Hello world!";
  Object.defineProperty( this, "$", {
    value: e1, writable: false, enumerable: true, configurable: false } );
}
```

