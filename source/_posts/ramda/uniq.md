title: Ramda 之 uniq()
tags:
  - Ramda
  - Ramda/uniq
feature: images/feature/ramda.png
date: 2019-05-10 23:26:20
---
ECMAScript 的 `Array.prototype` 並沒有內建 `uniq()`，在 ECMAScript 2015 可使用 `Set` 時實現，但僅限於 Primitive；Ramda 的 `uniq()` 可同時處理 Primitive 與 Object。

<!-- more -->

## Version

VS Code 1.33.1
Quokka 1.0.212
Ramda 0.26.1

## uniq()

```javascript
let data = [1, 2, 3, 1];

let uniq = arr => [...new Set(arr)];

console.log(uniq(data));
```

`data` 很明顯 `1` 是重複的，我們希望結果只顯示 `[1, 2, 3]`。

`Array.prototype` 雖然沒內建 method，但 ES2015 的 `Set` 具有不重複特性，再配合 spread operator，剛好可實作出 `uniq()`。

> 不過這種方法只能用在 primitive，不適用於 object，因為 `===` 比較的是 object reference，儘管 property 完全相同的 object，其 reference 依然不同

![uniq002](/images/ramda/uniq/uniq002.png)

## Primitive

```javascript
import { uniq } from 'ramda';

let data = [1, 2, 3, 1];

console.log(uniq(data));
```

事實上 Ramda 已經提供 `uniq()`，可直接使用。

> **uniq()**
> `[a] -> [a]`
> 回傳 element 不重複的 array

* `[a]`：data 為 array
* `[a]`：回傳 element 不重複的 array

![uniq000](/images/ramda/uniq/uniq000.png)

## Object

```javascript
import { uniq } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 },
  { title: 'FP in JavaScript', price: 100 }
];

console.dir(uniq(data));
```

`data` 很明顯第四筆是重複的，我們希望結果只顯示不重複的前三筆。

> Q：`===` 是比較 object reference，第一筆與第四筆的 object reference 顯然不同，為什麼 `uniq()` 可以視為重複 ?

`uniq()` 是以 Ramda 的 `equals()` 判斷 object 是否相等，而 `equals()` 比較的是 object property，而非 object reference，且 `equals()` 是以 deep 方式比較，而非 shallow。

![uniq001](/images/ramda/uniq/uniq001.png)

我們可發現 array 內無論是 primitive 或是 object，`uniq()` 都能勝任，若使用 ECMAScript 2015 的 `Set`，則只能處理 primitive，無法處理 object，因為其用的是 `===`，只能比較 object reference。

## Conclusion

* 若不使用 Ramda，則 `Set` 是簡單的寫法，唯 `Set` 只對 primitive 有效，對 object 無效
* Ramda 的 `uniq()` 是最通用的解決方案，無論 primitive 或 object 都適用

## Reference

[Ramda](https://ramdajs.com), [uniq()](https://ramdajs.com/docs/#uniq)