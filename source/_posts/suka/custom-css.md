title: 如何擴充 Suka 的 CSS ?
tags:
  - Hexo
  - Suka
feature: images/feature/suka.png
date: 2019-04-29 10:23:43
---
假如你想修改 CSS，卻又不想修改 Suka 原本的 CSS，可在 `<header>` 或 `<footer>` 插入自己的 CSS，以 `非侵入式` 修改。

<!-- more -->

## Version

Hexo 3.8.0

## Margin Issue

![css000](/images/suka/custom-css/css000.png)

Suka 預設的版型，在 desktop 的左右留有較多的空白，在 iPhone 與 iPad 則適中，所以想要調整其 CSS。

![css001](/images/suka/custom-css/css001.png)

根據 developer tool 觀察，關鍵在於 `main-container` class，其 `max-width` 的 `41.2rem` 短了些，且其定義在 `style.min.css`。

要修改 CSS，有兩種方式：

1. 在 `<head>` 掛入 `my-style.css`，蓋掉原本的 CSS
2. 直接修改 `suka/src/css/style.css`，然後使用 Gulp 壓成 `style.min.css`

兩種修改方式各有其優缺點，本文討論第 1 種方式。

## Add Custom CSS

在 Suka 文件中的 [Add custom code](https://theme-suka.skk.moe/docs/en/expert/) 提到允許我們在 `<head>` 與 `<footer>` 增加自己的 code，以 YAML 格式檔案放在 `source` 目錄，如此我們就能在不修改 Suka 既有 `style.min.css` 前提下，改變 Suka 視覺外觀。

**my-style.css**

```css
@media screen and (min-width: 961px) {
  .main-container {
      max-width: 50.2rem;
  }
}
```

在 `source` 目錄下新增 `assets` 目錄，並建立 `my-style.css`。

將 `main-container` 的 `max-width` 修改成 `50.2rem`。

> 由於 `my-style.css` 是建立在 `source` 目錄下，而非 `themes/suka` 目錄下，因此不會影響日後使用 `git pull` 更新 Suka

![css002](/images/suka/custom-css/css002.png)

**head.yml**

```yaml
myStyle:
  - '<link rel="stylesheet" href="/assets/my-style.css">'
```

在 `source` 目錄下新增 `_data` 目錄，並建立 `head.yml`。

第 1 行

```yaml
myStyle:
```

Script 名稱，可自行定義。

第 2 行

```yaml
- '<link rel="stylesheet" href="/assets/my-style.css">'
```

要插入的 HTML，必須以 `''`  框起來。

![css003](/images/suka/custom-css/css003.png)

> 由於 `head.yml` 是建立在 `source` 目錄下，而非 `themes/suka` 目錄下，因此不會影響日後使用 `git pull` 更新 Suka

![css004](/images/suka/custom-css/css004.png)

`main-container` class 的 `max-width` 改成 `50.2rem`，且明確來自 `my-style.css`。

## Conclusion

* 非侵入式修改的優點是日後來可更新 theme，缺點是沒有經過壓縮

## Reference

[Suka](https://theme-suka.skk.moe/), [Add custom code](https://theme-suka.skk.moe/docs/en/expert/)

