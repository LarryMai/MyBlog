title: 使用 isEmpty() 判斷 Empty Value
tags:
  - Ramda
  - Ramda/isEmpty
  - Ramda/empty
  - Maybe
feature: images/feature/ramda.png
date: 2019-05-25 19:13:57
---
根據 Ramda 的 `empty()` 判斷是否為 empty value。

<!-- more -->

## Version

VS Code 1.33.0
Quokka 1.0.205
Ramda 0.26.1
Croks 0.11.1

## isEmpty()

```javascript
import { isEmpty } from 'ramda';
import { Maybe } from 'crocks';

let { Just, Nothing } = Maybe;

console.log(isEmpty(''));
console.log(isEmpty({}));
console.log(isEmpty([]));

console.log(isEmpty(undefined));
console.log(isEmpty(null));

console.log(isEmpty(Just(4)));
console.log(isEmpty(Nothing()))
```

> **isEmpty()**
> `a -> Boolean`
> 判斷任意值是否為 empty

`a`：data 為任意型別

`Boolean`：回傳為 `true` 或 `false`

* 只要是 empty string、`{}` 或 `[]`，`isEmpty()` 都會回傳 `true`
* 但 `undefined` 與 `null` 則回傳 `false`，這兩個 nullable 要靠 `isNil()` 判斷
* `Just` 與 `Nothing` 都回傳 `false`， `Maybe` 已經超出 `isEmpty()` 判斷能力

![isEmpty000](/images/ramda/isempty/isEmpty000.png)

## Conclusion

* `isEmpty()` 無法判斷 `0`，因為 `isEmpty()` 使用 `equals(x, empty(x))` 判斷，而 `empty(1)` 為 `undefined` 而不是 `0`
* `isEmpty()` 無法判斷 `undefined` 與 `null` ，都回傳 `false`
* `isEmpty()` 無法判斷 `Maybe`，都回傳 `false`

## Reference

[Ramda](https://ramdajs.com), [isEmpty()](https://ramdajs.com/docs/#isEmpty)
[Ramda](https://ramdajs.com), [empty()](https://ramdajs.com/docs/#empty)
[Crocks](https://evilsoft.github.io/crocks/), [Maybe](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html)

