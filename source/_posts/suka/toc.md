title: 如何在 Suka 不使用 TOC ?
tags:
  - Hexo
  - Suka
feature: images/feature/suka.png
date: 2019-05-03 10:23:43
---
TOC 為 Table of Content 縮寫，Suka 預設會在右側顯示文件的 Outline。TOC 本意很好，但若 Heading 用得過深，或 Title 過長，其實並不美觀，這也限制了文件的架構設計，因此可能會想將此功能關閉。

<!-- more -->

## Version

Hexo 3.8.0

## Suka

Suka 支援兩種方式關閉 TOC：

* **_config.yml**
* **Front-matter**

## config.yml

```yaml
# Table Of Contents which will be show on right side of the post
toc:
  # You can still disable toc for each single post by setting their front-matter with 'toc: false'
  enable: false
  # Show line number for each toc list-item or not. Default is true.
  line_number: true
```

在 Suka 的 `_config.yml` (`themes/suak/_config.yml`)，在 `toc` 下，將 `enable` 設定為 `false`。

> 這種方式將以 global 關閉 TOC，也就是所有 post 都不支援 TOC

![toc000](/images/suka/toc/toc000.png)

## Front-matter

```yaml
toc: false
---
```

在每篇 post 的 front-matter 加上 `toc: false`，則只有該 post 不使用 TOC。

![toc001](/images/suka/toc/toc001.png)

## Conclusion

* TOC 是很好的設計，不過要版型搭配，目前我是使用 `_config.yml` 全域關閉 TOC

## Reference

[Suka](https://theme-suka.skk.moe), [主題配置](https://theme-suka.skk.moe/docs/config/)
[Suka](https://theme-suka.skk.moe), [開始創作](https://theme-suka.skk.moe/docs/compose/)