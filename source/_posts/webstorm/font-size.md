title: 如何調整 WebStorm 字型大小 ?
tags:
  - WebStorm
feature: images/feature/webstorm.png
date: 2019-05-22 13:32:24
---
WebStorm 基於 JavaScript 生態設計，預設組態已經很好用，需要調整的並不多，一開始安裝完 WebStorm，第一個要調整的就是 Font Size。

<!-- more -->

## Version

macOS 10.14.5
ＷebStorm 2019.1.2

## Default Font Size

![fontsize000](/images/webstorm/font-size/fontsize000.png)

WebStorm 預設 font size 較小，且不是根據 macOS 在 system preferences 所設定顯示，必須獨立設定。

## IDE

![fontsize001](/images/webstorm/font-size/fontsize001.png)

***Preferences -> Appearance & Behavior -> Appearance***

設定 IDE 的 font size。

按熱鍵 `⌘ ,` 進入 preferences，在 `Appearance` 下將 font size 調整為 `15`。

## Editor

![fontsize002](/images/webstorm/font-size/fontsize002.png)

***Preferences -> Editor -> Font***

設定 editor 的 font size。

1. **Font**：預設為 `Menlo`，也是很棒的字型，不過由於 ES6 的 arrow function 興起，建議改用 `Fira Code` 系列，較能顯示 FP 特色
2. **Size**：由於是寫 code 主要用字，建議比 IDE 的 font size 略大
3. **Line spacing**：行距，建議可稍微寬一點
4. **Fallback font**：若遇到 `Fira Code` 無法顯示的字，就改用 `Menlo` 顯示
5. **Enable font ligatures**：要打勾，`=>` 與 `!==` 才會以 `Fira Code` 的特殊字型顯示

## Terminal

![fontsize003](/images/webstorm/font-size/fontsize003.png)

***Preferences -> Editor -> Color Scheme -> Console Font***

設定內建 terminal 的 font size。

1. 將 `Use console font instead of the default` 打勾，也就是不使用 editor 的 `Fira Code`
2. 改用一般的 `Menlo`，font size 設定為 `14` 即可

![fontsize004](/images/webstorm/font-size/fontsize004.png)

設定完後，WebStorm 的 font size 就舒服多了。

## Conclusion

* WebStorm 沒有辦法吃 macOS 的 system preferences 較為可惜，必須手動一一設定 font size
* 這不只 WebStorm 適用，其他 JetBrains 產品如 Android Studio、DataGrip 也適用

