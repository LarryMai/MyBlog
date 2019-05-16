title: CSS 初體驗
tags:
  - CSS
feature: images/feature/css.png
date: 2019-05-15 21:12:48
---
縱使前端技術日新月異，但其實脫離不開三個最基本技術：HTML / CSS / ECMAScript，其中視覺部分就是 CSS 。

<!-- more -->

## Version

CSS 3

## Inline

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>CSS Inline</title>
</head>
<body>
  <div style="font-size:30px;color:#ff0000">test</div>
</body>
</html>
```

將 CSS 直接寫在 HTML 的 `style` attribute 內。

第 8 行

```html
<div style="font-size:30px;color:#ff0000">test</div>
```

HTML 使用 `<div></div>` 包住 `test`，並直接使用 `style` attribute 描述 element。

優點：

* 可使用 ECMAScript 控制

缺點：

* 容易違反 DRY，將來維護困難

## Internal

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>CSS Style</title>
  <style>
    div {
      font-size: 30px;
      color: #ff0000;
    }
  </style>
</head>
<body>
  <div>test</div>
</body>
</html>
```

將 CSS 寫在 `<head>` 的 `<style>` 內。

14 行

```html
<div>test</div>
```

HTML 使用 `<div></div>` 包住 `test`，

第 7 行

```css
div {
  font-size: 30px;
  color: #ff0000;
}
```

CSS 直接以 `div` element selector 選擇，設定其 `font-size` 與 `color`。

優點：

* 適合練習或 demo 使用

缺點：

* 容易違反 DRY，將來維護困難

## External

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>CSS External</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div>test</div>
</body>
</html>
```

將 CSS 寫在獨立的 `.css` 檔案。

第 6 行

```html
<link rel="stylesheet" href="style.css">
```

使用 `<link>` 載入獨立 `.css` 檔案。

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>CSS External</title>
  <style>
    @import url("style.css");
  </style>
</head>
<body>
  <div>test</div>
</body>
</html>
```

第 6 行

```html
<style>
  @import url("style.css");
</style>
```

使用 `@import url()` 載入獨立 `.css` 檔案，但必須包在 `<style></style>` 內。

**style.css**

```css
div {
  font-size: 30px;
  color: #ff0000;
}
```

CSS 直接以 `div` type selector 選擇，設定其 `font-size` 與 `color`

優點：

* 適合實際專案使用
* 可重複使用，不違反 DRY

缺點：

* 練習時要兩個檔案對照

## Conclusion

* CSS 的 inline 寫法，容易違反 DRY，且 priority 太高，將來不容易維護
* CSS 的 internal 寫法適合練習與 demo，實際專案不建議使用
* CSS 的 external 寫法有 module 特性，容易重複使用與維護，建議用在實際專案

