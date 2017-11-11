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
