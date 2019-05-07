title: Ramda 之 ap()
tags:
  - Ramda
  - Ramda/ap
  - Ramda/of
  - Ramda/compose
  - Ramda/all
  - Ramda/map
  - Ramda/includes
  - Ramda/identity
feature: images/feature/ramda.png
date: 2019-03-10 11:23:43
---
實務上常遇到一個 Array 中的所有 Function，必須對另外一個 Array 中所有 Data 都執行過一次，此時 Ramda 的 `ap()` 就是我們的好幫手。

<!-- more -->

## Version

VS Code 1.31.1
Quokka 1.0.136
Ramda 0.26.1

## Imperative

```javascript
let hasAllTags = (rules, tags) => {
  let result = true;

  for (let x of rules) {
    result = result && tags.some(y => y === x);
  }

  return result;
};

console.log(hasAllTags(['a', 'd'], ['a', 'e', 'd']));
console.log(hasAllTags(['a', 'd'], ['a', 'e']));
```

條件為 array 的元素必須包含 `a` 與 `d`，若符合條件則回傳 `true`，否則回傳 `false`。

若使用 imperative 寫法，會先定義 `result`，預設為 `true`。

由於 `rules` 是 array，因此必須使用 `for` loop 一一檢查傳進來的 array 是否包含了 `rules` 的每個元素，因此使用了 `Array.prototype` 的 `some()` 判斷。

最後將每個 `result` 判斷 `&&` 起來，回傳結果，這是典型 imperative 作法。

![ap002](/images/ramda/ap/ap002.png)

## ap()

```javascript
import { compose, all, ap, map, includes, of, identity } from 'ramda';

let hasAllTags = rules => compose(
  all(identity),
  ap(map(includes, rules)),
  of
);

console.log(hasAllTags(['a', 'd'])(['a', 'e', 'd']));
console.log(hasAllTags(['a', 'd'])(['a', 'e']));
```

其實由 imperative 作法，已經可以看出一些端倪：

- `&&` 可用 Ramda 的 `all()` 取代
- `some()` 可用 Ramda 的 `includes()` 取代

但 `for`  loop 該用什麼 Ramda function 取代呢 ?

Imperative 作法中，首先必須對 `rules` array 加以展開，執行每個 `includes()`，也就是說：`rules` array 有幾筆，其實 `includes()` 就要執行幾次。

```javascript
map(includes, rules)
```

因此可用 `map()` 對 `rules` 加以展開，成為：

```javascript
[
  includes('a'),
  includes('d'),
]
```

傳統觀念 `map()` 都是用來產生 data，但由於 Ramda function 都有 currying 特性，也此也可以使用 `map()` 配合其他 function 來達成 function array，真正落實 FP 之 `function as data` 理念。

```javascript
ap(map(includes, rules)),
```

且由於每個 `includes()` 都要對傳入的 `tags` 做測試，這剛好符合 Ramda `ap()` 格式。

由 `map()` 產生的 function array，將對所有 data 都執行一次。

> **ap()**
> `[a → b] → [a] → [b]`
>Function array 的所有 function 對 data array 的所有元素都執行一次，並將 function 傳回新 array

`[a -> b]`：以 function 所構成的的 array

`[a]`：要執行 function 的 data，所有元素都會執行 function 一次

`[b]`：function 執行的結果，以 array 構成，其個數為 `function 個數 * 元素個數`

```javascript
of
```

`ap()` 要的是 `人造` 的 array of data，因次必須再使用 Ramda 的 `of()` 先做加工。

> **of()**
> `a -> [a]`
> 將 data 包裝成 array

```javascript
all(identity),
```

最後再使用 `all()` 對 `ap()` 的結果做判斷，取代原本的 `&&`，由於 `all()` 要求的是 function 而非 value，因此要加上 `identity()`，將 data 轉成 function。

> **all()**
> `{a -> Boolean} -> [a] -> Boolean`
> 若所有的 predicate 都成立則回傳 `ture`，否則回傳 `false`

![ap001](/images/ramda/ap/ap001.png)

## Conclusion

* 若要使用 `for loop` 將一堆 function 對 array data 執行時，可先使用 `map()` 將一堆 function 做成 function array，最後再使用 `ap()` 使每個 function 對 data 都執行

* `ap()` 在實務上常要搭配 `of()`，產生人造的 array 才能對 function array 加以排列組合執行


## Reference

[Buzz De Cafe](https://buzzdecafe.github.io), [The Tao of λ: Applicatives, Ramda style](https://buzzdecafe.github.io/code/2014/08/12/applicatives-ramda-style)
[Ramda](https://ramdajs.com), [ap](https://ramdajs.com/docs/#ap)
[Ramda](https://ramdajs.com), [of](https://ramdajs.com/docs/#of)
[Ramda](https://ramdajs.com), [compose](https://ramdajs.com/docs/#compose)
[Ramda](https://ramdajs.com), [all](https://ramdajs.com/docs/#all)
[Ramda](https://ramdajs.com), [map](https://ramdajs.com/docs/#map)
[Ramda](https://ramdajs.com), [includes](https://ramdajs.com/docs/#map)
[Ramda](https://ramdajs.com), [identity](https://ramdajs.com/docs/#map)