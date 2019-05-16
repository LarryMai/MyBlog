title: 如何使用 Class Composition 組合 CSS ?
tags:
  - CSS
feature: images/feature/css.png
date: 2019-05-16 14:40:00
---
CSS 允許我們將重複 Property 抽出成獨立 Class，然後 HTML Element 再以 Class Composition 完成我們要的視覺效果。

<!-- more -->

## Version

CCS 3

## Before Refactoring

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Class Combination</title>
  <style>
    .fp {
      font-size: 120%;
      font-weight: normal;
      color: #ff0000;
    }
    .rxjs {
      font-size: 120%;
      font-weight: normal;
      color: #00ff00;
    }
    .speaking {
      font-size: 120%;
      font-weight: normal;
      color: #0000ff;
    }
  </style>
</head>
<body>
  <div class="fp">FP in JavaScript</div>
  <div class="rxjs">RxJS in Action</div>
  <div class="speaking">Speaking JavaScript</div>
</body>
</html>
```

第 7 行

```css
.fp {
  font-size: 120%;
  font-weight: normal;
  color: #ff0000;
}
.rxjs {
  font-size: 120%;
  font-weight: normal;
  color: #00ff00;
}
.speaking {
  font-size: 120%;
  font-weight: normal;
  color: #0000ff;
}
```

我們發現 `fp`、`rxjs` 與 `speaking` 有相同的 property：`font-size` 與 `font-weight`，如此違反 DRY，將來維護 CSS 會有困難。

## After Refactoring

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Class Combination</title>
  <style>
    .main {
      font-size: 120%;
      font-weight: normal;
    }
    
    .fp {
      color: #ff0000;
    }
    
    .rxjs {
      color: #00ff00;
    }
    
    .speaking {
      color: #0000ff;
    }
  </style>
</head>
<body>
  <div class="main fp">FP in JavaScript</div>
  <div class="main rxjs">RxJS in Action</div>
  <div class="main speaking">Speaking JavaScript</div>
</body>
</html>
```

第 7 行

```css
.main {
  font-size: 120%;
  font-weight: normal;
}
```

將相同的 property： `font-size` 與 `font-weight` 抽出成 `main` class。

 12 行

 ```css
.fp {
  color: #ff0000;
}

.rxjs {
  color: #00ff00;
}

.speaking {
  color: #0000ff;
}
 ```

不同的 property 維持在原本 `fp`、`rxjs` 與 `speaking` class。

23 行

```html
<div class="main fp">FP in JavaScript</div>
```

因此 element 就必須組合 `main` 與 `fp` 兩個 class，中間以 `空白` 間隔即可。

![class000](/images/css/class-composition/class000.png)

## Conclusion

* CSS 雖然主要目的在處理視覺部分，但並不表示 CSS 不需維護，因此原本 ECMAScript 需要注意的部分，CSS 仍要注意，尤其是 DRY，應避免 copy paste
* 如同 FP 的核心 Function Composition，CSS 的核心亦是 Class Composition。雖然 CSS 名為 class，其實與 OOP 的 class 不同，但重點仍是 composition，應該以 compose class 開發，避免以 override class 處理




