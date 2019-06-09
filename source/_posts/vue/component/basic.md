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

## Global Component

之前我們只會 MVVM 的 Hello World，若改用 component 寫法呢 ?

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Global Component</title>
</head>
<body>
  <div id="app">
    <hello-world></hello-world>
  </div>
</body>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="hello-world.js"></script>
<script src="index.js"></script>
</html>
```

第 9 行

```html
<hello-world></hello-world>
```

由原本的 `<span>Hello World</span>`，變成自訂的 `hello-world` tag。

> 我們會為自己的的 Vue Component 定義自己的 HTML tag

**index.js**

```javascript
new Vue({
  el: '#app'
});
```

建立 Vue Instance。

**hello-world.js**

```javascript
Vue.component('hello-world', {
  template: '<span>Hello World</span>',
});
```

使用 `Vue.component()` 定義 Vue component。

* 第 1 個參數為 string，傳入自訂的 HTML tag 名稱
* 第 2 個參數為 object，稱為 constructor object，傳入 component 所需要的 property

Vue Component 的使用，有幾點要注意：

* Vue 規定 Vue component 一定要定義在 Vue instance `之前`，否則 Life Cycle 在 `compile HTML` 階段，會不知道 Vue component 所自訂的 HTML tag
* 自訂的 HTML tag 名稱，無論使用 camelCase，CamelCase，最後 Vue 都會改用 kebab-case (全小寫，單字間以 `-` 隔開)，這是 W3C 所建議，且必須是 2 個單字，避免用一個單字與 HTML 預設 tag 重複
* 關於 component (HTML tag) 與 JavaScript 檔案命名方式，Vue 官方的 [Style Guide](https://vuejs.org/v2/style-guide/#Single-file-component-filename-casing-strongly-recommended) 建議:
   * **CamelCase**：`HelloWorld`、`HelloWorld.js`、`HelloWorld.vue`
   * **kebab-case**：`hello-world`、`hello-world.js`、`hello-world.vue`
   * Vue CLI 使用 **CamelCase**

> 建議 component 與 JavaScript 檔案名稱使用 kebab-case，可避免 Git 不分大小寫而誤判
>
> 建議 component 命名也使用 kebab-case，如此與 W3C 一致

## Local Component

使用 `Vue.component()` 所宣告的是 global component，也就是每個 Vue instance 都可使用，若你想定義只有某個 Vue instance 能使用的 component，則要使用 local component。

**inex.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Local Component</title>
</head>
<body>
  <div id="app">
    <hello-world></hello-world>
  </div>
</body>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="index.js"></script>
</html>
```

HTML 部分一樣不變使用 `<hello-world></hello-world>`。

**index.js**

```javascript
let template = '<span>Hello World</span>';

new Vue({
  el: '#app',
  components: {
    'hello-world': {
      template,
    }
  }
});
```

第 3 行

```javascript
new Vue({
  el: '#app',
  components: {
    'hello-world': {
      template,
    }
  }
});
```

在傳入 Vue instance 的 constructor 參數內，加上 `components` Property，為 object。

以自訂的 HTML tag `hello-world` 為 key，定義 component。

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

第 1 行

```javascript
let template = '<span>Hello World</span>';
```

定義 HTML template。

## MVVM vs. Component

目前 Vue Component 的 data 都是寫死的，我們知道 MVVM 的精髓就是 data binding，要如何將 MVVM 與 component 兩種架構合而為一呢 ?

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Counter Component</title>
</head>
<body>
  <div id="app">
    <my-counter></my-counter>
    <my-counter></my-counter>
  </div>
</body>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="my-counter.js"></script>
<script src="index.js"></script>
</html>
```

第 9 行

```html
<my-counter></my-counter>
```

使用自訂的 `<my-counter></my-counter>`。

**index.js**

```javascript
new Vue({
  el: '#app'
});
```

建立 Vue Instance。

**my-component.js**

```javascript
let template = `
  <div>
    <span>{{ counter }}</span>
    <p></p>
    <button @click="add">+1</button>
  </div>
`;

let add = function() {
  this.counter++;
};

