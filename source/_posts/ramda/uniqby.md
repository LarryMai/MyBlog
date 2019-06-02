title: 使用 uniqBy() 經過轉換後再取得不重複資料
tags:
  - Ramda
  - Ramda/uniqBy
  - Ramda/uniq
  - Ramda/identity
feature: images/feature/ramda.png
date: 2019-05-13 19:53:53
---
對於一般需求，`uniq()` 即可勝任，但若需經過 Function 轉換後才比較，則要使用 `uniqBy()`，自行傳入 Callback。

<!-- more -->

## Version

VS Code 1.33.1
Quokka 1.0.212
Ramda 0.26.1

## Functional

```javascript
import { uniqBy, identity } from 'ramda';

let data = [1, 2, 3, 1];

// fn :: [a] -> [a]
let fn = uniqBy(identity);

console.log(fn(data));
```

`data` 很明顯 `1` 是重複的，我們希望結果只顯示 `[1, 2, 3]`。

若我們不知道 `uniq()`，也可以使用 `uniqBy()` 與 `identity()` 組合而成。

> **uniqBy()**
> `(a -> b) -> [a] -> [a]`
> 將 array 經過 supply function 轉換，回傳 element 不重複 array

`(a -> b)`：supply function，先經過轉換後再比較

`[a]`：data 為 array

`[a]`：回傳 element 不重複 array

> 事實上 Ramda 的 `uniq()` 就是由 `uniqBy(identity)` 實現

![uniqby002](/images/ramda/uniqby/uniqby002.png)

```javascript
import { uniq } from 'ramda';

let data = [1, 2, 3, 1];

// fn :: [a] -> [a]
let fn = uniq;

console.log(fn(data));
```

事實上 Ramda 已經提供 `uniq()`，可直接使用。

> **uniq()**
> `[a] -> [a]`
> 回傳 element 不重複 array

![uniqby003](/images/ramda/uniqby/uniqby003.png)

## Primitive

```javascript
import { uniqBy } from 'ramda';

let data = [1, 2, 3, -1];

// fn :: [a] -> [a]
let fn = uniqBy(x => Math.abs(x));

console.log(fn(data));
```

若 `1` 與 `-1` 視為相同，我們希望結果只顯示 `[1, 2, 3]`。

此時就不能再使用 `uniq()`，因為條件太特殊，只能使用 `uniqBy()`。

第 5 行

```javascript
// fn :: [a] -> [a]
let fn = uniqBy(x => Math.abs(x));
```

自行提供 `x => Math.abs(x)`，如此 `1` 與 `-1` 都視為相同。

![uniqby004](/images/ramda/uniqby/uniqby004.png)

## Object

```javascript
import { uniqBy } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 },
  { title: 'FP in JavaScript', price: 400 }
];

// fn :: [a] -> [a]
let fn = uniqBy(x => x.title);

console.dir(fn(data));
```

若 element 為 object，只要 `title` 相同就視為相同，也就是第一筆與第四筆，雖然 `price` 不同，但仍視為相同，我們希望結果只顯示不重複的前三筆。

此時就不能再使用 `uniq()`，因為條件太特殊，只能使用 `uniqBy()`。

10 行

```javascript
// fn :: [a] -> [a]
let fn = uniqBy(x => x.title);
```

自行提供 `x => x.title`，如此 object 只要 `title` 相同就視為相同。

![uniq002](/images/ramda/uniqby/uniqby000.png)

## Point-free

```javascript
import { uniqBy, prop } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 },
  { title: 'FP in JavaScript', price: 400 }
];

// fn :: [a] -> [a]
let fn = uniqBy(prop('title'));

console.dir(fn(data));
```

`x => x.title` 也可由 `prop()` 產生使其 point-free，可讀性更高。


![uniq004](/images/ramda/uniqby/uniqby001.png)

## Conclusion

* `uniq()` 也可使用 `uniqBy(identity)` 組合而成
* `uniq()` 可視為簡易版；而 `uniqBy()` 為進階版 `uniq()`
* 若只根據 object 特定 property 判斷 unique 條件，則要使用 `uniqBy()`

## Reference

[Ramda](https://ramdajs.com), [uniq()](https://ramdajs.com/docs/#uniq)
[Ramda](https://ramdajs.com), [uniqBy()](https://ramdajs.com/docs/#uniqBy)
[Ramda](https://ramdajs.com), [identity()](https://ramdajs.com/docs/#identity)