title: 如何在 Suka 啟動 RSS ?
tags:
  - Hexo
  - Suka
feature: images/feature/suka.png
date: 2019-05-01 09:23:43
---
RSS 為 Blog 常見的功能，讓讀者只要透過 RSS Reader，就能追蹤最新文章，非常方便，該如何自行啟動呢 ?

<!-- more -->

## Version

Node 10.15.3
Hexo 3.8.0

## Hexo-generator-feed

```
$ yarn add hexo-generator-feed
```

安裝 `hexo-generator-feed`，Hexo 將會自動產生 RSS 所需要的 `atom.xml`。

![rss000](/images/suka/rss/rss000.png)

## Enable RSS

**_config.yml**

```ymal
rss:
  use: true
```

Theme 的 `_config.yml`，預設為 `false`，須自行改為 `true` 才會在 menu 顯示 RSS。

![rss001](/images/suka/rss/rss001.png)

**_config.yml**

```yaml
## RSS
feed:
  type: atom
  path: atom.xml
  limit: 20
  hub:
  content:
  content_limit: 140
  content_limit_delim: ' '
  order_by: -date
  icon: icon.png
```

在 blog 的 `_config.yml` 最下方貼上以上設定。

![rss002](/images/suka/rss/rss002.png)

![rss003](/images/suka/rss/rss003.png)

Menu 出現 `RSS`。

![rss004](/images/suka/rss/rss004.png)

按下 `RSS`，顯示 `atom.xml`。

## Conclusion

* RSS 其實屬於 Hexo 的功能，不過由於是另外以 `Hexo-generator-feed` 實現，所以 Hexo 官網也沒有說明，Suka  更沒有說明，須自行整合

## Reference

[Hexo](https://github.com/hexojs), [hexes/hexo-generator-feed](hexes/hexo-generator-feed)