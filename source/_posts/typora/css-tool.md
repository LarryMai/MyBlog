title: 如何觀察 Typora CSS ?
tags:
  - Typora
  - Safari
  - CSS
feature: images/feature/typora.png
date: 2019-05-23 12:41:34
---
Typora 是使用 Electorn 技術的 macOS App，也因此可自行使用 CSS 開發 Theme，或者利用既有的 Theme 加以微調 CSS，但該如何觀察 CSS 呢 ?

<!-- more -->

## Version

macOS Mojave 10.14.4
Typora 0.9.9.24.2 (2386)
Safari 12.1 (14607.1.40.1.4)

## Safari

一般想到 developer tools，都會想到 Chrome，但可惜 Typora 在 macOS 無法使用 Chrome 觀察 CSS，必須使用 Safari。

![css000](/images/typora/css-tool/css000.png)

***Safari -> Preferences -> Advanced***

將 `Show Develop menu in menu bar` 打勾，如此 menu 才會出現 `develop` 。

![css001](/images/typora/css-tool/css001.png)

***Develop -> [host name] -> typora-app — index.html***

![css002](/images/typora/css-tool/css002.png)

如此 Typora 的 HTML 與 CSS 就完整呈現了，可使用 element selection 工具觀察 HTML element。

## Conclusion

* Typora 在 macOS 只能使用 Safari 觀察，不能使用 Chrome
* Developer tool 一打通，剩下就是 CSS 功力了

## Reference

[Typora](https://typora.io), [Add Custom CSS](http://support.typora.io/Add-Custom-CSS/)

