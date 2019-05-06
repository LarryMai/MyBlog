title: 如何在 Suka 啟動 Local Search ?
tags:
  - Hexo
  - Suka
feature: images/feature/suka.png
date: 2019-05-02 12:23:43
---
由於 Hexo 是 Static Site Generator，只有 HTML / JavaScript / CSS，並沒有 Backend 技術，過去 Search 必須依賴外部 Search Engine 才能達成，目前 Suka 已經支援 Local Search，在 `hexo generate` 就會產生 `search.json`，可直接使用。

<!-- more -->

## Version

Hexo 3.8.0

## Enable Search

**_config.yml**

```yaml
# ---------------------------------------------------------------
# Menu Settings
# ---------------------------------------------------------------
# Nav settings
# Used to choose which
nav:
  home:
    use: true
  archives:
    use: true
  tags:
    use: true
  share:
    use: false
  search:
    use: true
    link: /search
  rss:
    use: true
```

此為 Suka 的 `_config.yml`，位於 `themes/suka/_config.yml`。

15 行

```yaml
search:
  use: true
  link: /search
```

* `use`：預設為 `true`，表示啟動 Search 功能，若不想使用，設定為 `false` 即可，menu 則不會出現 `Search` 
* `link`：將使用 `/search/index.html` 

![search000](/images/suka/search/search000.png)

## Search Template

**index.md**

```markdown
---
title: Search
layout: search
---
```

在 `blog/source` 新增 `search` 目錄，並建立 `index.md`。

- `title`：Browser title 所顯示的文字
- `layout`：使用 `search` layout 顯示

如此 `hexo generate` 就會產生 `search/index.html`。

![search001](/images/suka/search/search001.png)

![search002](/images/suka/search/search002.png)

按下 menu 的 `Search`，出現 Search 頁面。

## Conclusion

* Suka 內建已完整支援各種 search，不過這屬於 Hexo 部分，所以 Suka 文件也是點到為止，偏偏 Hexo 也沒有說明，須自行整合

## Reference

[Suka](https://theme-suka.skk.moe), [內置插件](https://theme-suka.skk.moe/docs/plugin/)