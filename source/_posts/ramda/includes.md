title: Ramda 之 includes()
tags:
  - Ramda
  - Ramda/includes
  - Ramda/any
feature: images/feature/ramda.png
date: 2019-05-14 19:52:21
---
實務上我們常需判斷某一個值是否存在於 Array 內，若存在則傳回 `true`，若不存在則傳回 `false`，對於簡單的需求，我們會希望不要傳入 Callback，直接傳入 Data 即可。

<!-- more -->

## Version

VS Code 1.31.1
Quokka 1.0.212
Ramda 0.26.1

## Imperative

```javascript
let data = [1, 2, 3];

// includes :: a -> [a] -> Boolean
let includes = arg => arr => {
  for(let elem of arr) {
    if (elem === arg) return true;
  }
  return false;
}

console.log(includes(1)(data));
```

建立 `includes()`，imperative 會使用 `for` loop 搭配 `if` 判斷，若找到就直接回傳  `true` 結束，若都找不到則回傳 `false`。

![includes000](/images/ramda/includes/includes000.png)

## Array.prototype.includes()

```javascript
let data = [1, 2, 3];

console.log(data.includes(1));
```

`Array.prototype` 有內建 `includes()`，直接傳入 data 即可。

> 就功能而言兩者都一樣，但 `Array.prototype.includes()` 屬 OOP 風格，為 data 的 method，而 `includes()` 為 FP 風格，data 以 argument 傳入 function，且為最後一個 argument，方便 point-free

![includes001](/images/ramda/includes/includes001.png)

## Functional

```javascript
import { any } from 'ramda';

let data = [1, 2, 3];

// includes :: a -> [a] -> Boolean
let includes = arg => any(x => x === arg);

console.log(includes(1)(data));
```

其實我們大可不必自己寫 `for` loop，可藉助 Ramda 的 `any()` 組合出 `includes()`。

> **any()**
> `(a -> Boolean) -> [a] -> Boolean`
>
> 判斷符合條件的 data 是否存在於 array 

![includes002](/images/ramda/includes/includes002.png)

## Primitive

```javascript
import { includes } from 'ramda';

let data = [1, 2, 3];

console.log(includes(1)(data));
```

事實上 Ramda 已經提供 `includes()`，可直接使用。

> **includes()**
> `a → [a] → Boolean`
> 判斷 data 是否存在於 array

`a`： 欲判斷的 data

`[a]`：data 為 array

`Boolean`：回傳比較結果

![includes003](/images/ramda/includes/includes003.png)

## Object

```javascript
import { includes } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

console.log(includes({ title: 'FP in JavaScript', price: 100 })(data));
```

`includes()` 也可判斷 object 是否存在於 array，值得注意的是 `includes()` 使用 `equals()` 判斷，而非 `===`，比較的是 object property，而非 object reference。

![includes004](/images/ramda/includes/includes004.png)

```javascript
import { includes } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

console.log(data.includes({ title: 'FP in JavaScript', price: 100 }));
```

若使用 `Array.prototype.includes()`，比較的是 object reference，明顯與 `includes()` 不同。

> 對於 primitive，`includes()` 與 `Array.prototype.includes()` 完全相同；但對於 object，則兩者不同

![includes005](/images/ramda/includes/includes005.png)

## String

```javascript
import { includes } from 'ramda';

let data = 'FP in JavaScript';

console.log(includes('JavaScript', data));
```

`includes()` 除了用在 array，也可以用在 `string`。

![includes006](/images/ramda/includes/includes006.png)

```javascript
let data = 'FP in JavaScript';

console.log(data.includes('JavaScript'));
```

若用在 `string`，則與 `String.prototype.includes()` 完全相同。

![includes007](/images/ramda/includes/includes007.png)

## Conclusion

* `includes()` 與 `any()` 非常接近，唯 `includes()` 提供 data，而 `any()` 提供 callback
* `includes()` 可視為簡易版；而 `any()` 為進階版 `includes()` 
* `includes()` 使用 `equals()` 判斷 `object`，比較的是 object property，而非 object reference，與 `array.prototype.includes()` 不同
* `includes()` 也能用在 `string`，相當於 `array.prototype.includes()` 與 `string.prototype.includes()` 合體

## Reference

[Ramda](https://ramdajs.com), [includes()](https://ramdajs.com/docs/#includes)
[Ramda](https://ramdajs.com), [any()](https://ramdajs.com/docs/#any)