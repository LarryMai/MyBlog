title: Ramda 之 without()
tags:
  - Ramda
  - Ramda/without
  - Ramda/difference
  - Ramda/flip
feature: images/feature/ramda.png
date: 2019-05-16 14:23:03
---
若要找出第一個 Array 不在第二個 Array 的 Element，我們會使用 `difference()`，若要找出第二個 Array 不在第一個 Array 的 Element 呢 ?

<!-- more -->

## Version

VS Code 1.33.1
Quokka 1.0.209
Ramda 0.26.1

## Functional

```javascript
import { flip, difference } from 'ramda';

let first = [1, 3, 5, 7];
let second = [1, 4, 7];

// without :: [a] → [a] → [a]
let without = flip(difference);

// fn :: [a] → [a] → [a]
let fn = without;

console.log(without(first, second));
```

想知道 `second` array 哪些 element 不存在於 `first` array，，若不知道 Ramda 提供了 `without()`，其實也可以由 `difference()` 與 `flip()` 組合出來。

![without000](/images/ramda/without/without000.png)

## without()

### Primitive

```javascript
import { without } from 'ramda';

let first = [1, 3, 5, 7];
let second = [1, 4, 7];

console.log(without(first, second));
```

其實 Ramda 已經提供了 `without()`，可直接使用。

> **without()**
> `[a] → [a] → [a]`
> 回傳第二個 array 不存在於第一個 array 部分

`[a]`：傳入第一個 array

`[a]`：傳入第二個 array

`[a]`：回傳不存在於第一個 array 部分 array

![without001](/images/ramda/without/without001.png)

### Object

```javascript
import { without } from 'ramda';

let first = [
  { firstName: 'Kevin', lastName: 'Yang'},
];

let second = [
  { firstName: 'Sam',   lastName: 'Xiao'},
  { firstName: 'Kevin', lastName: 'Yang'},
  { firstName: 'Jimmy', lastName: 'Huang'},
]

console.dir(without(first, second));
```

若 `second` array 的 element 為 object，想得知不存在於 `first` array 的 object 有哪些，`without()` 依然適用。

> ECMAScript 的 `===`，對於 object 是 reference equality，但以處理 data 角度而言，這並沒有實質用處，而 `without()` 比較的是 value equality，也就是 property 相等，就視為兩個 object 相等，這在實務上非常有用

![without002](/images/ramda/without/without002.png)

## Conclusion

* Ramda 並沒有如同 `differenceWith()` 提供 `withoutWith()`，但也可組合 `flip()` 與 `differenceWith()` 成為 `withoutWith()`
* `without()` 可輕易找出兩個 array 不重複部分，尤其對於 object 是比較其 value equality，而不是 reference equality，非常實用

## Reference

[Ramda](https://ramdajs.com), [without()](https://ramdajs.com/docs/#without)
[Ramda](https://ramdajs.com), [difference()](https://ramdajs.com/docs/#difference)
[Ramda](https://ramdajs.com), [flip()](https://ramdajs.com/docs/#flip)

