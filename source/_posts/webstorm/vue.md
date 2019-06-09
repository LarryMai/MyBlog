title: 如何使用 WebStorm 開發 Vue ?
tags:
  - WebStorm
  - Vue
feature: images/feature/webstorm.png
date: 2019-06-06 20:32:25
---
WebStorm 已經提供 Vue 的 plugin 完整支援 Vue，唯一只有 `.vue` 格式縮排部分，Webstorm 與 Vue 的看法迥異，在 Reformat  Code 之後，縮排會完全跑掉，需要特別設定。此外，在 Unit Test 與 Jest 部分，WebStorm 支援度還不夠，但仍可做些調整支援 Vue。

<!-- more -->

## Version

macOS High Sierra 10.13.6
Node.js 8.11.3
Yarn 1.9.4
Vue 2.5.17
WebStorm 2018.2.4

## Vue File

![vue000](/images/webstorm/vue/vue000.png)

由 Vue CLI 所建立的 `App.vue`，我們發現 Vue 的縮排有幾個特點 :

1. 無論是 HTML、JavaScript 或 CSS，其縮排都使用 2 個 space
2. `<script/>` 與 `<style/>` 的第一層不再縮排

![vue001](/images/webstorm/vue/vue001.png)

經過 WebStorm 的 reformat code，縮排就會跑掉。

## Tabs and Indents

### CSS

![vue002](/images/webstorm/vue/vue002.png)

1. ***Preferences -> Editor -> Code -> CSS -> Tabs and Indents***
2. Tab size、Indent 與 Continuation indent 全改成 `2`

### HTML

![vue003](/images/webstorm/vue/vue003.png)

1. ***Preferences -> Editor -> Code -> HTML -> Tabs and Indents***
2. Tab size、Indent 與 Continuation indent 全改成 `2`

### JavaScript

![vue004](/images/webstorm/vue/vue004.png)

1. ***Preferences -> Editor -> Code -> JavaScript -> Tabs and Indents***
2. Tab size、Indent 與 Continuation indent 全改成 `2`

### Space

![vue011](/images/webstorm/vue/vue011.png)

1. ***Preferences -> Editor -> Code Style -> JavaScript -> Spaces***
2. 將 `ES6 import/export braces` 打勾

![vue012](/images/webstorm/vue/vue012.png)

1. ***Preferences -> Editor -> Code Style -> JavaScript -> Spaces***
2. `In function expression` 不要打勾

### Template & Script

![vue005](i/images/webstorm/vue/vue005.png)

1. ***Preferences -> Editor -> Code -> HTML -> Other***
2. 修改 `Do not indent children of:` 

![vue006](/images/webstorm/vue/vue006.png)

1. 新增 `script` 與 `style`

> 也就是 `<script/>` 與 `<style/>` 的第一層不縮排，符合 Vue 的習慣

## CDN

若採用 CDN 方式，則需另外設定。

![vue007](/images/webstorm/vue/vue007.png)

1. WebStorm 提出警告不認識 `Vue`

![vue008](/images/webstorm/vue/vue008.png)

1. 選擇 CDN
2. 按 `Download library`

![vue009](/images/webstorm/vue/vue009.png)

1. `Vue` 的警告就消除了

![vue010](/images/webstorm/vue/vue010.png)

1. 在 ***Preferences -> Language & Frameworks -> JavaScript -> Libraries*** 下
2. 多了 `vue`，這就是剛剛 WebStorm 幫我們下載的

## Unit Test

![vue013](/images/webstorm/vue/vue013.png)

打開 Vue CLI 預設 Unit Test： `example.spec.js`，會發現幾個問題：

* WebStorm 對於 babel-resolver 的 `@` 尚未支援
* WebStorm 也看不懂 Jest 的 `describe()`、`it()` 、`expect()` 與 `toMatch()`，因此也沒有 Intellisense 支援

### Babel-resolver

**import-resolver.js**

```javascript
System.config({
  paths: {
    '@/*': 'src/*',
  },
});
```

![vue014](/images/webstorm/vue/vue014.png)

1. 在 project 根目錄新增 `import-resolver.js`，將以上內容貼上，告訴 WebStorm `@` 相當於 `src` 目錄

![vue015](/images/webstorm/vue/vue015.png)

1. WebStorm 會自動吃到 project 根目錄的 `import-resolver.js`，因此能找到 `HelloWorld.vue`

### TypeScript Jest Definition

![vue016](/images/webstorm/vue/vue016.png)

1. ***Preferences -> Language & Frameworks -> JavaScript -> Libraries***
2. 按 `Download` 下載 Jest TypeScript 定義檔

![vue017](/images/webstorm/vue/vue017.png)

1. 選擇 `Jest`
2. 按 `Download and Install`

![vue018](/images/webstorm/vue/vue018.png)

1. 下載後出現 `@types/jest`

![vue019](/images/webstorm/vue/vue019.png)

* `describe()`、`it()`、`expect()` 與 `toMatch()` 都已經被 WebStorm 識別，也支援 IntelliSense

### Add Jest Package

```
$ yarn add --dev jest
```

![vue020](/images/webstorm/vue/vue020.png)

1. 將 Jest 加入  development dependency

![vue021](/images/webstorm/vue/vue021.png)

1. WebStorm 會認出 `spec.js` 格式為 Jest 的 testing code，可在 editor 內執行 Unit Test

> 雖然已經出現 Icon，但目前還是無法在 WebStorm 內直接執行 Jest，還有些設定要克服

## Conclusion

* 在 JetBrains 其他工具也適用此設定
* WebStorm 目前支援 Vue 良好，除了少部分要調整外，就可完美在 WebStorm 開發 Vue，不用再安裝任何 plugin
* WebStorm 對於 Vue 的 Unit Test 與 Jest 支援還不夠，目前執行 Unit Test 比較適合使用 Vue CLI

## Reference

[@IsaoTakahashi](https://qiita.com/IsaoTakahashi), [WebStormでVue.jsの快適な開発環境をつくる](https://qiita.com/IsaoTakahashi/items/20c82de0ddd2b71f4f75)

