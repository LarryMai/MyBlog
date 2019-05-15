title: CSS 之 Attribute Selector
tags:
  - CSS
  - CSS/Selector
feature: images/feature/css.png
date: 2019-05-15 22:13:44
---
僅管是相同 Tag 的 HTML Element，，只要其 Attribute 的 Value 不一樣，就能使用 Attribute Selector 描述該 Element。

<!-- more -->

## Version

CCS 3

## Attribute Selector

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Attribute Selector</title>
  <style>
    div[title="sam"] {
      font-size: 30px;
      color: #ff0000;
    }
  </style>
</head>
<body>
  <div title="sam">Sam</div>
  <div title="mary">Mary</div>
</body>
</html>
```

14 行

```html
<div title="sam">Sam</div>
<div title="mary">Mary</div>
```

`Sam` 與 `Mary` 都使用 `<div>`，該如合描述使用 `title="sam"` 的 `<div>` 呢 ?

第 7 行

```css
div[title="sam"] {
  font-size: 30px;
  color: #ff0000;
}
```

既然 `<div>` 使用了 `title` attribute，CSS 部分若要以 attribute selector 描述 `<div>`，selector 為 `[attribute="value"]`。

![attr000](/images/css/selector/attribute-selector/attr000.png)

## Conclusion

* Attribute selector 使用 `[attribute="value"]` 描述，可套用在相同 tag 的不同 element




