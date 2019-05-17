title: Ramda 之 differenceWith()
tags:
  - Ramda
  - Ramda/differenceWith
  - Ramda/equals
  - Ramda/allPass
  - Ramda/eqProps
feature: images/feature/ramda.png
date: 2019-05-17 10:37:42
---
若要找出第一個 Array 哪些 Element 不在第二個 Array，Ramda 提供了 `difference()`，但若要自訂比較條件，則要使用 `differenceWith()`。

<!-- more -->

## Version

VS Code 1.334
Quokka 1.0.213
Ramda 0.26.1

## Primitive

```javascript
import { differenceWith } from 'ramda';

let first = [1, 3, 5, 7];
let second = [1, 4, 7];

// fn :: [a] -> [a] -> [a]
let fn = differenceWith((x, y) => x === y);

console.log(fn(first, second));
```

`difference()` 提供的是內定的比較方式，而 `differenceWith()` 可提供自訂的 predicate。

> **differenceWith()**
> `((a, a) → Boolean) → [a] → [a] → [a]`
> 自行提供 predicate 決定比較方式，回傳第一個 array 不存在於第二個 array 部分

`((a, a) → Boolean)`：決定判斷相等的 predicate

`[a]`：第一個 array

`[a]`：第二個 array

`[a]`：回傳第一個 array 不存在於第二個 array 部分

![diff002](/images/ramda/differencewith/diff002.png)

## Point-free

```javascript
import { differenceWith, equals } from 'ramda';

let first = [1, 3, 5, 7];
let second = [7, 1, 4];

// fn :: [a] -> [a] -> [a]
let fn = differenceWith(equals);

console.log(fn(first, second));
```

`differnceWith()` 的 predicate，也可以使用 `equals()` 使其 point-free。

![diff003](/images/ramda/differencewith/diff003.png)

## Object

```javascript
import { differenceWith } from 'ramda';

let first = [
  { firstName: 'Sam',   lastName: 'Xiao'},
  { firstName: 'Kevin', lastName: 'Yang'},
  { firstName: 'Jimmy', lastName: 'Huang'},
];

let second = [
  { firstName: 'Kevin', lastName: 'Yang'},
];

// fn :: [a] -> [a] -> [a]
let fn = differenceWith((x, y) => x.firstName === y.firstName && x.lastName === y.lastName);

console.dir(fn(first, second));
```

`difference()` 也可以搭配 `differenceWith()` 使用自己的 predicate，此例完全與 `difference()` 相同。

> `differenceWith()` 可貴之處在於自訂 predicate，如只比較 `lastName` 即可，不用比較 `firstName`，若沒有特別之處，使用 `difference()` 即可

![diff004](/images/ramda/differencewith/diff004.png)

## Point-free

```javascript
import { differenceWith, allPass, eqProps } from 'ramda';

let first = [
  { firstName: 'Sam',   lastName: 'Xiao'},
  { firstName: 'Kevin', lastName: 'Yang'},
  { firstName: 'Jimmy', lastName: 'Huang'},
];

let second = [
  { firstName: 'Kevin', lastName: 'Yang'},
];

// fn :: [a] -> [a] -> [a]
let fn = differenceWith(allPass([
  eqProps('firstName'),
  eqProps('lastName')
]));

console.dir(fn(first, second));
```

若 predicate 有多個條件，就很適合使用 `allPass()` 使其 point-free，且每個條件又可使用 `eqProps()`，也是 point-free。

![diff005](/images/ramda/differencewith/diff005.png)

## Conclusion

- `differenceWith()` 可輕易找出兩個 array 不重複部分，尤其對於 object 是比較其 value equality，而不是 reference equality，非常實用
- `differenceWith()` 對於 object，可使用 `allPass()` 與 `eqProps()` 使其 point-free，非常經典

## Reference

[Ramda](https://ramdajs.com), [differenceWith()](https://ramdajs.com/docs/#differenceWith)
[Ramda](https://ramdajs.com), [equals()](https://ramdajs.com/docs/#equals)
[Ramda](https://ramdajs.com), [allPass()](https://ramdajs.com/docs/#allPass)
[Ramda](https://ramdajs.com), [eqProps()](https://ramdajs.com/docs/#eqProps)

