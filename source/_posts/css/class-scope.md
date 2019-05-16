title: 使用 Empty Class 建立 Selector Scope
tags:
  - CSS
feature: images/feature/css.png
date: 2019-05-16 15:23:43
---
實務上會遇到有些 CSS Class，並沒有設定任何 Property，目的在於建立出 Scope，方便 Selector 選擇到我們要的 Element。

<!-- more -->

## Version

CSS 3

## Class Selector

```html
<!DOCTYPE html>
<head>
  <meta charset="UTF-8">
  <title>Class Scope</title>
  <style>
    .box {
      font-size: 30px;
      color: #ff0000;
    }
  </style>
</head>
<body>
  <div class="box">box 1</div>
  <div class="box">box 2</div>
</body>
</html>
```

`box 1` 與 `box 2` 都使用 `.box`，若我們想讓 `box 2` 為 `紅色`，直接使用 class selector 選取 `.box`，則 `box 1` 與 `box 2` 都會被選到，這顯然不是我們要的。

![class001](/images/css/class-scope/class001.png)

## Empty Class

```html
<!DOCTYPE html>
<head>
  <meta charset="UTF-8">
  <title>Class Scope</title>
  <style>
    .post .box {
      font-size: 30px;
      color: #ff0000;
    }
  </style>
</head>
<body>
  <div class="box">box 1</div>
  <div class="post">
    <div class="box">box 2</div>
  </div>
</body>
</html>
```

13 行

```html
<div class="post">
  <div class="box">box 2</div>
</div>
```

實務上會在要選擇的 `<div>` 外面再加上一層 `<div>`，並加上一個新的 class，但不描述任何 property。

第 6 行

```css
.post .box {
  color: #f00;
}
```

如此就可使用 descendant combinator 同時選取 `.post` 與 `.box` 兩個 class，這樣只會選到 `box 2`，類似替 CSS selector 創建了 scope。

![class000](/images/css/class-scope/class000.png)

## Conclusion

* CSS 的 class 並不只用在設定 property，也可以使用 empty class 建立出 scope，方便 CSS selector 選擇到我們要的 element