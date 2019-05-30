title: 如何在 Vue 使用 Ramda ?
tags:
  - Ramda
  - Vue
feature: images/feature/ramda.png
date: 2018-05-18 10:35:27
---
Ramda 是 Clojure 在 ECMAScript 的實作，讓我們可以將更多 FP 特性在 ECMAScript 實現。本文將以 Vue 為例，示範如何在 Vue 使用 Ramda。

<!-- more -->

## Version

Vue 2.6.6
Vue CLI 3.5.1
Ramda 0.26.1

## 安裝 Ramda

```shell
$ yarn add ramda
```

![ramda000](/images/ramda/vue/ramda000.png)

![ramda001](/images/ramda/vue/ramda001.png)

安裝完後在 `package.json` 的 `dependencies` 下多了 `ramda`。

## 使用 Ramda

**App.vue**

```html
<template>
  <div id="app">
    {{ totalPrice(books) }}
  </div>
</template>

<script>
import { filter, map, pipe, reduce, add, prop, compose, lt } from 'ramda';

let totalPrice = pipe(
  filter(compose(lt(100), prop('price'))),
  map(prop('price')),
  reduce(add, 0)
);

export default {
  name: 'app',
  data: () => ({
    books: [
      { title: 'FP in JavaScript', price: 100 },
      { title: 'RxJS in Action', price: 200 },
      { title: 'speaking JavaScript', price: 300 }
    ],
  }),
  methods: {
    totalPrice,
  }
}
</script>
```

一個最簡單的 Ramda。

第 8 行

```javascript
import { filter, map, pipe, reduce, add, prop, compose, lt } from 'ramda';
```

Import 有用到的 Ramda function。

第 10 行

```javascript
let totalPrice = pipe(
  filter(compose(lt(100), prop('price'))),
  map(prop('price')),
  reduce(add, 0)
);
```

使用 Ramda 實現 `totalPrice()` method，使用了 `filter()`、`map()` 與 `reduce()`，也使用了 `compose()`、`lt()`、`prop()` 與 `add()` 使其 Point-free。

第 3 行

```javascript
{{ totalPrice(books) }}
```

將 `books` 傳入 `totalPrice()`。

![ramda002](/images/ramda/vue/ramda002.png)

若能顯示 `500`，表示 Ramda 已經成功執行在 Vue。

## Conclusion

* 在 Vue 使用 Ramda 很簡單，只要使用 yarn 安裝 Ramda，然後在 ECMAScript import 想用的 function 即可

## Sample Code

完整範例可以在我的 [GitHub](https://github.com/oomusou/vue-ramda) 上找到

## Reference

[Ramda](https://ramdajs.com), [pipe()](https://ramdajs.com/docs/#pipe)
[Ramda](https://ramdajs.com), [compose()](https://ramdajs.com/docs/#compose)
[Ramda](https://ramdajs.com), [filter()](https://ramdajs.com/docs/#filter)
[Ramda](https://ramdajs.com), [lt()](https://ramdajs.com/docs/#lt)
[Ramda](https://ramdajs.com), [prop()](https://ramdajs.com/docs/#prop)
[Ramda](https://ramdajs.com), [map()](https://ramdajs.com/docs/#map)
[Ramda](https://ramdajs.com), [add()](https://ramdajs.com/docs/#add)
[Ramda](https://ramdajs.com), [reduce()](https://ramdajs.com/docs/#reduce)

