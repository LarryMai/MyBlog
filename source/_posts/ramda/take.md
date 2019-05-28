title: Ramda 之 take()
tags:
  - Ramda
  - Ramda/take
  - Ramda/slice
feature: images/feature/ramda.png
date: 2019-05-28 16:48:10
---
若只想取 Array 的前幾筆資料，Ramda 提供了 `take()`。

<!-- more -->

## Version

VS Code 1.34.0
Quokka 1.0.217
Ramda 0.26.1

## Imperative

```javascript
let data = [1, 2, 3];

// take :: Number -> [a] -> [a]
let take = count => arr => {
  let result = [];
  let index = 0;

  for(let elem of arr) {
    if (index < count) {
      result = [...result, elem];
    }
    else {
      return result;
    }
    index++;
  }
};

console.log(take(2)(data));
```

建立 `take()`，imperative 會使用 `for` loop，並先建立好回傳的 `result` array，當 `index` 小於 `count` 時，`elem` 會塞進回 `result`，當大於等於 `count` 時，則回傳 `result` 結束執行。

![take000](/images/ramda/take/take000.png)


## Functional

```javascript
import { slice } from 'ramda';

let data = [1, 2, 3];

// take :: Number -> [a] -> [a]
let take = count => slice(0, count);

console.log(take(2)(data));
```

要從既有的 array 回傳部分 array，第一個想到的就是 `slice()`，因此 `take()` 也可使用 `slice()` 實現。

![take001](/images/ramda/take/take001.png)

## Primitive

```javascript
import { take } from 'ramda';

let data = [1, 2, 3];

console.log(take(2)(data));
```

事實上 Ramda 已經內建 `take()`，可直接使用。

> **take()**
> `Number -> [a] -> [a]`
> 取得 array 中的前 n 筆資料

`Number`：前 n 筆

`[a]`：data 為 array

`[a]`：回傳前 n 筆資料

![take002](/images/ramda/take/take002.png)

## Object

```javascript
import { take } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

console.dir(take(2)(data));
```

`take()` 也可以用在 object。

![take003](/images/ramda/take/take003.png)

## String

```javascript
import { take } from 'ramda';

let data = 'JavaScript';

console.log(take(4)(data));
```

`take()` 除了用在 array，也可以用在 string。

> **take()**
> `Number -> String -> String`
> 取得 string 中的前 n 個 character 的 string

`Number`：前 n 個 character

`string`：data 為 string

`string`：回傳前 n 個 character 的 string

![take004](/images/ramda/take/take004.png)

## Conclusion

* `take()` 也可使用 `slice()` 實現
* `take()` 不只用在 array，也可於用於 string

## Reference

[Ramda](https://ramdajs.com), [slice()](https://ramdajs.com/docs/#slice)
[Ramda](https://ramdajs.com), [take()](https://ramdajs.com/docs/#take)

