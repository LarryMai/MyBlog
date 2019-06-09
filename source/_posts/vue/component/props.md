title: Vue Component 之 Props
tags:
  - Vue
feature: images/feature/vue.png
date: 2019-06-09 22:16:37
---
Component 若只有一層，那問題不大，若牽涉到 Component 包含 Component，就會牽涉到一個基本問題：該如何將 Data 由外層 Component 傳給內層 Component ? 這時候就要使用 Vue Component 的 Props，這也是 Vue 從 React 學來的部分。

<!-- more -->

## Version

Vue 2.6.10

## Static Props

一樣是老梗 Hello World，但這次 data 並不是在 Vue instance 內定義，而是由外層傳進來。

> 將 `靜態資料` 由 prop 傳進 component

**App.vue**

```html
<template>
  <hello-world greeting="Hello" user-name="World"></hello-world>
</template>

<script>
import helloWorld from './components/hello-world.vue';

export default {
  name: 'app',
  components: { helloWorld }
};
</script>
```

第 2 行

```html
<hello-world greeting="Hello" user-name="World"></hello-world>
```

一樣使用 `hello-world` component，但這次透過自訂的 `greeting` 與 `user-name` props 將 data 由 component 外層傳進 component。 

第 6 行

```javascript
import helloWorld from './components/hello-world.vue';
```

將 `hello-world` component import 進來，使用 JavaScript 的 camelCase。

10 行

```javascript
components: { helloWorld }
```

剛剛 import 進的 `helloWorld` 宣告在 `components` property 下。

**hello-world.vue**

```html
<template>
  <span>{{ greeting }} {{ userName }}</span>
</template>

<script>
export default {
  name: 'hello-world',
  props: [
    'greeting',
    'userName'
  ],
}
</script>
```

第 8 行

```javascript
props: [
  'greeting',
  'userName',
],
```

若要由 component 的外層傳 data 進來，則要在 Vue component 使用 `props` property。

`props` 為 array，每個 prop 使用 string 定義。

第 1 行

```html
<template>
  <span>{{ greeting }} {{ userName }}</span>
</template>
```

在 `props` 宣告過 `greeting` 與 `userName` prop 之後，就可在 HTML template 如同使用 `data` 一般。

> HTML template 能綁定的資料，除了能使用 `data`、`computed` 外，已可以直接使用 `props`

```html
<hello-world greeting="Hello" user-name="World"></hello-world>
```

在 props 使用上，有一點要注意：

* props 在 JavaScript 雖然宣告成 camelCase，在使用端需改成 kebab-case 才能使用 (W3C 建議)


## Dynamic Props

之前傳進 props 的值是寫死的，能夠動態將 `data` 與 `props` 結合嗎 ?

**App.vue**

```html
<template>
  <ul>
    <hello-world v-for="(userName, index) in userNames" greeting="Hello" :user-name="userName" :key="index"></hello-world>
  </ul>
</template>

<script>
import helloWorld from './components/hello-world.vue';

export default {
  name: 'app',
  components: { helloWorld },
  data: () => ({
    userNames: [
      'Sam',
      'Kevin',
      'John'
    ]
  }),
};
</script>
```

第 3 行

```html
<ul>
  <hello-world v-for="(userName, index) in userNames" greeting="Hello" :user-name="userName" :key="index"></hello-world>
</ul>
```

想將 `userName` 綁定到 `user-name` props，必須使用 `:` attribute binding。

而 `userName` 則由 `v-for` 來自於 data 的 `userNames`。

13 行

```javascript
data: () => ({
  userNames: [
    'Sam',
    'Kevin',
    'John'
  ]
}),
```

`userNames` 在 `data` 內定義。

**hello-world.vue**

```html
<template>
  <li>
    <span>{{ greeting }} {{ userName }}</span>
  </li>
</template>

<script>
export default {
  name: 'hello-world',
  props: [
    'greeting',
    'userName'
  ],
}
</script>
```

與 static props 的 `hello-world.vue` 完全一樣。

> 透過 dynamic props，我們就能將 component 外層的 data 透過 props 傳進 component，但必須使用 attribute binding

## Unidirectional Dataflow

Vue props 是 unidirectional dataflow，也就是 data 只能從 component 外層傳進 component，而無法由內層 component 傳到外層 component。

> 若要將 data 從內層 component 傳到外層 component，則要使用 event

**App.vue**

```html
<template>
  <div id="app">
    {{ counter }}
    <button @click="add">+</button>
    <my-counter :inner-counter="counter"></my-counter>
  </div>
</template>

<script>
import myCounter from './components/my-counter.vue';

let add = function() {
  this.counter++;
};

export default {
  el: '#app',
  components: { myCounter },
  data: () => ({ counter: 0 }),
  methods: { add }
};
</script>
```

第 3 行

```html
{{ counter }}
<button @click="add">+</button>
```

外層有自己 `add()` 計算 counter。

第 5 行

```html
<my-counter :inner-counter="counter"></my-counter>
```

想要將 `counter` data 透過 `inner-counter` props 傳進 `my-counter` component。

12 行

```javascript
let add = function() {
  this.counter++;
};
```

上層有自己 `add()` 計算 `counter`。

> 這裡因為要使用 `this` 修改 `content`，所以只能使用 function expression，不能使用 arrow function

**my-counter.vue**

