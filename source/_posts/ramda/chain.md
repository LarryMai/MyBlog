title: 使用 chain() 改變 Chain 內的值
tags:
  - Ramda
  - Ramda/chain
  - Ramda/map
  - Ramda/flatten
  - Ramda/assoc
  - Ramda/prop
feature: images/feature/ramda.png
date: 2019-06-07 15:59:38
---
`chain()` 屬於 Ramda 較進階 Function，威力強大。當 Data 為 Nested Array 時，其結果為一層 Array，俗稱 Flatten Map；當 Data 為 Function 時，可將 Data 先透過 Function 處理，再連同 Data 傳入 Main Function。

<!-- more -->

## Version

VS Code 1.35.0
Quokka 1.0.224
Ramda 0.26.1

## Array

### Imperative

```javascript
let data = [
  { title: 'Secrets of the JavaScript Ninja', authors: ['John Resig', 'Bear Bibeault'] },
  { title: 'RxJS in Action', authors: ['Paul P.Daniels', 'Luis Atencio'] }
];

// getBooks :: [a] -> [b]
let fn = source => {
  const result = [];

  for (let x of source) {
    for (let y of x.authors) {
      result.push(y);
    }
  }

  return result;
};

fn(data); // ?
```

實務上常會遇到 array 內有 array 的 nested array，若使用 imperative，就得使用兩層 `for` loop，且內層 array 由外層 array 決定。

![chain000](/images/ramda/chain/chain000.png)

### Functional

```javascript
import { map } from 'ramda';

let data = [
  { title: 'Secrets of the JavaScript Ninja', authors: ['John Resig', 'Bear Bibeault'] },
  { title: 'RxJS in Action', authors: ['Paul P.Daniels', 'Luis Atencio'] }
];

// fn :: [a] -> [b]
let fn = map(x => x.authors);
fn(data); // ?
```

若使用 `map()`，由於 data 是兩層 array，最後結果也會維持兩層 array，這顯然不是我們所要的。

![chain003](/images/ramda/chain/chain003.png)

### flatten()

```javascript
import { map, pipe, flatten } from 'ramda';

let data = [
  { title: 'Secrets of the JavaScript Ninja', authors: ['John Resig', 'Bear Bibeault'] },
  { title: 'RxJS in Action', authors: ['Paul P.Daniels', 'Luis Atencio'] }
];

// fn :: [a] -> [b]
let fn = pipe(
  map(x => x.authors),
  flatten,
);

fn(data); // ?
```

Ramda 另提供 `flatten()`，專門將多層 array 攤平成一層 array，這就是我們所要的結果了。

> **flatten()**
> `[a] -> [b]`
> 將多層 array 攤平成一層 array

![chain002](/images/ramda/chain/chain002.png)

### chain()

```javascript
import { chain } from 'ramda';

let data = [
  { title: 'Secrets of the JavaScript Ninja', authors: ['John Resig', 'Bear Bibeault'] },
  { title: 'RxJS in Action', authors: ['Paul P.Daniels', 'Luis Atencio'] }
];

// fn :: [a] -> [b]
let fn = chain(x => x.authors);

fn(data); // ?
```
對於 `pipe(map, flatten)`，由於太常使用，Ramda 另外提供了 `chain()`，因此只要使用 `chain()` 就能達成需求。

> **chain()**
> `Chain m => (a → m b) → m a → m b`
> 若 data 為 array，相當於 `flatten()` 與 `map()` 組合

`m` 為 `Chain` 型別，其定義可參考 [Fantacy Land](https://github.com/fantasyland/fantasy-land#chain)，先簡單想成可以是 array 或 function 即可。

`(a -> m b)`：第一個參數為 function，由任意型別轉成 array 或 function

`m a`：第二個參數為 array 或 function

`m b`：回傳為另一個 array 或 function

![chain004](/images/ramda/chain/chain004.png)

### Point-free

```javascript
import { chain, prop } from 'ramda';

let data = [
  { title: 'Secrets of the JavaScript Ninja', authors: ['John Resig', 'Bear Bibeault'] },
  { title: 'RxJS in Action', authors: ['Paul P.Daniels', 'Luis Atencio'] }
];

// fn :: [a] -> [b]
let fn = chain(prop('authors'));

fn(data); // ?
```

若想讓 `chain()` 的 callback 也 point-free，可改用 `prop()` 產生。

![chain005](/images/ramda/chain/chain005.png)

## Function

### map()

```javascript
import { map } from 'ramda';

let data = [
  { id: 1, title: 'FP in JavaScript' },
  { id: 2, title: 'RxJS in Action' },
  { id: 3, title: 'Speaking JavaScript' }
];

// fn :: [a] -> [b]
let fn = map(x => ({ ...x, price: x.id }));

console.dir(fn(data));
```

原本 object 只有 `id` 與 `title` 兩個 property，希望能新增 `price` property，其值與  `id` 一樣。

![chain001](/images/ramda/chain/chain001.png)

### chain()

```javascript
import { assoc, chain, map, prop, pipe, add } from 'ramda';

let data = [
  { id: 1, title: 'FP in JavaScript' },
  { id: 2, title: 'RxJS in Action' },
  { id: 3, title: 'Speaking JavaScript' }
];

// addPrice :: a -> b
let addPrice = chain(
  assoc('price'),
  prop('id')
);

// fn :: [a] -> [b]
let fn = map(addPrice);

console.dir(fn(data));
```

之前 `map()` 的 callback 還帶有 `x`， 還不是 point-free，有進一步優化的空間嗎 ?

15 行

```javascript
// fn :: [a] -> [b]
let fn = map(addPrice);
```

`map()` 的 callback 由 `addPrice()` 產生。

第 9 行

```javascript
// addPrice :: a -> b
let addPrice = chain(
  assoc('price'),
  prop('id')
);
```

對於新增 property，Ramda 另外提出了 `assoc()`。

> **assoc()**
> `String -> a -> {k: v} -> {k: v}`
> 複製原本 object 的 property 外，還可新增 property

第一個參數為 key，我們指定為 `price`，第二個參數為 value，由於其值根據 `id` ，因此透過 `prop('id')` 取得再傳給 `assoc()`，因此使用了 `chain()`。

> **chain()**
> `Chain m => (a → m b) → m a → m b`
> 若 data 為 function， `chain(f, g)(x) = f(g(x), x)`

![chain006](/images/ramda/chain/chain006.png)

## Conclusion

* 當 `chain()` 搭配 array 時，可視為 `flatMap()`，用來將 nested array 攤平
* 當 `chain()` 搭配 function 時，可將 data 先透過 application function，再連同 data 傳入main function

## Reference

[Fantasy Land](https://github.com/fantasyland/fantasy-land#chain), [Chain](https://github.com/fantasyland/fantasy-land#chain)
[Ramda](https://ramdajs.com), [map()](https://ramdajs.com/docs/#map)
[Ramda](https://ramdajs.com), [flatten()](https://ramdajs.com/docs/#flatten)
[Ramda](https://ramdajs.com), [chain()](https://ramdajs.com/docs/#chain)
[Ramda](https://ramdajs.com), [assoc()](https://ramdajs.com/docs/#assoc)
[Ramda](https://ramdajs.com), [prop()](https://ramdajs.com/docs/#prop)