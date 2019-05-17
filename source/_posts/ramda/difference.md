title: Ramda 之 difference()
tags:
  - Ramda
  - Ramda/difference
feature: images/feature/ramda.png
date: 2019-05-17 09:56:34
---
若要找出第一個 Array 哪些 Element 不在第二個 Array，Ramda 提供了 `difference()`， 尤其比較的是 Value 而不是 Reference，非常實用。

<!-- more -->

## Version

VS Code 1.34
Quokka 1.0.213
Ramda 0.26.1


## Primitive

```javascript
import { difference } from 'ramda';

let first = [1, 3, 5, 7];
let second = [1, 4, 7];

console.log(difference(first, second)); 
```

想知道 `first` array 哪些 element 不存在於 `second` array，Ramda 提供了 `difference()`。

> **difference()**
> `[*] → [*] → [*]`
> 回傳第一個 array 不存在於第二個 array 部分

`[*]`：第一個 array

`[*]`：第二個 array

`[*]`：回傳第一個 array 不存在於第二個 array 部分

![diff000](/images/ramda/difference/diff000.png)

## Object

```javascript
import { difference } from 'ramda';

let first = [
  { firstName: 'Sam',   lastName: 'Xiao'},
  { firstName: 'Kevin', lastName: 'Yang'},
  { firstName: 'Jimmy', lastName: 'Huang'},
];

let second = [
  { firstName: 'Kevin', lastName: 'Yang'},
];

console.dir(difference(first, second)); 
```

若 `first` array 的 element 為 object，想得知不存在於 `second` array 的 object 有哪些，`difference()` 依然適用。

> ECMAScript 的 `===`，對於 object 是 reference equality，但以處理 data 角度而言，這並沒有實質用處，而 `difference()` 比較的是 value equality，也就是 property 相等，就視為兩個 object 相等，這在實務上非常有用

![diff001](/images/ramda/difference/diff001.png)


## Conclusion

* `difference()` 可輕易找出兩個 array 不重複部分，尤其對於 object 是比較其 value equality，而不是 reference equality，非常實用

## Reference

[Ramda](https://ramdajs.com), [difference()](https://ramdajs.com/docs/#difference)