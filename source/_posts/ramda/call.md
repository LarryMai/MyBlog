title: Ramda 之 call()
tags:
  - Ramda
  - Ramda/call
  - Ramda/add
  - Ramda/converge
  - Ramda/add
  - Ramda/pipe
  - Ramda/subtract
  - Ramda/flip
feature: images/feature/ramda.png
date: 2019-05-14 22:23:43
---
Ramda 的 `call()` 乍看很不起眼，但若搭配 `converge()` 之後，就能動態產生 Converging Function。

<!-- more -->

## Version

VS Code 1.32.3
Quokka 1.0.195
Ramda 0.26.1

## call()

```javascript
import { call, add } from 'ramda';

console.log(call(add, 1, 2));
```

最簡單的 `call()`，第一個 argument 為 function，第二個 argument 之後為該 function 的 argument list，相當於執行 `add(1, 2)`。

> **call()**
> `(*… → a),*… → a`
> 執行第一個 argument 的 function，並將之後 argument 當作該 function 的 argument list

`(*… → a)`：任意 function

`*… `：任意 argument list

`a`：回傳 function 執行的結果

![call000](/images/ramda/call/call000.png)

## Imperative

```javascript
import { converge, prop } from 'ramda';

let product = {
  discount: 5000,
  price: 30000
};

// calPrice :: (Number, Number) -> Number
let calPrice = (discount, price) => price - discount - 1000;

// fn :: Number -> Number
let fn = product => calPrice(product.discount, product.price);
console.log(fn(product));
```

`calPrice()` 傳入 `discount` 與 `price` argument，會計算出折扣後價錢。

`fn()` 則傳入 `product` object，再呼叫 `calPrice()`。

![call003](/images/ramda/call/call003.png)

## Point-free

```javascript
import { converge, prop } from 'ramda';

let product = {
  discount: 5000,
  price: 30000
};

// calPrice :: (Number, Number) -> Number
let calPrice = (discount, price) => price - discount - 1000;

// fn :: Object -> Number
let fn = converge(
  calPrice, [
    prop('discount'),
    prop('price')
  ]
);

console.log(fn(product));
```

首先將 `fn()` point-free。

11 行

```javascript
// fn :: Object -> Number
let fn = converge(
  calPrice, [
    prop('discount'),
    prop('price')
  ]
);

console.log(fn(product));
```

`fn()` 的 argument 為 `product` object，包含 `discount` 與 `price` property，分別傳入 `折扣` 與 `價錢`。

最後要執行的是 `calPrice()`，分別有 `discount` 與 `price` 兩個 argument。

由於傳入 `fn()` 的是 object，因此勢必要先使用 `prop()` 拆解後才能傳給 `calPrice()` 執行。

這正是使用 `fn()` 時機，將 `calPrice()` 當作 converging function，將 `prop()` 當作 branching function。

目前就功能而言沒問題，唯 `calPrice()` 並非 point-free，是否有繼續優化空間呢 ?

![call001](/images/ramda/call/call001.png)

```javascript
import { converge, prop, call, add, pipe, subtract, flip } from 'ramda';

let product = {
  discount: 5000,
  price: 30000
};

// calPrice :: (Number, Number) -> Number
let calPrice = pipe(
  add(1000),
  flip(subtract)
);

// fn :: Object -> Number
let fn = converge(
  call, [
    pipe(prop('discount'), calPrice),
    prop('price')
  ]
);

console.log(fn(product));
```

由於 `calPrice()` 有兩個 argument：`discount` 與 `price`，基於 currying 特性，可將 `calPrice()` 視為由 `discount` 產生的 function，其 argument 只剩下 `price`。

第 8 行

```javascript
// calPrice :: (Number, Number) -> Number
let calPrice = pipe(
  add(1000),
  flip(subtract)
);
```

`calPrice()` 最直覺會想用 `pipe()`，多減 `1000` 元就相當於 `discount` 多加 `1000` 元，所以先套用 `add(1000)` 將 `discount` 加 `1000`，再傳到下一個 `subtract()`。

但 `subtract()` 第一個 argument 為 `被減數`，而 `add(1000)` 為 `減數`，因此必須使用 `flip()` 將 `subtract()` 的 signature 反轉後才能使用。

我們發現 `calPrice()` 已經 point-free 了。

14 行

```javascript
// fn :: Object -> Number
let fn = converge(
  call, [
    pipe(prop('discount'), calPrice),
    prop('price')
  ]
);
```

由於 `calPrice()` 由 `discount` 產生，因此 `converge()` 的 converging function 就不能再是 `calPrice()`，而要改用 `call()`，如此才會執行第一個 branching function。

第一個 branching function 先以 `prop('discount')` 拆解 object，再傳入 `calPrice()`，動態產生新的 function。

第二個 branching function 以 `prop('price')` 拆解 object，再傳入第一個 branching function 執行，這是因為 `call()` 的緣故。

![call002](/images/ramda/call/call002.png)

## Conclusion

* 當 converging function 並非固定，而是由其他 function 動態產生時，可搭配 `call()`，使其在 branching function 先動態產生 function，再由 `call()` 呼叫執行之

## Reference

[Ramda](https://ramdajs.com), [call()](https://ramdajs.com/docs/#call)
[Ramda](https://ramdajs.com), [add()](https://ramdajs.com/docs/#add)
[Ramda](https://ramdajs.com), [converge()](https://ramdajs.com/docs/#converge)
[Ramda](https://ramdajs.com), [add()](https://ramdajs.com/docs/#add)
[Ramda](https://ramdajs.com), [pipe()](https://ramdajs.com/docs/#pipe)
[Ramda](https://ramdajs.com), [subtract()](https://ramdajs.com/docs/#subtract)
[Ramda](https://ramdajs.com), [flip()](https://ramdajs.com/docs/#flip)