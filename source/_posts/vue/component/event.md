title: Vue Component 之 Event
tags:
  - Vue
feature: images/feature/vue.png
date: 2019-06-10 09:21:27
---
Vue Component 的 Props，只能解決 Data 由外層 Component 傳給內層 Component，若要將 Data 從內層 Component 傳到外層 Component 呢 ? 此時就要使用 Vue Component 的 Event，這也是 Vue 從 React 學來的。

<!-- more -->

## Version

Vue 2.6.10

## Simple Component with Custom Event

**App.vue**

```html
<template>
  <my-button @my-click="outerClick"></my-button>
</template>

<script>
import myButton from './components/my-button.vue'

let outerClick = num => console.log(num);

export default {
  name: 'app',
  components: { myButton },
  methods: { outerClick }
};
</script>
```

第 1 行

```html
<template>
  <my-button @my-click="outerClick"></my-button>
</template>
```

使用 `<my-button></my-button>`，並對 `my-click` 做 Event Binding 到 `outerClick()`，但 `my-click` event 在哪裡定義呢 ?

> Event binding 要加 event 名稱前加上 `@`，event handler 名稱不必加上 `()`

第 8 行

```javascript
let outerClick = num => console.log(num);
```

定義 `my-click` 的 event handler ：`outerClick()`，會接收 event 傳出的 `num` 並使用 `console.log()` 顯示。

> 由於 `outerClick()` 並沒有使用到 `this`，為 pure function，因此可直接使用 arrow function

**my-button.vue**

```html
<template>
  <button @click="innerClick">Click Me</button>
</template>

<script>
let innerClick = function() {
  this.$emit('my-click', 10);
};

export default {
  name: 'my-button',
  methods: { innerClick }
};
</script>
```

第 1 行

```html
<template>
  <button @click="innerClick">Click Me</button>
</template
```

定義 `my-button` component 的 HTML template，注意其 `<button></button>` 的 `click` ㄍevent binding 到 `my-button` 自己的 `innerClick()`。

第 6 行

```javascript
let innerClick = function() {
  this.$emit('my-click', 10);
};
```

使用 Vue Instance 所提供的 `$emit()` 發出 event。

* 第 1 個參數為 event 名稱
* 第 2 個參數為想透過 event 所傳出的值
* 若有多個值，可繼續其他參數

> Event 名稱必須為 Kebab-case，不能使用 CamelCase 或 camelCase，這裏 Vue 並不會自動幫你轉成 Kebab-case

由於 `innerClick()` 有使用到 `this`，有 side effect，因此只能使用 function expression，不能使用 arrow function

## MyCounter Again

**App.vue**

```html
<template>
  <div>
    <my-counter :start-counter="outerCounter" @emit-counter="showLatestCounter"></my-counter>
    <h1>{{ outerCounter }}</h1>
  </div>
</template>

<script>
import myCounter from './components/my-counter.vue'

let showLatestCounter = function(counter) {
  this.outerCounter = counter;
};

export default {
  name: 'app',
  components: { myCounter },
  data: () => ({ outerCounter: 0 }),
  methods: { showLatestCounter }
};
</script>
```

第 3 行

```html
<my-counter :start-counter="outerCounter" @emit-counter="showLatestCounter"></my-counter>
```

使用自定義的 `<my-counter></my-counter>`。

* 將 `outerCounter` data 傳進 `start-counter` props 當初始值
* 將 `emit-counter` event 做 event binding 到 `showLastCounter()`，但 `emit-counter` event 在哪裡定義呢 ?

第 4 行

```html
<h1>{{ outerCounter }}</h1>
```

顯示外層 component 接收內層 component 資料。

11 行

```javascript
let showLatestCounter = function(counter) {
  this.outerCounter = counter;
};
```

由 component 外部的 `showLatestCounter()` 接收 `emit-counter` event，將接收內部傳出的 `counter` 值給 `outerCounter` data 顯示。

**my-counter.vue**

