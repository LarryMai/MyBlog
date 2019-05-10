title: Ramda 之 remove()
tags:
  - Ramda
  - Ramda/remove
  - Ramda/slice
  - Ramda/without
feature: images/feature/ramda.png
date: 2019-05-02 11:23:43
---
若要從 Array 擷取其中一部分，但我們無法如 `slice()` 指定要的部分，只能指定不要的部分，此時我們可使用 Ramda 的 `remove()`。

<!-- more -->

## Functional

```javascript
import { slice, without } from 'ramda';

let data = [1, 2, 3, 4, 5];

let remove = (start, count, arr) => without(slice(start, start + count, arr), arr);
console.dir(remove(1, 3, data));
```

假如我們一開始不知道 Ramda 的 `remove()`，也可透過 `slice()` 與 `without()` 組合：

* 先使用 `slice()` 找出不要的部分
* 再使用 `without()` 排除不要的部分

![remove000](/images/ramda/remove/remove000.png)

## remove()

### Primitive

```javascript
import { remove } from 'ramda';

let data = [1, 2, 3, 4, 5];
console.dir(remove(1, 3, data));
```

事實上 Ramda 已經內建 `remove()`，可直接使用。

> **remove()**
> `Number → Number → [a] → [a]`
>
> 直接指定 Array 中不要的部分擷取部分 Array

`Number`：Array 的 start index

`Number`：Array 的 element count

`[a]`：Data 為 array

`[a]`：回傳所截取的 array

![remove002](/images/ramda/remove/remove002.png)

### Object

```javascript
import { remove } from 'ramda';

let data = [
  { firstName: 'Sam',    lastName: 'Xiao'},
  { firstName: 'Kevin',  lastName: 'Yang'},
  { firstName: 'Jimmy',  lastName: 'Huang'},
  { firstName: 'Jessie', lastName: 'Hsieh'},
  { firstName: 'John',   lastName: 'Wu'}
];

console.dir(remove(1, 3, data));
```

若 element 為 object，`remove()` 依然適用。

![remove001](/images/ramda/remove/remove001.png)

## Conclusion

* `remove()` 是與 `slice()` 相對的 function：`slice()` 是指定要的部分，而 `remove()` 是指定不要的部分

## Reference

[Ramda](https://ramdajs.com), [remove()](https://ramdajs.com/docs/#remove)
[Ramda](https://ramdajs.com), [slice()](https://ramdajs.com/docs/#slice)
[Ramda](https://ramdajs.com), [without()](https://ramdajs.com/docs/#without)