```html
<template>
  <div>
    <h2>{{ innerCounter }}</h2>
    <button @click="add">+</button>
  </div>
</template>

<script>
let add = function() {
  this.innerCounter++;
};

export default {
  name: 'my-counter',
  props: [ 'innerCounter' ],
  methods: { add }
};
</script>
```

15 行

```javascript
props: [ 'innerCounter' ],
```

宣告 `innerCounter` props。

第 9 行

```javascript
let add = function() {
  this.innerCounter++;
};
```

直接對 `innterCounter` props 做累加。

![prop000](/images/vue/component/props/prop000.png)

這種寫法雖然可以執行，但有個淺在問題：由於 Vue 的 unidirectional dataflow，每次 component 外層的 `data` 改變，都會同時透過 `props` 傳入內層 component，因而會 `覆蓋` 掉原本 component 的資料。

Vue 也在 console 提出警告，建議不要對 props 直接修改，因為會隨時會被外層 component 的 data 蓋掉。

由於不建議直接改 props，Vue 官網建議改用兩種 pattern 使用 props：

* Data
* Computed

##  Data

**App.vue**

```html
<template>
  <div id="app">
    {{ counter }}
    <button @click="add">+</button>
    <my-counter :start-counter="counter"></my-counter>
  </div>
</template>

<script>
import myCounter from './components/my-counter.vue';

let add = function() {
  this.counter++;
};

export default {
  el: '#app',
  components: { myCounter },
  data: () => ({ counter: 0 }),
  methods: { add }
};
</script>
```

第 5 行

```html
<my-counter :start-counter="counter"></my-counter>
```

由 `inner-counter` props 改成 `start-counter` props。

> `inner-counter` 要留給 component 的 `data` 所用

**my-counter.vue**

```html
<template>
  <div>
    <h2>{{ innerCounter }}</h2>
    <button @click="add">+</button>
  </div>
</template>

<script>
let add = function() {
  this.innerCounter++;
};

export default {
  name: 'my-counter',
  props: [ 'startCounter' ],
  data: function() {
    return {
      innerCounter: this.startCounter
    };
  },
  methods: { add }
};
</script>
```

15 行

```javascript
props: [ 'startCounter' ],
```

改成 `startCounter` props。

16 行

```javascript
data: function() {
  return {
    innerCounter: this.startCounter
  };
},
```

由於 prop 不適合直接修改，因此我們另外宣告了 `innerCounter`。

`startCounter` 的用途只在於初始化 `innerCounter`，之後 `startCounter` 有任何變動，都無法改變 `innerCounter`。

> 由原本直接修改 prop，改成修改 Data，prop 只退化用來初始化 model

```javascript
data: () => ({ innerCounter: this.startCounter }),
```

注意這裡只使能使用 function expression，不能使用以上的 arrow function 寫法，因為在 data function 使用了 `this` context 存取 `props`，arrow function 將無法改變 `this` 指向 component，所以一定得用 function expression。

第 9 行

```javascript
let add = function() {
  this.innerCounter++;
};
```

直接對 `innerCounter` data 做累加。

> 這裡因為要使用 `this` 讀寫 `innerCounter` data，所以只能使用 function expression，不能使用 arrow function

所以 Vue 使用 function expression 或 arrow function，關鍵就在是否使用了 `this`。

## Computed

使用 `data` 的優點是內層 component 的資料不會受外層影響，但缺點是內層 component 的資料不會與外層連動。

若要內外層 `data` 能夠連動，Vue 則建議改用 `computed`。

**my-counter.vue**

```html
<template>
  <div>
    <h2>{{ add }}</h2>
  </div>
</template>

<script>
let add = function() {
  return this.startCounter + 1;
};

export default {
  name: 'my-counter',
  props: [ 'startCounter' ],
  computed: { add }
};
</script>
```

第 8 行

```javascript
let add = function() {
  return this.startCounter + 1;
};
```

一樣是 `add()`，但改用 return，而非直接改 counter。

> 這裡因為要使用 `this` 讀取 `startContent` property，所以只能使用 function expression，不能使用 arrow function

15 行

```javascript
computed: { add }
```

將 `add()` 由原本由 `methods` 修改成 `computed`。

> 我們可以發現 `methods` 與 `computed` 本質都是 function，但因為宣告的 property 不一樣，其功用就不一樣

如此內層 component 的 data 就與外層同步。

> 2 種寫法都各有適用場景，視需求決定要使用哪種方式

## Conclusion

* 透過 `props`，`data` 也能由外層傳進 component
* Dynamic props 能將 MVVM 的 `data` 綁定 `props`，不再只是寫死的資料
* Props 只支援 Unidirectional dataflow，也就是外層的 data 改變，會影響到內層 component，但內層 component 的 data 改變，不會影響到外層 component
* `Data` 與 `computed` 能改善 `props` 的 unidirectional dataflow，但也不能完全取代，需視需求而使用
* 若要內層 component 的 data 改變，也要影響到外層 component，就必須使用 event
* Vue 的 `data`、`computed`、`method` 要使用 function expression 或 arrow function，關鍵就在於使否使用了 `this`

## Sample Code

完整範例可以在我的 [GitHub](https://github.com/oomusou/vue-props) 上找到

## Reference

[Vue](https://vuejs.org), [Props](https://vuejs.org/v2/guide/components-props.html)