```html
<template>
  <div>
    <h1>{{ innerCounter }}</h1>
    <button @click="add">+1</button>
    <button @click="emit">Emit Counter</button>
  </div>
</template>

<script>
let add = function() {
  this.innerCounter++;
};

let emit = function() {
  this.$emit('emit-counter', this.innerCounter);
};

export default {
  name: 'my-counter',
  props: [ 'startCounter' ],
  data: function() {
    return {
      innerCounter: this.startCounter
    };
  },
  methods: {
    add,
    emit
  }
};
</script>

```

第 1 行

```html
<template>
  <div>
    <h1>{{ innerCounter }}</h1>
    <button @click="add">+1</button>
    <button @click="emit">Emit Counter</button>
  </div>
</template>
```

定義 `my-counter` component 的 HTML template，注意有兩個 `<button/>`，分別對應到 `add()` 與 `emit()`。

其中 `add()` 負責 `innerCounter` 的累加。

而 `emit()` 負責發出 event 通知 component 外層。

20 行

```javascript
props: [ 'startCounter' ],
```

宣告 `startCounter` props，讓外層藉由 `startCounter` 傳入 counter 的初始值。

21 行

```javascript
data: function() {
  return {
    innerCounter: this.startCounter
  };
},
```

定義 `innerCounter` data，由 `startCounter` props 定義 `innerCounter` 初始值。

> 因為需使用 `this` context 存取 `startCounter` props，屬於 side effect，所以要使用 function expression，不能使用 arrow function

10 行

```javascript
let add = function() {
  this.innerCounter++;
};
```

對 `innerCounter` data 做累加。

> 因為需使用 `this` context 存取 `innerCounter` data，屬於 side effect，所以要使用 function expression，不能使用 arrow function

14 行

```javascript
let emit = function() {
  this.$emit('emit-counter', this.innerCounter);
};
```

這行是關鍵，對外發出 `emit-counter` event，並傳出目前的 `innerCounter` data。

> 因為需使用 `this` context 存取 `$emit()` ，屬於 side effect，所以要使用 function expression，不能使用 arrow function


## .sync Modifier

回想剛剛的 `my-counter` 範例：

1. `my-counter` component 透過 `emit-counter` event 傳出目前 `innerCounter` data
2. 外層再透過 `showLatestCounter()` 接收 `emit-counter` event 傳來的值
3. 將 event 接收值指定到 `outerCounter` data
4. 顯示 `outerCounter` data

實務上這種內層 component 的 data 一改變，在外層的 data 也要立即改變的應用非常多，Vue 另外提供了 `.sync` modifier，可以少寫很多程式碼。

**App.vue**

```html
<template>
  <div>
    <my-counter :start-counter.sync="outerCounter" @emit-counter="showLatestCounter"></my-counter>
    <h1>{{ outerCounter }}</h1>
  </div>
</template>

<script>
import myCounter from './components/my-counter.vue'

export default {
  name: 'app',
  components: { myCounter },
  data: () => ({ outerCounter: 0 }),
};
</script>

```

第 3 行

```html
<my-counter :start-counter.sync="outerCounter" @emit-counter="showLatestCounter"></my-counter>
```

一樣透過 `start-counter` props 傳入 `outerCounter` data，但多了 `.sync` modifier，表示將來 `my-counter` component 內部 data 有任何改變，則 `outerCounter` data 會同步跟著改變。

11 行

```javascript
export default {
  name: 'app',
  components: { myCounter },
  data: () => ({ outerCounter: 0 }),
};
```

只需對 `outerCounter` 設定初始值即可。

因為 `outerCounter` data 會自動與 `my-counter` component 的 data 同步，原來的 `showLatestCounter()` 已經不需要。

**my-counter.vue**

```html
<template>
  <div>
    <h1>{{ innerCounter }}</h1>
    <button @click="add">+1</button>
    <button @click="emit">Emit Counter</button>
  </div>
</template>

<script>
let add = function() {
  this.innerCounter++;
};

let emit = function() {
  this.$emit('update:startCounter', this.innerCounter);
};

export default {
  name: 'my-counter',
  props: [ 'startCounter' ],
  data: function() {
    return {
      innerCounter: this.startCounter
    };
  },
  methods: {
    add,
    emit
  }
};
</script>
```

14 行

```javascript
let emit = function() {
  this.$emit('update:startCounter', this.innerCounter);
};
```

