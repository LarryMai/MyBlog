title: 如何對 CSS Extract Variable ?
tags:
  - WebStorm
  - CSS
feature: images/feature/webstorm.png
date: 2019-05-27 12:52:28
---
WebStorm 在 2019.1 開始支援 Extract Variable，可將 CSS 重複部分直接抽成 CSS Variable，方便日後維護。

<!-- more -->

## Version

macOS Mojave 10.14.5
WebStorm 2019.1.2

## CSS

```css
.box1 {
  color: #f00;
  font-size: 16px;
}

.box2 {
  color: #f00;
  font-size: 20px;
}
```

很明顯 `color: #f00` 是重複的，因此我們想將 `#f00` 抽成 variable。

## Extract Variable

![variable000](/images/webstorm/css-extract-variable/variable000.gif)

1. 將 cursor 放在要抽取的 `#f00` 上
2. 按熱鍵 `⌃ T` 顯示 `Refactor This`
3. 選擇 `4. Variable...`
4. 選擇 `Replace all 2 occurences`

會抽出 property 同名 variable。

## Rename Variable

![variable001](/images/webstorm/css-extract-variable/variable001.gif)

Extract Variable 預設以 property 命名，你也可重構成其他名稱。

1. 將 cursor 放在 variable 上
2. 按熱鍵 `⌃ T` 顯示 `Refactor This`
3. 選擇 `1. Rename...`
4. 輸入新的名稱

將原本 `color` 全改成 `red`。

## Conclusion

* 有了 extract Variable 與 rename variable，CSS 也能如 ECMAScript 做初步重構