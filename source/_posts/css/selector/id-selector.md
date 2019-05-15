title: CSS 之 ID Selector
tags:
  - CSS
  - CSS/Selector
feature: images/feature/css.png
date: 2019-05-15 21:58:44
---
CSS 也可以直接以 ID 描述 Element，這種方式速度最快，但因為 ID 不能重複，只能套用在單一 Element。

<!-- more -->

## Version

CCS 3

## ID Selector

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>ID Selector</title>
  <style>
    #name {
      font-size: 30px;
      color: #ff0000;
    }
  </style>
</head>
<body>
  <div id="name">Sam</div>
</body>
</html>
```

14 行

```html
<div id="name">Sam</div>
```

HTML 部分有 `<div>`，以 `id` attribute 使用 `name` id。

第 7 行

```css
#name {
  font-size: 30px;
  color: #ff0000;
}
```

既然 `<div>` 使用了 `name` id，CSS 部分若要以 ID selector 描述 `<div>`，selector 為 `#` + `ID 名稱`。

![id000](/images/css/selector/id-selector/id000.png)

## Conclusion

* ID selector 使用 `#` + `id 名稱` 描述，只能套用單一 HTML element，且 priority 太高，實務上較少使用