Vue.component('my-counter', {
  template,
  data: () => ({ counter: 0 }),
  methods: {
    add,
  }
});
```

13 行

```javascript
Vue.component('my-counter', {
  template,
  data: () => ({ counter: 0 }),
  methods: {
    add,
  }
});
```

宣告 `my-counter` component，costructor object 則有 `template`、`data` 與 `methods` property。


第 1 行

```javascript
let template = `
  <div>
    <span>{{ counter }}</span>
    <p></p>
    <button @click="add">+1</button>
  </div>
`;
```

使用 ES6 的 string template 定義 `template`。

> 由於 HTML template 實務上會很多行，用普通字串不方便，建議改用 ES6 的 string template，就不必再 `字串相加` 了

15 行

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

類似 OOP method 寫法。

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

因為 object 與 arrow function 的 body 的使用 `{}`，所以 ES6 規定當 arrow function 回傳 object 時，需在 `{}` 外加上 `()` 識別。

個人較喜歡 ES6 的 arrow function，因為簡潔沒有贅字，此部分可依個人品味決定。

> Q : 為什麼寫成 Vue Component 後，要從 `data` property 改成 `data()` function ?

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

第 9 行

```javascript
let add = function() {
  this.counter++;
};
```

對 `data` 內的 `counter` 累加。

> Q：為什麼這裡又要用 function expression 而不用 arrow function 呢 ?

因為這裡使用了 `this` 存取 `data` property，若使用 arrow function 會導致 `this` context 不是指向 component，因此不能使用 arrow function。

> 若 function 需要使用 `this` context，則必須使用 function expression
>
> 若 function 為 pure function，則可使用 arrow function

如之前的 data function 為 pure function，因此可用 arrow function。

> 使用 Vue component 時，還有一點值得注意 !

* 不可使用 self closing 語法

```html
<div id="app">
  <my-counter/>
  <my-counter/>
</div>
```

這種寫法，Vue 不會出錯，但只有一個 component 能動。

```html
<div id="app">
  <my-counter></my-counter>
  <my-counter></my-counter>
</div>
```

要這樣寫，Vue 才能正常執行。


## Component vs. DOM Parser

有時候在使用 Vue component 時，會發現無法如預期顯示在 browser 裏。

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>DOM Parse Error</title>
</head>
<body>
  <div id="app">
    <select>
      <my-option></my-option>
    </select>
  </div>
</body>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="index.js"></script>
<script src="my-option.js"></script>
</html>
```

第 9 行

```html
<select>
  <my-option></my-option>
</select>
```

在 `<select></select>` 內使用自訂的 `<my-option></my-option>` Vue component。

**index.js**

```javascript
new Vue({
  el: '#app'
});
```

建立 Vue Instance。

**my-option.js**

```javascript
let template = '<option>Vue</option>';

Vue.component('my-option', {
  template,
});
```

自訂的 `my-option` 只包含 `<option>Vue</option>` 部分。

![basic004](/images/vue/component/basic/basic004.png)

1. Chrome 無法正常顯示
2. `<select></select>` 以下沒有任何 `<option></option>`

這牽涉到各 browser 的 DOM parser 如何解析 HTML。

以 Chrome 而言，它認為 `<select></select>` 下 `只應該` 是 `<option></option>`，其他的 HTML tag 都為非法，因此忽略不使用。

> DOM Parser 會因 Browser 而異

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>DOM Parse OK</title>
</head>
<body>
  <div id="app">
    <my-select></my-select>
  </div>
</body>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="my-select.js"></script>
<script src="index.js"></script>
</html>
```

由本來的 `<my-option></my-option>` 改成 `<my-select></my-select>`。

**index.js**

```javascript
new Vue({
  el: '#app'
});
```

建立 Vue instance。

**my-select.js**

```javascript
let template = `
  <select>
    <option>Vue</option>    
  </select>
`;

