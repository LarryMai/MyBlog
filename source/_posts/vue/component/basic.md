title: Vue Component 基礎概念
tags:
  - Vue
feature: images/feature/vue.png
date: 2019-06-08 14:24:07
---
Component 概念為 React 所發明，讓我們可以將 HTML、CSS 與 JavaScript 封裝成 component，Vue、Angular 也使用 Component ，至此 3 大 Framework 都採用 Component-based 架構。

<!-- more -->

## Version

Vue 2.6.10

## Introduction

Vue instance 有自己的 `data`、`methods`、`computed` 、`watch` property，但 Vue instance 只是 HTML 的代言人，讓我們在 JavaScript 控制 HTML，但若要 `重複使用`，就不是那麼方便，這時我們需要的是 Vue component。

除此之外，Vue component 也讓我們在開發時實踐 `Divide and Conquer` 哲學，先將需求切成小小 component，然後各自擊破，最後再將 component 組合起來，如此 component 也更加 `單一職責`，更 `容易維護` 與 `重複使用`。

## Simple Component

之前我們只會 MVVM 的 Hello World，若改用 component 寫法呢 ?

**App.vue**

```html
<template>
  <hello-world></hello-world>
</template>

<script>
import helloWorld from './components/hello-world.vue';

export default {
  name: 'app',
  components: {
    helloWorld
  }
};
</script>
```

我們發現 `App.vue` 分兩區，`<template/>` 負責 HTML，而 `<script/>` 則負責 JavaScript。

第 1 行

```html
<template>
  <hello-world></hello-world>
</template>
```

由原本的 `<span>Hello World</span>`，變成自訂的 `hello-world` tag。

> 自訂的 HTML tag 名稱，無論使用 camelCase，CamelCase，最後 Vue 都會改用 kebab-case (全小寫，單字間以 `-` 隔開)，這是 W3C 所建議，且必須是 2 個單字，避免用一個單字與 HTML 預設 tag 重複

第 6 行

```javascript
import helloWorld from './components/hello-world.vue';
```

將 `hello-world` component import 進來，使用 JavaScript 的 camelCase。

10 行

```javascript
components: {
  helloWorld
}
```

將剛剛 import 進的 `helloWorld` 宣告在 `components` property 下。

**hello-world.vue**

```html
<template>
  <span>Hello World</span>
</template>

<script>
export default {
  name: 'hello-world',
};
</script>
```

第 1 行

```html
<template>
  <span>Hello World</span>
</template>
```

將原本的 `<span>Hello World</span>` 搬到 `hello-world.vue` 的 HTML template 內。

第 5 行

```html
<script>
export default {
  name: 'hello-world',
};
</script>
```

在 `<script/>` 寫 JavaScript。

第 6 行

```javascript
export default {
```

ES6 稱為 default export，一個 module 內只能有一個 function 或一個 object 使用 default export，其他都必須使用 named export。

> 寫 comport 時，Vue 習慣將 comport 使用 default export，由使用端自行對 object 取名

第 7 行

```javascript
name: 'hello-world',
```