由原本 emit `emit-counter` event，改成 `update:startCounter` event，第二個參數一樣傳出 `innerCounter` model。

> Vue 規定，若要使用 `.sync` modifier，必須 emit `update:prop` 這種格式的 event，如此 `.sync` modifer 才收得到

## v-model Directive

> Q : `.sync` modifier 看起來好像 two way binding，可以使用 `v-model` 嗎 ?

**App.vue**

```html
<template>
  <div>
    <my-counter v-model="outerCounter"></my-counter>
    <h1>{{ outerCounter }}</h1>
  </div>
</template>

<script>
import myCounter from './components/my-counter.vue'

export default {
  name: 'app',
  components: { myCounter },
  data: () => ({ outerCounter: 0 }),
};
</script>
```

第 3 行

```html
<my-counter v-model="outerCounter"></my-counter>
```

直接在 `my-counter` component 使用 `v-model` directive，並綁定到 `outerCounter` data。

**my-counter.vue**

```html
<template>
  <div>
    <h1>{{ innerCounter }}</h1>
    <button @click="add">+1</button>
    <button @click="emit">Emit Counter</button>
  </div>
</template>

<script>
let add = function() {
  this.innerCounter++;
};

let emit = function() {
  this.$emit('input', this.innerCounter);
};

export default {
  name: 'my-counter',
  props: [ 'value' ],
  data: function() {
    return {
      innerCounter: this.value
    };
  },
  methods: {
    add,
    emit
  }
};
</script>
```

20 行

```javascript
props: [ 'value' ],
```

`v-model` directive 預設使用 `value` props，所以要自行宣告。

14 行

```javascript
let emit = function() {
  this.$emit('input', this.innerCounter);
};
```

`v-model` directive 預設接收 `input` event，所以要自行 emit。

要使用 `v-model` directive，要遵循 Vue 的兩項規定：

1. 使用 `value` props
2. 使用 `input` event

>`sync` modifier 與 `v-model` directive 兩者功能相同，但 `v-model` 觀念比較容易理解，程式碼也比較少，個人偏好 `v-model`

## Binding to DOM Event

目前外層可以透過內層 component 發出的 custom event 接收到 data，但如第一個 `my-button` component，其實是內層 component 監聽 `click` DOM event 並且發出自己的 `my-click` custom event。

既然源頭是 `click` DOM event，能否讓外層直接 event binding 到內層 `my-button` 的 `click` DOM event 呢 ?

**App.vue**

```html
<template>
  <my-button @click.native="outerClick"></my-button>
</template>

<script>
import myButton from './components/my-button.vue';

let outerClick = () => console.log('Button clicked');

export default {
  name: 'app',
  components: { myButton },
  data: () => ({ outerCounter: 0 }),
  methods: { outerClick }
};
</script>
```

第 2 行

```html
<my-button @click.native="outerClick"></my-button>
```

想直接綁定 `my-button` 的 `click` DOM event，由於是原生 event，只要在 `@click` 加上 `.native` modifier，Vue 就會幫你將 `outerClick()` 直接綁定到原生的 `click` DOM event。

第 8 行

```javascript
let outerClick = () => console.log('Button clicked');
```

由於 `outerClick()` 直接綁定到 DOM event，所以就收不到內層 `my-button` component 所傳出的 data，因此稍作修改。

**my-button.vue**

```html
<template>
  <button>Click Me</button>
</template>

<script>
export default {
  name: 'my-counter',
};
</script>
```

也因為 `click.native` 直接綁定到原生的 DOM event，因此就不需要另外 emit event 了。

## Conclusion

* 透過 event，data 能由內層 component 傳出到外層 component
* `sync` modifier 讓我們直接將 component 內外的 data 同步
* `v-model` directive 算是 `sync` modifier 的簡化版，讓我們更直覺實現 component 的 two way binding
* `native` modifier 能讓我們直接綁定 component 內的 DOM event，不用再自行 emit event

## Sample

完整的範例可以在我的 [GitHub](https://github.com/oomusou/vue-event) 上找到

## Reference

[Vue](https://vuejs.org), [Custom Event](https://vuejs.org/v2/guide/components-custom-events.html)

