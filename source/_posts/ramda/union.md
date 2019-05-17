title: Ramda 之 union()
tags:
  - Ramda
  - Ramda/union
  - Ramda/concat
  - Ramda/uniq
  - Ramda/pipe
feature: images/feature/ramda.png
date: 2019-05-17 12:58:46
---
若想將兩個 Array 加以合併，並只保留不重複部分，Ramda 提供了 `union()`。

<!-- more -->

## Version

VS Code 1.4
Quokka 1.0.209
Ramda 0.26.1

## Functional

```javascript
import { pipe, concat, uniq } from 'ramda';

let first = [1, 3, 5];
let second = [5, 3, 7];

// union :: [*] → [*] → [*]
let union = pipe(concat, uniq);

// fn :: [*] → [*] → [*]
let fn = union;

console.log(fn(first, second));
```

根據 `union()` 原始意義：將兩個 Array 加以合併，只保留不重複部分，其實我們也可以使用 `concat()` 與 `uniq()` 組合而成。

![union000](/images/ramda/union/union000.png)

## union()

```javascript
import { union } from 'ramda';

let first = [1, 3, 5];
let second = [5, 3, 7];

// fn :: [*] → [*] → [*]
let fn = union;

console.log(fn(first, second));
```

Ramda 其實已經提供了 `union()`，可直接使用。

> **uniq()**
> `[*] → [*] → [*]`
> 將兩個 array 合併成不重複 array

`[*]`：第一個 array

`[*]`：第二個 array

`[*]`：回傳合併後不重複 array

![union001](/images/ramda/union/union001.png)

## Object

```javascript
import { union } from 'ramda';

let first = [
  { firstName: 'Sam',   lastName: 'Xiao'},
  { firstName: 'Kevin', lastName: 'Yang'},
  { firstName: 'Jimmy', lastName: 'Huang'},
];

let second = [
  { firstName: 'Kevin', lastName: 'Yang'},
];

// fn :: [*] → [*] → [*]
let fn = union;

console.dir(fn(first, second));
```

`union()` 也可以用在 object。

> 由於 `union()` 是使用 `uniq()` 實現，而 `uniq()` 是使用 `equals()` 比較，而非 ECMAScript 的 `===`，因此比較的是 value equality，而非 reference equality

![union002](/images/ramda/union/union002.png)

## Conclusion

* 只要牽涉到比較，Ramda 都是使用自家的 `equals()`，而非 ECMAScript 的 `===`，因此是 value equality，而非 reference equality

## Reference

[Ramda](https://ramdajs.com), [union()](https://ramdajs.com/docs/#union)
[Ramda](https://ramdajs.com), [pipe()](https://ramdajs.com/docs/#pipe)
[Ramda](https://ramdajs.com), [concat()](https://ramdajs.com/docs/#concat)
[Ramda](https://ramdajs.com), [uniq()](https://ramdajs.com/docs/#uniq)

