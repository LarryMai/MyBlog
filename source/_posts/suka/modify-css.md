title: 如何修改 Suka 的 CSS ?
tags:
  - Hexo
  - Suka
feature: images/feature/suka.png
date: 2019-04-30 09:23:43
---
雖然可以使用 `非侵入式` 客製化 Suka 的 CSS，但若真的想從 Theme 的 CSS Source 下手一勞永逸，則必須搭配 Gulp。

<!-- more -->

## Version

Hexo 3.8.0

## Margin Issue

![css000](/images/suka/modify-css/css000.png)

Suka 預設的版型，在 desktop 的左右留有較多的空白，在 iPhone 與 iPad 則適中，所以想要調整其 CSS。

![css001](/images/suka/modify-css/css001.png)

根據 developer tool 觀察，關鍵在於 `main-container` class，其 `max-width` 的 `41.2rem` 短了些，且其定義在 `style.min.css`。

要修改 CSS，有兩種方式：

2. 在 `<head>` 掛入 `my-style.css`，蓋掉原本的 CSS
2. 直接修改 `suka/src/css/style.css`，然後使用 Gulp 壓成 `style.min.css`

兩種修改方式各有其優缺點，本文討論第 2 種方式。

## Gulp

```
$ yarn global add gulp-cli
```

Suka 會透過 Gulp 將 `style.css` 壓成 `style.min.css`，因此要先安裝 Gulp。

![css000](/images/suka/modify-css/css002.png)

## CSS

**style.css**

```css
@media screen and (min-width: 961px) {
  .main-container {
    max-width: 50.2rem;
  }
}
```

將 `themes/suka/src/css/style.css` 的 319 行 的 `max-width`，改成 `50.2rem`。

![css003](/images/suka/modify-css/css003.png)

## Minify CSS

```
$ gulp
```

在 `themes/suka` 目錄下輸入 `gulp`，對 CSS 與 ECMAScript 進行壓縮。

![css004](/images/suka/modify-css/css004.png)

![css005](/images/suka/modify-css/css005.png)

`main-container` class 的 `max-width` 改成 `50.2rem`，且明確來自 `style.min.css`。

## Gulp Watch

```
$ gulp watch
```

由於 HTML 所使用的是 `style.min.css`，若在開發階段修改 `style.css`，還要不斷的重複執行 `gulp` 去壓縮 CSS 才能預覽結果，開發將非常沒效率。

也就是若要修改 CSS，實務上會同時開兩個 process，一個 process 執行 `hexo server`，另一個 process 執行 `gulp watch`，如此只要 `style.css` 被修改，將立刻被壓縮成 `style.min.css`，browser 也能第一時間看到結果。

![css006](/images/suka/modify-css/css006.png)

![css007](/images/suka/modify-css/css007.png)

## Conclusion

* 直接修改的優點是經過壓縮，缺點是日後無法更新 theme
* 必須搭配 `gulp watch` 才會方便開發

## Reference

[Suka](https://theme-suka.skk.moe), [開發指南](https://theme-suka.skk.moe/docs/dev/)

