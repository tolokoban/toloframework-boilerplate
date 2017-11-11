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
  { name="item", attribs: { name: "foo" } }
]}
```

### Using namespaces
```xml
<view xmlns:x="class:org.tolokoban.views.input">
  <x:date/>
</view>
```

```js
{ name: "view", children: [
  { name: "date", ns: "class:org.tolokoban.views.input" }
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

## Short syntax for attributes with complex types
```xml
<div class="[touchable, 666]"/>
```
```js
{ name: "div", attribs: { class: ["touchable", "666"] } }
```

## Don't mix syntaxes for attributes
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
