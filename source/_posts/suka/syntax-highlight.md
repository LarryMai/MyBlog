title: 如何在 Suka 設定 Syntax Highlight ?
tags:
  - Hexo
  - Suka
feature: images/feature/suka.png
date: 2019-05-03 11:23:43
---
由於是討論程式技術的 Blog，因此 Syntax Highlight 就非常重要，除了 Language 支援要多，Keyword 判斷要正確，背景為黑底更是基本需求。

<!-- more -->

## Version

Hexo 3.8.0

## Prism

Suka 支援 [Highlight.js](https://highlightjs.org)、[Prism](https://prismjs.com/index.html) 與 [Hanabi](https://github.com/egoist/hanabi) 三種 syntax highlight，經過實測，若要求黑底，Highlight.js 有些語法顏色會錯，Prism 則正常，因此本文以 Prism 為主。

**_config.yml**

```yaml
highlight:
  enable: false
  line_number: false
  auto_detect: false
  tab_replace: false
  
suka_theme:
  prism:
    enable: true
    line_number: true
    theme: darcula   
```

設定 blog 根目錄 `_config.yml`。

將原本 `highlight` 全部設定為 `false`，避免與 Prism 相衝。

將 `prism` 的 `enable` 設定為 `true`，並設定所使用的 `theme`。

```
a11y-dark
atom-dark
base16-ateliersulphurpool.light
cb
coy
darcula
dark
default
duotone-dark
duotone-earth
duotone-forest
duotone-light
duotone-sea
duotone-space
funky
ghcolors
hopscotch
okaidia
pojoaque
solarizedlight
tomorrow
twilight
vs
xonokai
```

根據 Suka 文件，支援以上 24 種 Prism theme。

![prism000](/images/suka/syntax-highlight/prism000.png)

![prism001](/images/suka/syntax-highlight/prism001.png)

```
$ hexo clean
$ hexo server
```

Suka 要求每次只要更改 syntax highlight theme，就一定要 `hexo clean` 重新來過。

![prism002](/images/suka/syntax-highlight/prism002.png)

![prism003](/images/suka/syntax-highlight/prism003.png)

順利使用黑底的 Darcula theme。

## Conclusion

* 感謝 Suka 已經整理好各種 syntax highlight theme，可輕易更改自己喜歡的配色

## Reference

[Suka](https://theme-suka.skk.moe), [內置插件](https://theme-suka.skk.moe/docs/plugin/)
[Highlight.js](https://highlightjs.org)
[Prism](https://prismjs.com/index.html) 
[Hanabi](https://github.com/egoist/hanabi)