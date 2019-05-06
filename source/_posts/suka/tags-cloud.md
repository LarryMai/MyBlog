title: 如何在 Suka 啟動 Tags Cloud ?
tags:
  - Hexo
  - Suka
feature: images/feature/suka.png
date: 2019-04-30 10:23:43
---
Tags Cloud 為 Blog 常見的功能，Post 越多者字型越大，反之則越小，非常醒目，該如何自行啟動呢 ?

<!-- more -->

## Version

Hexo 3.8.0

## Front-matter

**hello-world.md**

![tags003](/images/suka/tags-cloud/tags003.png)

第 3 行

```yaml
tags:
  - Hexo
  - Suka
```

新增 `Hexo` 與 `Suka` 兩個 tag。

## Enable Tags

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
  rss:
    use: false
  share:
    use: true
  search:
    use: true
    link: /search
```

此為 Suka 的 `_config.yml`，位於 `themes/suka/_config.yml`。

11 行

```yaml
tags:
  use: true
```

預設為 `true`，表示啟動 Tags Cloud 功能，若不想使用，設定為 `false` 即可，menu 則不會出現 `Tags` 。

> 此為自行新增之功能

![tags000](/images/suka/tags-cloud/tags000.png)

## Tags Template

**index.md**

```yaml
---
title: Tags
layout: tags
---
```

在 `blog/source` 新增 `tags` 目錄，並建立 `index.md`。

* `title`：Browser title 所顯示的文字
* `layout`：使用 `tags` layout 顯示

如此 `hexo generate` 就會產生 `tags/index.html`。

![tags001](/images/suka/tags-cloud/tags001.png)

![tags002](/images/suka/tags-cloud/tags002.png)

按下 menu 的 `Tags`，出現 `Suka` 與 `Hexo` 兩個 tag。

## Conclusion

* Suka 預設在 `_config.yml` 沒有 `tags` 設定，`index-nav.ejs` 也沒有支援 Tags，特別自己補上

## Reference

[Suka](https://theme-suka.skk.moe), [獨立頁面](https://theme-suka.skk.moe/docs/pages/)