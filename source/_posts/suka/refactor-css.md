title: 如何重構 Suka CSS ?
tags:
  - Hexo
  - Suka
feature: images/feature/suka.png
date: 2019-05-09 14:56:37
---
Suka 本身已經支援 RWD，唯在視覺要求方面，可能與原作者稍有差異，本文總結自己重構其 CSS 心得。

<!-- more -->

## Version

Hexo 3.8.0

## CSS

我主要將 Suka CSS 做了以下重構：

* CSS Module
* Device margin
* Device font size

## CSS Module

Suka 的 CSS 主要放在 `suka/src/css/style.css` 下，我將之重構如下：

**style.css**

```css
@import url("global.css");
@import url("menu.css");
@import url("icon.css");
@import url("header.css");
@import url("main-layout.css");
@import url("post-entry.css");
@import url("post.css");
@import url("post-content.css");
@import url("footer.css");
@import url("tags-cloud.css");
@import url("timeline.css");
@import url("archive.css");
@import url("search.css");
@import url("link.css");
```

原本 Suka 是將整個 blog 的 CSS 全寫在 `style.css`，共 `1446` 行，由於行數過多，實際維護困難。

```css
/*
 * Global
 * Menu
 * Icon
 * Header
 * Timeline
 * Main Layout
 * Post Entry
 * Post
 * Post Content: markdown css
 * Footer
 * Page: Archive
 * Page: Tagscloud
 * Page: Search
 * Page: Link
*/
```

其實在 `style.css` 最上方，原作者已經加上註解，我只是根據這些註解拆成獨立 CSS module 而已，然後全部改用 `@import` 。

> Q：使用 `import` 會每個檔案都有 HTTP request，會增加 server loading 嗎 ?

若是一般 CSS，的確會如此沒錯。

但 Suka 使用了 Gulp 處理，最後都會壓縮成為單一 `style.min.css`，因此這裡使用 `@import` 不會影響效能，依然只有一個 HTTP request。

## Device Margin

Suka 是以 mobile first 設計，從其 CSS 也可發現其 defualt value 都是 mobile phone，其次才使用 media query 考慮 tablet 與 desktop。

### Home

![css000](/images/suka/refactor-css/css000.png)

Suka 原本在 home 的上下左右 margin 保留了較多的空白。

![css001](/images/suka/refactor-css/css001.png)

我仍然維持其留白設計，但上下左右 margin 皆減少。

> 這這牽涉到視覺美感，每個人喜好不同，我只是以自己的喜好去調整

**main-layout.css**

```css
/* iPad portrait */
@media only screen and (min-width: 721px) {
  .main-container {
    max-width: 44rem;
  }
}

/* iPhone landscape  */
@media only screen and (min-width: 800px) {
  .main-container {
    max-width: 37.5rem;
  }
}

/* desktop, iPad landscape */
@media only screen and (min-width: 961px) {
  .main-container {
    max-width: 48rem;
  }
}

/* iPad Pro  */
@media only screen and (min-width: 1335px) {
  .main-container {
    max-width: 66rem;
  }
}
```

調整 home 的 margin 主要在 `main-layout.css`。

15 行

```css
/* desktop, iPad landscape */
@media only screen and (min-width: 961px) {
  .main-container {
    max-width: 48rem;
  }
}
```

調整  `.main-container` 的 `max-width`，即可改變左右 margin。

我們發現了 desktop 與 iPad landscape 共用相同的 media query，主要因爲 iPad landscape 為 `1024 x 768`，而 Macbook Pro 15 Retina 最低解析度為 `1024 x 640`，而 home 的 margin 在此剛好 iPad landscape 與 desktop 剛好相同，故可共用。


### Post

![css002](/images/suka/refactor-css/css002.png)

Post 部分 Suka 仍保持左右有較寬的 margin，且右側支援 TOC。

![css003](/images/suka/refactor-css/css003.png)

TOC 是很好的設計，我過去很愛用，但有些限制：

* Heading 若太長，會影響版面呈現
* Heading 層數若過深，也會影響版面呈現

所以過去常常為了畫面美觀，最後放棄 markdown 的 heading 架構，所以這次我沒使用 TOC。

也因為不使用 TOC，因此左右 margin 稍嫌累贅，因此也調整了左右 margin。

> Suka 原本 home 與 post 的 margin 也不相同，因此從 home 切到 post，會有視覺跳動的感覺，也予以修正

**post.css**
```css
/* iPhne portrait */
@media screen and (min-width: 400px) {
  .post-container {
    max-width: 20rem;
  }
}

/* iPad portrait */
@media screen and (min-width: 721px) {
  .post-container {
    max-width: 37.5rem;
  }
}

/* iPhone landscape  */
@media screen and (min-width: 800px) {
  .post-container {
    max-width: 37.5rem;
  }
}

/* desktop, iPad landscape */
@media screen and (min-width: 961px) {
  .post-container {
    max-width: 48rem;
  }
}

/* iPad Pro */
@media screen and (min-width: 1281px) {
  .post-container {
    max-width: 66rem;
  }
}
```

