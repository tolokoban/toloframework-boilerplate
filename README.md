# toloframework-boilerplate
Convert XML files into toloframework modules.

## Parsing XML

### Simple case: attributes and children
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


