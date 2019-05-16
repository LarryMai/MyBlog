title: CSS 之 Universal Selector
tags:
  - CSS
  - CSS/Selector
feature: images/feature/css.png
date: 2019-05-15 22:32:04
---
最暴力的 Selector，一次將選取全部 Element，但 Side Effect 也最大，須小心使用。

<!-- more -->

## Version

CCS 3

## Universal Selector

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Universal Selector</title>
  <style>
    * {
      font-size: 30px;
      color: #ff0000;
    }
  </style>
</head>
<body>
  <div>Sam</div>
  <div>Kevin</div>
</body>
</html>
```

第 7 行

```css
* {
  font-size: 30px;
  color: #ff0000;
}
```

若要描述所有的 element，可使用 universal selector，以 `*` 表示。

![uni000](/images/css/selector/universal-selector/uni000.png)

## Conclusion

* 實務上較少使用 universal selector，因為 side effect 太大


