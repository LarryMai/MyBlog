title: Ramda 之 unionWith()
tags:
  - Ramda
  - Ramda/unionWith
  - Ramda/concat
  - Ramda/uniqWith
  - Ramda/allPass
  - Ramda/eqProps
feature: images/feature/ramda.png
date: 2019-05-17 13:57:26
---
若想將兩個 Array 加以合併，並只保留不重複部分，Ramda 提供了 `union()`，但若要自訂比較條件，則要使用 `unionWith()`。

<!-- more -->

## Version

VS Code 1.33.1
Quokka 1.0.209
Ramda 0.26.1

## Functional

```javascript
import { uniqWith, concat } from 'ramda';

let first = [1, 3, 5];
let second = [5, 3, 7];

// unionWith :: ((a, a) → Boolean) → [*] → [*] → [*]
let unionWith = pred => arr1 => arr2 => uniqWith(pred, concat(arr1, arr2));

// fn :: [*] -> [*] -> [*]
let fn = unionWith((x, y) => x === y);

console.log(fn(first)(second));
```

`union()` 可透過 `concat()` 與 `uniq()` 組合；同理，`unionWith()` 也可透過 `concat()` 與 `uniqWith()` 組合而成。

![unionwith000](/images/ramda/unionwith/unionwith000.png)

## unionWith()

```javascript
import { unionWith } from 'ramda';

let first = [1, 3, 5];
let second = [5, 3, 7];

// fn :: [*] -> [*] -> [*]
let fn = unionWith((x, y) => x === y);

console.log(fn(first)(second));
```

事實上 Ramda 已經提供 `unionWith()`，可直接使用。

> **unionWith()**
> `((a, a) → Boolean) → [*] → [*] → [*]`
> 自行提供 predicate 決定比較方式，將兩個 array 合併成不重複 array

`(a, a) -> Boolean`：決定判斷相等的 predicate

`[*]`：第一個 array

`[*]`：第二個 array

`[*]`：回傳第一個 array 不存在於第二個 array 部分

![unionwith001](/images/ramda/unionwith/unionwith001.png)

## Object

```javascript
import { unionWith } from 'ramda';

let first = [
  { firstName: 'Sam',   lastName: 'Xiao'},
  { firstName: 'Kevin', lastName: 'Yang'},
  { firstName: 'Jimmy', lastName: 'Huang'},
];

let second = [
  { firstName: 'Kevin', lastName: 'Yang'},
];

// fn :: [*] -> [*] -> [*]
let fn = unionWith((x, y) => x.firstName === y.firstName && x.lastName === y.lastName);

console.dir(fn(first)(second));
```

`unionWith()` 也可以用在 object。

> 由於 `unionWith()` 是使用 `uniqWith()` 實現，而 `uniqWith()` 是使用 `equals()` 比較，而非 ECMAScript 的 `===`，因此比較的是 value equality，而非 reference equality

![unionwith002](/images/ramda/unionwith/unionwith002.png)

## Point-free

```javascript
import { unionWith, allPass, eqProps } from 'ramda';

let first = [
  { firstName: 'Sam',   lastName: 'Xiao'},
  { firstName: 'Kevin', lastName: 'Yang'},
  { firstName: 'Jimmy', lastName: 'Huang'},
];

let second = [
  { firstName: 'Kevin', lastName: 'Yang'},
];

// fn :: [*] -> [*] -> [*]
let fn = unionWith(allPass([
  eqProps('firstName'),
  eqProps('lastName')
]));

console.dir(fn(first)(second));
```

若 predicate 有多個條件，就很適合使用 `allPass()` 使其 point-free，且每個條件又可使用 `eqProps()`，也是 point-free。

![unionwith003](/images/ramda/unionwith/unionwith003.png)

## Conclusion

* 只要牽涉到比較，Ramda 都是使用自家的 `equals()`，而非 ECMAScript 的 `===`，因此是 value equality，而非 reference equality

## Reference

[Ramda](https://ramdajs.com), [unionWith()](https://ramdajs.com/docs/#unionWith)
[Ramda](https://ramdajs.com), [concat()](https://ramdajs.com/docs/#concat)
[Ramda](https://ramdajs.com), [uniqWith()](https://ramdajs.com/docs/#uniqWith)
[Ramda](https://ramdajs.com), [allPass()](https://ramdajs.com/docs/#allPass)
[Ramda](https://ramdajs.com), [eqProps()](https://ramdajs.com/docs/#eqProps)

