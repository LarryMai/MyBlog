title: Crocks 之 prop()
tags:
  - Ramda
  - Ramda/prop
  - Crocks
  - Crocks/prop
  - Maybe
  - Maybe/map
  - Maybe/option
feature: images/feature/crocks.png
date: 2019-05-23 11:16:45
---
Ramda 的 `prop()` 可能回傳 `undefined`，這也是常見 Bug 來源之一；而 Crocks 的 `prop()` 則回傳 `Maybe`，可確保 ECMAScript 不再回傳不預期結果。 

<!-- more -->

## Version

VS Code 1.34.0
Quokka 1.0.216
Ramda 0.26.1
Crocks 0.11.1

## ECMAScript

```javascript
let obj = {
  title: 'FP in JavaScript', price: 100
};

// fn :: Object -> Number
let fn = obj => obj.price * 0.8;

console.log(fn(obj));
```

`fn()` 會取 `obj` 的 `price` 再打八折，若 `obj` 正確提供 `price` property，則一切正常。

![prop000](/images/crocks/maybe/prop/prop000.png)

```javascript
let obj = {
  title: 'FP in JavaScript'
};

// fn :: Object -> Number
let fn = obj => obj.price * 0.8;

console.log(fn(obj));
```

若 `obj` 根本沒有 `title` property，結果為 `NaN`，這並不是我們預期結果。

![prop001](/images/crocks/maybe/prop/prop001.png)

## Ramda

```javascript
import { pipe, prop, multiply } from 'ramda';

let obj = {
  title: 'FP in JavaScript'
};

// fn :: Object -> Number
let fn = pipe(
  prop('price'), 
  multiply(0.8)
);

console.log(fn(obj));
```

> **prop()**
> `s → {s: a} → a | Undefined`
> 傳入 key 與 object，傳回 value

若 key 找不到，`prop()` 會回傳 `undefined`，所以儘管使用了 Ramda 的 `prop()`，問題依舊沒解決，一樣是 `NaN`。

![prop002](/images/crocks/maybe/prop/prop002.png)

## Maybe

```javascript
import { multiply, prop, isNil, not, compose } from 'ramda';
import { safe } from 'crocks';

let obj = {
  title: 'FP in JavaScript',
};

// isNotNil :: * -> Boolean
let isNotNil = compose(not, isNil);

// prop_ = s -> {s: a} -> Maybe a
let prop_ = compose(safe(isNotNil), prop);

// fn :: Object -> Number
let fn = obj => prop_('price', obj)
  .map(multiply(0.8))
  .option(0);

console.log(fn(obj));
```

我們希望 `prop()` 能回傳 `Maybe`，property 存在則回傳 `Just`，若不存在則回傳 `Nothing` 而不是 `undefined`。

12 行

```javascript
// prop_ = s -> {s: a} -> Maybe a
let prop_ = compose(safe(isNotNil), prop);
```

自訂 `prop_()` 取代 Ramda 的 `prop()`，signature 與 Ramda 的 `prop()` 完全相同，差異只在於回傳 `Maybe`。

透過 `safe(isNotNil)` 將 `prop()` 回傳結果轉成 `Maybe`。

第 8 行

```javascript
// isNotNil :: * -> Boolean
let isNotNil = compose(not, isNil);
```

但 Ramda 只提供 `isNil()`，因此自行由 `not()` 與 `isNil()` 組合出 `isNotNil()`。

14 行

```javascript
// fn :: Object -> Number
let fn = obj => prop_('price', obj)
  .map(multiply(0.8))
  .option(0);
```

`fn()` 改用自訂的 `prop_()`。

由於 `prop_()` 回傳 `Maybe`，因此就不能再使用 `pipe()`，而要改用 `Maybe` 的 `map()` 執行 `multiply(0.8)`。

最後再透過 `option()` 將 `Maybe` 轉回 number。

![prop003](/images/crocks/maybe/prop/prop003.png)

## Crocks

```javascript
import { multiply } from 'ramda';
import { prop } from 'crocks';

let obj = {
  title: 'FP in JavaScript',
};

// fn :: Object -> Number
let fn = obj => prop('price', obj)
  .map(multiply(0.8))
  .option(0);

console.log(fn(obj));
```

事實上 Crocks 已經提供回傳 `Maybe` 的 `prop()`，可直接使用。

> **prop()**
> `(String | Integer) -> a -> Maybe b`
>
> 取得 object 的 property 或 array 的 element，回傳為 `Maybe`

`String | Integer`：object 的 key 或 array 的 index

`a`：data 可為 object 或 array

`Maybe b`：回傳 object 的 value 或 array 的 element，但會包成 `Maybe`

> 與 Ramda 的 `prop()` 不同點除了回傳 `Maybe` 外，Crocks 的 `prop()`  還能用在 array

![prop004](/images/crocks/maybe/prop/prop004.png)

## Conclusion

* Ramda 與 ECMAScript 妥協，有些 function 回傳 `undefiend`，如 `prop()`，這很容易產生 bug
* Crocks 的 `prop()` 除了用在 object，也能用在 array
* Crocks 的 `prop()` 回傳的是 `Maybe`，強迫我們要透過 `option()` 處理 `undefined`，實務上建議使用 Crocks 的 `prop()` 取代 Ramda 的 `prop()`

## Reference

[Andy Van Slaars](https://egghead.io/instructors/andrew-van-slaars), [Safely Access Object Properties with prop()](https://egghead.io/lessons/javascript-safely-access-object-properties-with-prop)
[Ramda](https://ramdajs.com), [prop()](https://ramdajs.com/docs/#prop)
[Crocks](https://evilsoft.github.io/crocks/), [prop()](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html#prop)