Vue.component('my-select', {
  template,
});
```

連 `<select>` 一起包進 Vue component，如此 Chrome 就無法干涉 `<option>`。

## Dynamic Component

先定義好 Vue component，然後動態切換 component。

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Dynamic Component</title>
</head>
<body>
  <div id="app">
    <button @click="onSelectLesson">Lessons</button>
    <button @click="onSelectApply">Apply</button>
    <p></p>
    <component :is="content"></component>
  </div>
</body>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="my-lessons.js"></script>
<script src="my-apply.js"></script>
<script src="index.js"></script>
</html>
```

12 行

```html
<component :is="content"></component>
```

使用 Vue 擴充的 `<component></component>`，綁定其 `is`，當 `content` 指定什麼 component 時，`<component></component>` 就會動態切換該 component。

> 並沒有在 HTML 內事先使用特定 component tag，只使用 `<component></component>`做為 place holder 保留其動態彈性

**index.js**

```javascript
let onSelectLession = function() {
  this.content = 'my-lessons';
};

let onSelectApply = function() {
  this.content = 'my-apply';
};

new Vue({
  el: '#app',
  data: () => ({ content: 'my-lessons' }),
  methods: {
    onSelectLesson,
    onSelectApply,
  }
});
```

11 行

```javascript
data: () => ({ content: 'my-lessons' }),
```

在 `data` 內定義 `content` model，其中預設值 `my-lessons` 為 component 名稱。

第 1 行

```javascript
let onSelectLession = function() {
  this.content = 'my-lessons';
};

let onSelectApply = function() {
  this.content = 'my-apply';
};
```

由 function 動態改變 `content`，MVVM 會再動態改變 `<component></component>` 的 `:is`，達到動態組件的需求。

**my-lessions.js**

```javascript
let template = `
  <ul>
    <li>React</li>
    <li>Angular</li>
    <li>Vue</li>
  </ul>
`;

Vue.component('my-lessons', {
  template
});
```

定義了 `my-lessions` component。

**my-apply.js**

```javascript
let template = `
  <form>
    <textarea></textarea>
    <p></p>
    <button>Submit</button>
  </form>
`;

Vue.component('my-apply', {
  template,
});
```

定義了 `my-apply` component。

> 但這兩個 Vue component 都沒在 HTML 內被使用，將由 JavaScript 動態指定

## Keep-Alive

由於 `<component></component>` 類似 `v-if`，其 dynamic component 是藉由 `刪除 DOM element，並建立新的 DOM element` 的方式，所以原本的 user 輸入的資料，也會一併被刪除。

若要保留原本 user 輸入的資料，就必須搭配 `<keep-alive><keep-alive>`。

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Keep Alive</title>
</head>
<body>
<div id="app">
  <button @click="selectLesson">Lessons</button>
  <button @click="selectApply">Apply</button>
  <p></p>
  <keep-alive>
    <component :is="content"></component>
  </keep-alive>
</div>
</body>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="my-lessons.js"></script>
<script src="my-apply.js"></script>
<script src="index.js"></script>
</html>
```

12 行

```html
<keep-alive>
  <component :is="content"></component>
</keep-alive>
```

在 `<component></component>` 外部加上 `<keep-alive></keep-alive>`，則 dynamic component 內的資料將獲得保留。

JavaScript 的寫法不用改變。

> Vue 底層會將 user 的輸入保留，然後切換 component 時，除了建立新的 component 外，還會將 user 原本所輸入的資料也 `重新` 填回新建立的 component，讓動態切換 component 更方便

## Conclusion

* Vue 提供了 Vue component，讓我們將 HTML、CSS 與 JavaScript 使用 component 包起來，方便閱讀，也更容易維護
* MVVM 可以與 component 完美結合，但 `data` property 必須改用 `data` function
* Vue 並非不能使用 arrow function，只要能分辨何時可用即可
* Vue component 有時候會違背 browser 的 DOM Parser，此時必須改變寫法繞過 browser
* Dynamic component 讓我們可以根據商業邏輯自行切換 component
* 透過神奇的 `<keep-alive></keep-alive>`，user 原本的輸入將保留在 component 內

## Reference

[Vue](https://vuejs.org/), [Component Basics](https://vuejs.org/v2/guide/components.html)
[Vue](https://vuejs.org/), [Style Guide](https://vuejs.org/v2/style-guide/#Base-component-names-strongly-recommended)