關於 component (HTML tag) 與 JavaScript 檔案命名方式，Vue 官方的 [Style Guide](https://vuejs.org/v2/style-guide/#Single-file-component-filename-casing-strongly-recommended) 建議：

* **CamelCase**：`HelloWorld`、`HelloWorld.js`、`HelloWorld.vue`
* **kebab-case**：`hello-world`、`hello-world.js`、`hello-world.vue`

> 建議 component 與 JavaScript 檔案名稱使用 kebab-case，可避免 Git 不分大小寫而誤判
>
> 建議 import component 名稱使用 camelCase，符合 ECMAScript 習慣
>
> 建議 component 名稱使用 kebab-case，如此與 W3C 一致


## MVVM vs. Component

目前 Vue Component 的 data 都是寫死的，我們知道 MVVM 的精髓就是 data binding，要如何將 MVVM 與 component 兩種架構合而為一呢 ?

**App.vue**

```html
<template>
  <div>
    <my-counter></my-counter>
    <p></p>
    <my-counter></my-counter>
  </div>
</template>

<script>
import myCounter from './components/my-counter.vue';

export default {
  name: 'app',
  components: {
    myCounter
  }
};
</script>
```

第 3 行

```html
<my-counter></my-counter>
```

使用自訂的 `<my-counter></my-counter>`。

第 10 行

```javascript
import myCounter from './components/my-counter.vue';

export default {
  name: 'app',
  components: {
    myCounter
  }
};
```

import 進 `myCounter`，並在 `components` property 內宣告了 `myCounter`。

**my-counter.vue**

```html
<template>
  <div>
    <span>{{ counter }}</span>
    <p></p>
    <button @click="add">+1</button>
  </div>
</template>

<script>
let add = function() {
  this.counter++;
};

export default {
  name: 'my-counter',
  data: () => ({ counter: 0}),
  methods: { add }
};
</script>
```

16 行

```javascript
data: () => ({ counter: 0 }),
```

`data` 部分，由原本 Vue instance 的 `data` property 改成 `data()` function，回傳 `data` object。

這裡有 3 種寫法：

* OOP method
* Function expression
* Arrow function

```javascript
data() {
  return {
    counter: 0
  }
}
```

使用 ES6 類似 OOP method 寫法。

```javascript
data: function() {
  return {
    counter: 0
  }
}
```

使用 ES5 的 function expression 寫法。

```javascript
data: () => ({ counter : 0 })
```

使用 ES6 的 arrow function。

因為 object 與 arrow function 的 body 都使用 `{}`，所以 ES6 特別規定當 arrow function 回傳 object 時，需在 `{}` 外加上 `()` 識別。

個人較喜歡 ES6 的 arrow function，因為簡潔沒有贅字，此部分可依個人品味決定。

> Q : 為什麼寫成 Vue component 後，要從 `data` property 改成 `data()` function ?

```javascript
data: {
  counter: 0
},
```

若改成 `data` property 寫法，Vue 會無法執行，且出現 warning。

![basic001](/images/vue/component/basic/basic001.png)

![basic002](/images/vue/component/basic/basic002.svg)

在正統 OOP，兩個 component 應該是兩個 instance，而 `data` 包在 instance 內，因此 component 間的 `data` 不會互相影響，也就是 OOP 的 `封裝`。

![basic003](/images/vue/component/basic/basic003.svg)

但 Vue 底層並不是採用 OOP 方式，而是共用同一份 component instance，只有 `data` 是不同份。

這也是為什麼為什麼 Vue 要你改用 `data()` function，而且是回傳全新對 `data` object。

> 只要寫 Vue component，就一定要改用 `data()` function，不能使用 `data` property
> 

17 行

```javascript
methods: { add }
```

在 `methos` property 內宣告 `add` method。

> Q：為什麼 object 內只有 `template`，不是應該 key / value 嗎 ?

```javascript
let name = 'Sam';

let student = {
  name: name
};

student.name; // ?
```

在 ES5，若要將變數指定給 property，儘管是同名，一樣得使用 `key: value` 格式，很明顯的重複。

```javascript
let name = 'Sam';

let student = {
  name
};

student.name; // ?
```

在 ES6，若 property 的 key 與 value 的變數同名，可只寫 key 即可，稱為 property shorthand。

10 行

```javascript
let add = function() {
  this.counter++;
};
```

定義 `add()` 實現 `counter` 累加。

> Q：為什麼這裡又要用 function expression 而不用 arrow function 呢 ?

因為這裡使用了 `this` 存取 `data` property，若使用 arrow function 會導致 `this` context 不是指向 component，因此不能使用 arrow function。

> 若 function 需要使用 `this` context，則必須使用 function expression
>
> 若 function 為 pure function，則可使用 arrow function

如之前的 data function 為 pure function，因此可用 arrow function。

> Q：為什麼不將 method 寫在 constructor object 內，而要單獨抽出 function 呢 ?

主要為了兩個理由：

* 避免 Vue 巢狀過深
* 凸顯 JavaScript 的 Functional 特色

### 避免 Vue 巢狀過深

Vue 最為人詬病的，就是將 method 寫在 constructor object 內時，還沒寫 code 已經縮排三層，導致真的寫 production code 時，很容易看到縮排到 5 層以上，導致日後難以維護，因次特別使用 ES6 的 property shorthand，將 method 放到第一層，避免傳統 Vue 寫法巢狀過深。

### 凸顯 JavaScript 的 Functional 本色

由於傳統 Vue 使用了 ES6 的 method 語法，搭配了 `this` 後很像 OOP，因此讓很多初學者忘了 JavaScript 的 functional 本色，事實上 Vue 的 `data`、`method`、`computed`、`watch`、`hook` 本質都是 function，既然是 function 就可搭配 higher order function / closure 產生，這是 OOP method 寫法所無法達成的。

> 使用 Vue component 時，還有一點值得注意 !

* 不可使用 self closing 語法

```html
<div>
  <my-counter/>
  <my-counter/>
</div>
```

這種寫法，Vue 不會出錯，但只有一個 component 能動。

```html
<div>
  <my-counter></my-counter>
  <my-counter></my-counter>
</div>
```

要這樣寫，Vue 才能正常執行。

## Dynamic Component

先定義好 Vue component，然後動態切換 component。

**App.vue**

```html
<template>
  <div>
    <button @click="onSelectLesson">Lessons</button>
    <button @click="onSelectApply">Apply</button>
    <p></p>
    <component :is="content"></component>
  </div>
</template>

<script>
import myLessons from './components/my-lessons.vue';
import myApply from './components/my-apply.vue';

let onSelectLesson = function() {
  this.content = 'my-lessons';
};

let onSelectApply = function() {
  this.content = 'my-apply';
};

export default {
  name: 'app',
  components: {
    myLessons,
    myApply
  },
  data: () => ({ content: 'my-lessons' }),
  methods: {
    onSelectLesson,
    onSelectApply,
  }
};
</script>
```

11 行

```javascript
import myLessons from './components/my-lessons.vue';
import myApply from './components/my-apply.vue';
```

一樣要使用 import 將要切換的 component import 進來。

24 行

```javascript
components: {
  myLessons,
  myApply
},
```

在 `components` property 內宣告要使用的 component。

第 6 行

```html
<component :is="content"></component>
```

使用 Vue 自行擴充的 `<component></component>`，綁定其 `is`，當 `content` data 指定什麼 component 時，`<component></component>` 就會動態切換該 component。

> 並沒有在 HTML 內事先使用特定 component tag，只使用 `<component></component>` 作為 place holder 保留其動態彈性

28 行

```javascript
data: () => ({ content: 'my-lessons' }),
```

在 `data` 內定義 `content`，其預設值 `my-lessons` 為預設 component 名稱。

14 行

```javascript
let onSelectLession = function() {
  this.content = 'my-lessons';
};

let onSelectApply = function() {
  this.content = 'my-apply';
};
```

由 function 動態改變 `content`，MVVM 會再動態改變 `<component></component>` 的 `:is`，達到動態組件的需求。

這裡因為要使用 `this` 修改 `content`，所以只能使用 function expression，不能使用 arrow function。

> Event handler 建議使用 `on` 為 prefix，凸顯其為 event handler，而非普通 function

**my-lessions.vue**

```html
<template>
  <ul>
    <li>React</li>
    <li>Angular</li>
    <li>Vue</li>
  </ul>
</template>

<script>
export default {
  name: 'my-lessons',
};
</script>
```

定義了 `my-lessions` component。

**my-apply.vue**

```html
<template>
  <form>
    <textarea></textarea>
    <p></p>
    <button>Submit</button>
  </form>
</template>

<script>
export default {
  name: "my-apply"
};
</script>
```

定義了 `my-apply` component。

> 但這兩個 Vue component 都沒在 HTML 內被使用，將由 JavaScript 動態指定

## Keep-Alive

由於 `<component></component>` 類似 `v-if`，其 dynamic component 是藉由 `刪除 DOM element，並建立新的 DOM element` 的方式，所以原本的 user 輸入的資料，也會一併被刪除。

若要保留原本 user 輸入的資料，就必須搭配 `<keep-alive><keep-alive>`。

**App.vue**

```html
<template>
  <div>
    <button @click="onSelectLesson">Lessons</button>
    <button @click="onSelectApply">Apply</button>
    <p></p>
    <keep-alive>
      <component :is="content"></component>
    </keep-alive>
  </div>
</template>

<script>
import myLessons from './components/my-lessons.vue';
import myApply from './components/my-apply.vue';

let onSelectLesson = function() {
  this.content = 'my-lessons';
};

let onSelectApply = function() {
  this.content = 'my-apply';
};

export default {
  name: 'app',
  components: {
    myLessons,
    myApply
  },
  data: () => ({ content: 'my-lessons' }),
  methods: {
    onSelectLesson,
    onSelectApply,
  }
};
</script>
```

第 6 行

```html
<keep-alive>
  <component :is="content"></component>
</keep-alive>
```

在 `<component></component>` 外部加上 `<keep-alive></keep-alive>`，則 dynamic component 內的資料將獲得保留。

JavaScript 的寫法不用改變。

> Vue 底層會將 user 的輸入保留，然後切換 component 時，除了建立新的 component 外，還會將 user 原本所輸入的資料也 `重新` 填回新建立的 component，讓動態切換 component 更方便

## Conclusion

* Vue 提供了 Vue component 與 Vue file，讓我們將 HTML、CSS 與 JavaScript 使用 component 包起來，方便閱讀，也更容易維護
* MVVM 可以與 component 完美結合，但 `data` property 必須改用 `data` function
* Vue 並非不能使用 arrow function，只要能分辨何時可用即可
* Dynamic component 讓我們可以根據商業邏輯自行切換 component
* 透過神奇的 `<keep-alive></keep-alive>`，user 原本的輸入將保留在 component 內

## Sample Code

完整範例可以在我的 [GitHub](https://github.com/oomusou/vue-component) 上找到

## Reference

[Vue](https://vuejs.org/), [Component Basics](https://vuejs.org/v2/guide/components.html)
[Vue](https://vuejs.org/), [Style Guide](https://vuejs.org/v2/style-guide/#Base-component-names-strongly-recommended)

