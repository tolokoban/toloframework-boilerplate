# toloframework-boilerplate
Convert XML files into toloframework modules.

## Parsing XML

### Attributes and children
```xml
<view>
  <item name="foo"/>
</view>
```

```js
{ name: "view", chidren: [
  { name: "item", attribs: { name: "foo" } }
]}
```

### Using namespaces
```xml
<view xmlns:x="class:org.tolokoban.views.input"
      xmlns:best="https://best-website-in-the-world.net">
  <x:date/ best:type="foobar">
</view>
```

```js
{ name: "view", children: [
  { name: "date", ns: "class:org.tolokoban.views.input", attribs: {
    "https://best-website-in-the-world.net:type": "foobar"
  }}
]}
```

### Attributes can be arrays
```xml
<div>
  <div.class>touchable</div.class>
  <div.class>elevation-8</div.class>
</div>
```
```xml
<div class="touchable">
  <div.class>elevation-8</div.class>
</div>
```
Both give this JS object:
```js
{ name: "div", attribs: { class: ["touchable", "elevation-8"] } }
```

### Attributes can be objects
```xml
<div>
  <div.event name="click" slot="onClick"/>
</div>
```
```js
{ name: "div", attribs: { event: { name: "click", slot: "onClick" } } }
```

## Attributes can be complex objects
```xml
<div a="foo">
  <div.a>bar</div.a>
  <div.a y="7">
    <x value="9" />
    <x>11</x>
  </div.a>
</div>
```
```js
{ name: "div", attribs: {
  a: [
    "foo",
    "bar",
    { y: "7", x: [{ value: "9"}, "11"] }
  ]
}}
```

### Short syntax for attributes with complex types
#### Arrays
```xml
<div class="[touchable, 666]"/>
```
```js
{ name: "div", attribs: { class: ["touchable", "666"] } }
```
#### Objects
```xml
<div a="{x:666, y:yok}"/>
```
```js
{ name: "div", attribs: { a: { x: "666", y: "yok" } } }
```
#### Long strings
```xml
<div a="{name: 'Linus Torvald'}"/>
<div license="{text: 'It\'s free!'}"/>
```
#### Nested types
```xml
<div att="{ headers: [Product, {label: Price, align: right}] }"/>
```
```js
{ name: "div", attribs: { att: {
  headers: [
    "Prouct",
    { label: "Price", align: "right" }
  ]
}}}
```
#### Commas are optional in arrays
```xml
<div class="[touchable, 666]"/>
<div class="[touchable 666]"/>
```
#### Commas are optional in objects
```xml
<div a="{x:666, y:yok}"/>
<div a="{x:666 y:yok}"/>
```
#### Keys are optional
```xml
<div att="{Link converter:boolean selected}"/>
```
```js
{ name: "div", att: { $0: "Link", converter: "boolean", $1: "selected" } }
```
Missing keys are replaces with `$n` where `n` starts from 0.

### Don't mix syntaxes for attributes
__This is not allowed:__
```js
<div.att x="36">47</div.att>
```
If you give a content, you can't give attributes because the parser don't know if you want a string (`"47"`) or an object (`{x: "36"}`).

### Stickers
```xml
<grid>
  <item grid.row="3"/>
</grid>
```
```xml
<grid>
  <item>
    <grid.row>3</grid.row>
  </item>
</grid>
```
Both give the same JS object:
```js
{ name: "view", children: [
  { name: "item", stick: { grid: { row: "3" } } }
]}
```
Stickers can be as complex as attributes.
