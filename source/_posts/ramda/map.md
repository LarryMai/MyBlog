title: Ramda 之你所不知道的 map()
tags:
  - Ramda
  - Ramda/map
  - Ramda/compose
  - Ramda/pipe
  - Ramda/flip
  - Ramda/subtract
feature: images/feature/ramda.png
date: 2019-06-07 16:18:17
---
`map()` 是 FP 最具代表性的 Higher Order Function，僅管大部分 FP 原則大家都沒搞得很懂 (包括我自己)，但 `map()` 各位一定都用得很熟；大部分人都將 `map()` 用在 Array，事實上 Function as Data，`map()` 也能直接用在 Function 上。

<!-- more -->

## Version

VS Code 1.35.0
Quokka 1.0.224
Ramda 0.26.1

## Currying

```javascript
// fn :: Number -> Number -> Number
let fn = discount => price => price - discount - 10;

fn(20)(100); // ?
```

`fn()` 第一個參數為 `discount`，第二個參數為 `price`，除此之外，還在 function body 內多扣了 `10` 元。

這個 function 就功能而言 100% 沒問題，但能否更進一步 point-free 呢 ?

![map000](/images/ramda/map/map000.png)

## pipe()

```javascript
import { pipe, subtract, flip, add } from 'ramda';

// fn :: Number -> Number -> Number
let fn = pipe(
  add(10),
  flip(subtract)
);

fn(20)(100); // ?
```

最直覺會想用 `pipe()`，多減 `10` 元就相當於 `discount` 多加 `10` 元，所以先套用 `add(10)` 將 `discount` 加 `10`，在傳到下一個 `subtract()`。

但 `subtract()` 第一個參數為 `被減數`，而 `add(10)` 結果為 `減數`，因此必須使用 `flip()` 將 `subtract()` 參數位置反轉後才能使用。

![map001](/images/ramda/map/map001.png)

## compose()

```javascript
import { compose, subtract, flip, add } from 'ramda';

// fn :: Number -> Number -> Number
let fn = compose(
  flip(subtract),
  add(10)
);

fn(20)(100); // ?
```

既然能用 `pipe()` 就能使用 `compose()`，`pipe()` 是 `由左到右` 執行，而 `compose()` 是 `由右向左` 執行。

![map002](/images/ramda/map/map002.png)

## map()

```javascript
import { map, subtract, flip, add } from 'ramda';

// fn :: Number -> Number -> Number
let fn = map(
  flip(subtract),
  add(10)
);

fn(20)(100); // ?
```

跟 `compose()` 寫法長得好像喔，只是將 `compose()` 換成 `map()`，難道 `map()` 等於 `compose()` ?

> **map()**
> `Functor f => (a → b) → f a → f b`
> 將 Functor a 轉換成 Functor b

這是 `map()` 的定義，注意 Ramda 從來都沒說第二個參數是 array，而是說它是個 `Functor`。

Array 是 Functor，function 也是 `Functor`，所以也能將 function 用在 `map()`。

`add(10)` 回傳的是 `Number -> Number`，是 function。

`flip(subtract)` 在 `map()` 的第一個參數，為 projection function，可將 `add(10)` 這個 function 透過 `flip(subtract)` 加以轉換，又因 `add(10)` 欠參數未填滿，所以剛好提供 `calPrice` 參數。

> 事實上在 [map()](https://ramdajs.com/docs/#map) 官網，最後一句話就是 `Also treats functions as functors and will compose them together.`，只可惜微言大意，並沒有更進一步的範例，因此很容易忽略

![map003](/images/ramda/map/map003.png)

## Conclusion

* `pipe()` 是 `由左向右`，而 `compose()` 是 `由右向左`，兩者可等價替換
* 若只 `compose()` 兩個 function，可等價使用 `map()` 替換，因為 function 也是 `Functor`，將一個 function 透過 projection function 轉換，等效於兩個 function 去 compose

## Reference

[Ramda](https://ramdajs.com), [map()](https://ramdajs.com/docs/#map)
[Ramda](https://ramdajs.com), [pipe()](https://ramdajs.com/docs/#pipe)
[Ramda](https://ramdajs.com), [flip()](https://ramdajs.com/docs/#flip)
[Ramda](https://ramdajs.com), [subtract()](https://ramdajs.com/docs/#subtract)
[Ramda](https://ramdajs.com), [compose()](https://ramdajs.com/docs/#compose)
