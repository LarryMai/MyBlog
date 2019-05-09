title: 如何使 VS Code 支援 CSS Intellisense ?
tags:
  - VS Code
  - Bootstrap
  - CSS
feature: images/feature/vscode.png
date: 2019-05-04 09:23:43
---
VS Code 預設並沒有支援 CSS Class 的 Intellisense，這使得 HTML 在套用 CSS Class 時很不方便，需另外安裝 `HTML CSS Support` Extension。

<!-- more -->

## Version

VS Code 1.33.1
HTML CSS Support 0.2.0
Bootstrap 4.3.1

## HTML CSS Support

![css000](/images/vscode/css-intellisense/css000.png)

另外安裝 `HTML CSS Support`。

## CSS Intellisense

CSS 就使用上，大概有 3 種情境：

1. 使用 local 的 `*.min.css`
2. 使用 local 的 `*.css`
3. 直接使用 CDN 上的 `*.min.css`

對於前兩種狀況，HTML CSS Support 都可以直接抓到 CSS class。

但對於第 3 種方式，就必須額外設定。

## Remote CSS

![css001](/images/vscode/css-intellisense/css001.png)

在 command palette 下，輸入 `json`，選擇 `Preferences: Open Settings (JSON)`。

**settings.json**

```json
"css.remoteStyleSheets": [
  "https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"
]
```

新增 `css.remoteStyleSheets`，在 array 內加入 Bootstrap 的 CDN。

![css003](/images/vscode/css-intellisense/css003.png)

只是以 Bootstrap 為例，其他如 Font Awesome …等的 CDN 也適用。

![css002](/images/vscode/css-intellisense/css002.png)

Bootstrap 的 CSS class 出現了。

## Conclusion

* 加上 HTML CSS Support 之後，就可以在 VS Code 很開心地使用 CSS class

## Reference

[VS Code](https://code.visualstudio.com), [HTML CSS Support](https://marketplace.visualstudio.com/items?itemName=ecmel.vscode-html-css)