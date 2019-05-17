title: CSS 之 Class Selector
tags:
  - CSS
  - CSS/Selector
feature: images/feature/css.png
date: 2019-05-15 21:36:41
---
Class Selector 為 CSS 最常用的 Selector，實務上常看到幾種寫法：`.box1, box2`、`.box1 .box2` 與 `.box1.box2`，因為很像很容易誤會其意義。

<!-- more -->

## Version

CCS 3

## Version

CCS 3

## Class Selector

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Class Selector</title>
  <style>
    .box1 {
      font-size: 30px;
      color: #ff0000;
    }
    
    .box2 {
      font-size: 30px;
      color: #ff0000;
    }
  </style>
</head>
<body>
  <div class="box1">Box 1</div>
  <div class="box2">Box 2</div>
</body>
</html>
```

19 行

```html
<div class="box1">Box 1</div>
```

HTML 部分有 `<div>`，以 `class` attribute 使用 `box1` class。

第 7 行

```css
.box1 {
  font-size: 30px;
  color: #ff0000;
}

.box2 {
  font-size: 30px;
  color: #ff0000;
}
```

既然 `<div>` 使用了 `box1` class，CSS 部分若要以 class selector 描述 `<div>`，selector 為 `.` + `class 名稱`。

每個 class 各自描述，這是最基本，也最不會搞混的寫法。

> Class selector 可套用在多個 HTML element，沒有 side effect，重複使用程度最高，還可以透過多個 class 組合 CSS property，實務上使用最多的是 class selector

```css
[class~='box1'] {
  font-size: 30px;
  color: #ff0000;
}

[class~='box2'] {
  font-size: 30px;
  color: #ff0000;
}
```

因為 CSS class 用於 HTML 的 `class` attribute，因此也可以使用 attribute selector 表示。

其中 `class ~='box1'` 表示 `box1` 在 `class` attribute 的 class list 當中，所以等效於 `.box1`。

> 雖然語法等效，但實務上不會這樣寫

![class000](/images/css/selector/class-selector/class000.png)

## Grouping Selector

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Grouping Selector</title>
  <style>
    .box1,
    .box2 {
      font-size: 30px;
      color: #ff0000;
    }
  </style>
</head>
<body>
  <div class="box1">Box 1</div>
  <div class="box2">Box 2</div>
</body>
</html>
```

第 7 行

```css
.box1,
.box2 {
  font-size: 30px;
  color: #ff0000;
}
```

class 之間以 `,` 隔開，表示這兩個 class 共用相同的 property，為了避免重複，所以寫在一起。

![class001](/images/css/selector/class-selector/class001.png)

## Descendant Combinator

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Descendant Combinator</title>
  <style>
    .box1 .box2 {
      font-size: 30px;
      color: #ff0000;
    }
  </style>
</head>
<body>
  <div class="box1">
    Box 1
    <div class="box2">Box 2</div>
    <div class="box3">Box 3
        <div class="box2">Box 2</div>
    </div>
  </div>
</body>
</html>
```

第 7 行

```css
.box1 .box2 {
  font-size: 120%;
  color: #ff0000;
}
```

Class 之間以 `空白` 隔開，表示 `box1` 下所有 `box2` 均受影響。

因此 `box3` 下的 `box2` 也會受影響， 因為都在 `box1` 下。

![class002](/images/css/selector/class-selector/class002.png)

## Multiple Class Selector

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Class Selector 1</title>
  <style>
    .box1.box2 {
      font-size: 120%;
      color: #ff0000;
    }
  </style>
</head>
<body>
  <div class="box1 box2">
    Box 1 Box 2
  </div>
  <div class="box1">
    Box 1
    <div class="box2">
        Box 2
    </div>
  </div>
  <div class="box2">
    Box 2
  </div>
</body>
</html>
```

第 7 行

```css
.box1.box2 {
  font-size: 120%;
  color: #ff0000;
}
```

Class 之間直接連在一起，表示當 `box1` 與 `box2` **組合**在一起時才會受影響。

![class003](/images/css/selector/class-selector/class003.png)

## Conclusion

* Class selector 使用 `.` + `class 名稱` 描述，且可套用多個 HTML element，side effect 最小，實務上使用最多的是 class selector
* 由於 class 以 `空白隔開` 與 `連在一起` 意義不一樣，所以 class 之間是否有 `空白` 就非常重要，不再只是 coding style 而已

## Reference

[MDN](https://developer.mozilla.org/en-US/), [Class selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Class_selectors)