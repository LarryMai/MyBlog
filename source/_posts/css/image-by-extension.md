title: 如何根據 File Extension 自動顯示圖片 ?
tags:
  - CSS
feature: images/feature/css.png
date: 2019-05-16 17:11:29
---
一個常見的需求，由於不算視覺部分，直覺要使用 JavaScript 處理，其實 CSS 就能解決。

<!-- more -->

## Version

CSS 3

## Image by File Extension

![ext000](/images/css/image-by-extension/ext000.png)

若 file extension 為 `pdf`，則顯示 `pdf.svg`；若為 `doc`，則顯示 `word.svg`。

若使用 JavaScript 當然可行，可能完全使用 CSS 實現嗎 ?

## Attribute Selector 

```html
<!DOCTYPE html>
<head>
  <meta charset="UTF-8">
  <title>Image by Extenstion</title>
  <style>
    .download a {
      display: block;
      padding-left: 30px;
    }

    .download a[href $= '.pdf'] {
      background: url('pdf.svg') no-repeat;
    }

    .download a[href $= '.doc'] {
      background: url('word.svg') no-repeat;
    }
  </style>
</head>
<body>
  <div class="download">
    <a href="aaa.pdf">aaa.pdf</a>
    <a href="bbb.doc">bbb.doc</a>
  </div>
</body>
</html>
```

21 行

```html
<div class="download">
  <a href="aaa.pdf">aaa.pdf</a>
  <a href="bbb.doc">bbb.doc</a>
</div>
```

很平凡地將兩個 `<a>` 包在 `<div>` 內。

第 6 行

```css
.download a {
  display: block;
  padding-left: 30px;
}
```

* `<a>` 是 inline element，為了使其並排，將 `display` 設定為 `block`
* 圖片將使用 `background-image` 呈現，故特別將 `<a>` 設定 `padding-left` 用來顯示圖片

11 行

```css
.download a[href $= '.pdf'] {
  background: url('pdf.svg') no-repeat;
}
```

Attribute selector 有個特色，就是能搭配 regular expression。

* `$=` 表示結尾是 `.pdf`
* 使用 `background` 一次設定 `檔案位置` 與 `no-repeat`

15 行

```css
.download a[href $= '.doc'] {
  background: url('word.svg') no-repeat;
}
```

大部分情況下，使用 `background` 是方便的，它讓我們一次設定多個 property。

但在這種使用情境下，我們卻必須 `重複` 設定 `no-repeat`。

## Refactor CSS

```html
<!DOCTYPE html>
<head>
  <meta charset="UTF-8">
  <title>Image by Extenstion</title>
  <style>
    .download a {
      display: block;
      padding-left: 30px;
      background-repeat: no-repeat;
    }

    .download a[href $= '.pdf'] {
      background-image: url('pdf.svg');
    }

    .download a[href $= '.doc'] {
      background-image: url('word.svg');
    }
  </style>
</head>
<body>
  <div class="download">
    <a href="aaa.pdf">aaa.pdf</a>
    <a href="bbb.doc">bbb.doc</a>
  </div>
</body>
</html>
```

第 6 行

```css
.download a {
  display: block;
  padding-left: 30px;
  background-repeat: no-repeat;
}
```

將共用的 `no-repeat` 重構到 `.download a`，如此 `no-repeat` 只要寫一次即可。

13 行

```css
.download a[href $= '.pdf'] {
  background-image: url('pdf.svg');
}
```

改用 `background-image`，只設定 `檔案位置` 即可。

17 行

```css
.download a[href $= '.doc'] {
  background-image: url('word.svg');
}
```

不需再重複 `no-repeat`。

## Conclusion

* 善用 Attribute selector 的 regular expression，如此就不必使用到 JavaScript
* 當 property 重複設定時，可改設定到上層 selector

## Reference

[MDN](https://developer.mozilla.org/en-US/), [Attribute selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors)

