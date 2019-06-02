title: 使用 slice() 指定 Array 中要的部分
tags:
  - Ramda
  - Ramda/slice
feature: images/feature/ramda.png
date: 2019-06-02 15:35:39
---
Ramda 也如原生 ECMAScript 提供 `slice()`，唯 Ramda 的 `slice()` 是以 FP 形式呈現。

<!-- more -->

## Version

VS Code 1.33.0
Quokka 1.0.205
Ramda 0.26.1

## Imperative

```javascript
let data = [1, 2, 3, 4, 5];

// slice :: Number -> Number -> [a] -> [a]
let slice = begin => end => arr => {
  let result = [];
  let index = 0;

  for(let elem of arr) {
    if (begin <= index && index < end) {
      result = [...result, elem];
    }

    index++;
  }

  return result;
};

slice(1)(3)(data); // ?
```

建立 `slice()`，imperative 會先建立欲回傳的 `result` array，使用 `for` loop 搭配 `if` 判斷，若 `index` 在傳進的 `begin` 與 `end` 之中，則塞進 `result` 中。

![slice008](/images/ramda/slice/slice008.png)

## Array.prototype.slice()

```javascript
let data = [1, 2, 3, 4, 5];

data.slice(1, 3); // ?
```

ECMAScript 原生的 `array.prototype` 已內建 `slice()`，可直接使用。

![slice009](/images/ramda/slice/slice009.png)

## Primitive

```javascript
import { slice } from 'ramda';

let data = [1, 2, 3, 4, 5];

slice(1)(3)(data); // ?
```

Ramda 亦內建 `slice()`，可直接使用。

> **slice()**
> `Number -> Number -> [a] -> [a]`
> 回傳 array 的一部分

`Number`：array 的起始 index

`Number`：array 的結束 index (但不包含)

`[a]`：data 為 array

`[a]`：回傳新的部分 array

![slice000](/images/ramda/slice/slice000.png)

```javascript
import { slice } from 'ramda';

let data = [1, 2, 3, 4, 5];

slice(1)(Infinity)(data); // ?
```

若要回傳到 array 最後一個 element，直接傳入 `Infinity` 即可。

> 若要回傳到 array 最後一個 element，在 `Array.prototype.slice()` 只要省略不傳即可，因為 ECMAScript 支援 optional parameter，預設就是最後一個 element；但 Ramda 不支援 optional parameter，而支援 partial application，因此一定要明確傳入 `Infinity`

![slice001](/images/ramda/slice/slice001.png)

```javascript
import { slice } from 'ramda';

let data = [1, 2, 3, 4, 5];

slice(1)(-2)(data); // ?
```

Index 也可以傳入負數，其中 `-1` 就是最後一個 element，`-2` 就是倒數第二個 element，以此類推。

> `Array.prototype.slice()` 的 index 也支援負數

![slice002](/images/ramda/slice/slice002.png)

```javascript
import { slice } from 'ramda';

let data = [1, 2, 3, 4, 5];

console.log(slice(-3, -1)(data));
```

也可以兩個參數都是負數。

![slice003](/images/ramda/slice/slice003.png)

## Object

```javascript
import { slice } from 'ramda';

let data = [
  { title: "FP in JavaScript", price: 100 },
  { title: "RxJS in Action", price: 200 },
  { title: "Speaking JavaScript", price: 300 }
];


console.dir(slice(1)(3)(data));
```

`slice()` 亦可用於 object。

![slice010](/images/ramda/slice/slice010.png)

## String

```javascript
import { slice } from 'ramda';

let data = 'Hello';

slice(1)(3)(data); // ?
```

`slice()` 除了可用在 array 外，還可以用在 string，相當於 `String.prototype.substring()`。

> **slice()**
> `Number -> Number -> String -> String`
> 回傳 string 的一部分

`Number`：string 的起始 index

`Number`：string 的結束 index (但不包含)

`String`：data 為 string

`String`：回傳新的部分 string

![slice004](/images/ramda/slice/slice004.png)

```javascript
import { slice } from 'ramda';

let data = 'Hello';

slice(1)(Infinity)(data); // ?
```

與 array 一樣，若要回傳到 string 最後一個字元，第二個參數要傳 `Infinity`。

![slice005](/images/ramda/slice/slice005.png)

```javascript
import { slice } from 'ramda';

let data = 'Hello';

slice(1)(-2)(data); // ?
```

與 array 一樣，index 也可以傳入負數。

![slice006](/images/ramda/slice/slice006.png)

```javascript
import { slice } from 'ramda';

let data = 'Hello';

slice(-3)(-1)(data); // ?
```

與 array 一樣，兩個參數都能傳入負數。

![slice007](/images/ramda/slice/slice007.png)

## Conclusion

* `slice()` 底層也是呼叫 `Array.prototype.slice()`，因此所有特性與原生的 `slice()` 相同，唯原生的 `slice()` 是以 OOP 設計，`slice()` 為 array 的 method；而 Ramda 的 `slice()` 是以 FP 設計，array 以參數傳入，且在最後一個參數，方便做 point-free
* 由於 Ramda 不支援 optional parameter，而支援 partial application，因此第二個參數不能省略，若要抓到最後一個 element，要明確傳入 `Infinity`
* `slice()` 若用於 string，相當於 `String.prototype.substring()`

## Reference

[Ramda](https://ramdajs.com), [slice()](https://ramdajs.com/docs/#slice)

