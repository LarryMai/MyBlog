title: CSS 之 Type Selector
tags:
  - CSS
  - CSS/Selector
feature: images/feature/css.png
date: 2019-05-15 21:28:30
---
Type Selector 會一次影響所有 HTML Tag，直接以 Tag 名稱描述即可，不用任何符號。

<!-- more -->

## Version

CCS 3

## Type Selector

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Type Selector</title>
  <style>
    div {
      font-size: 30px;
      color: #ff0000;
    }
  </style>
</head>
<body>
  <div>Sam</div>
</body>
</html>
```

14 行

```html
<div>Sam</div>
```

HTML 部分有 `<div>`。

第 7 行

```css
div {
  font-size: 30px;
  color: #ff0000;
}
```

CSS 部分若要以 type selector 描述 `<div>`，selector 直接使用 tag 名稱即可。

![type000](/images/css/selector/type-selector/type000.png)

## Conclusion

* 或許你會覺得用 Tag Selector 較傳神，不過 Type Selector 為 W3C spec 的標準用語
* Type selector 直接使用 tag 名稱描述，但會影響所有 tag，side effect 最大