調整 home 的 margin 主要在 `post.css`。

22 行

```css
/* desktop, iPad landscape */
@media screen and (min-width: 961px) {
  .post-container {
    max-width: 48rem;
  }
}
```

調整  `.post-container` 的 `max-width`，即可改變左右 margin。

目前 desktop 與 iPad landscape 依然共用 media query。

## Device Font Size

Suka 對於 mobile phone 的 font size 調整得很好，但 tablet 與 desktop 對我而言，font size 稍微小了些，因此稍作調整。

此次調整 RWD，遇到最大的難點在於 iPad landscape 與 desktop 的 font size 要不一樣，偏偏 iPad landscape 為 `1024 x 768`，而 desktop 為 `1024 x 640`，因此無法單純使用 `min-width` 分辨出 iPad landscape 或 desktop。

`width` 雖然相同，但 `height` 不同，因此直覺會想到改用 `min-height`，但有趣的是：Ｍobile Safari 並不理會 `height`，但 Safari 則會，因此使用了以下 hack：

### Home

**post-entry.css**

```css
/* iPad landscape */
@media only screen and (min-width: 961px) {
  .post-entry a.card-title {
    font-size: 1.1rem;
  }

  .post-entry .card-body {
    font-size: 1.0rem;
  }
}

/* desktop */
@media only screen and (min-width: 961px) and (max-height: 640px) {
  .post-entry a.card-title {
    font-size: 1rem;
  }

  .post-entry .card-body {
    padding-top: .6rem;
    font-size: .8rem;
  }

  .post-entry .post-thumbnail {
    min-height: 6.5rem;
    margin-top: 15px;
  }
}
```

調整 home 的 font-size 主要在 `post-entry.css`。

第 1 行

```css
/* iPad landscape */
@media only screen and (min-width: 961px) {
  .post-entry a.card-title {
    font-size: 1.1rem;
  }

  .post-entry .card-body {
    font-size: 1.0rem;
  }
}
```

既然 Mobile Safari 吃不到 `height`，那 `min-width: 961px` 就讓給 iPad landscape。

12 行

```css
/* desktop */
@media only screen and (min-width: 961px) and (max-height: 640px) {
  .post-entry a.card-title {
    font-size: 1rem;
  }

  .post-entry .card-body {
    padding-top: .6rem;
    font-size: .8rem;
  }

  .post-entry .post-thumbnail {
    min-height: 6.5rem;
    margin-top: 15px;
  }
}
```

因為 desktop 為 `1024 x 640`，所以直覺會設定 `min-height: 640px`，不過實際上 Safari 所傳回的 `height` 會扣掉一些值，反而要設定成 `max-height` 才抓得到。

如此就能 iPad landscape 與 desktop 分別使用不同 media query。

### Post

**post-content.css**

```css
/* iPad portrait */
@media only screen and (min-width: 721px) {
  #post-content {
    font-size: 1.1rem;
  }
}

/* iPhone landscape */
@media only screen and (min-width: 800px) {
  #post-content {
    font-size: 0.95rem;
  }
}

/* iPad landscape */
@media only screen and (min-width: 961px) {
  #post-content {
    font-size: 1.1rem;
  }
}

/* desktop */
@media only screen and (min-width: 961px) and (max-height:640px) {
  #post-content {
    font-size: .85rem;
  }
}
```

調整 post 的 font-size 主要在 `post-content.css`。

15 行

```css
/* iPad landscape */
@media only screen and (min-width: 961px) {
  #post-content {
    font-size: 1.1rem;
  }
}

/* desktop */
@media only screen and (min-width: 961px) and (max-height:640px) {
  #post-content {
    font-size: .85rem;
  }
}
```

依然使用相同技巧分辨 iPad landscape 與 desktop。

### Archive

調整 archive 的 font-size 主要在 `archive.css`。

### Search

調整 archive 的 font-size 主要在 `search.css`。

## Summary

一共調整了以下 CSS：

* `main-layout.css`：home margin
* `post.css`：post margin
* `post-entry.css`：home font size
* `post-content.css`：post font size
* `archive.css`：archieve font size
* `search.css`：search font size

## Conclusion

* CSS 也要使用 module，將來才容易維護
* RWD 的 media query 難在 break point 該如何下，尤其 iPad landscape 與 desktop 的 `width` 相同，透過 `max-height` 技巧，可暫時讓 iPad landscape 與 desktop 使用不同 media query

## Sample Code

完整的範例可以在我的 [GitHub](https://github.com/oomusou/hexo-theme-suka) 上找到

## Reference

[Viz Devices](https://vizdevices.yesviz.com/devices/macbookpro-2018-15/), [Macbook Pro 15" 2018 Spec](https://vizdevices.yesviz.com/devices/macbookpro-2018-15/)
[EmranAhmed](https://gist.github.com/EmranAhmed), [CSS Responsive breakpoint, Media Query break point](https://gist.github.com/EmranAhmed/8044351)

