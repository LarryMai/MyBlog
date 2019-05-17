title: Ramda 之 eqBy()
tags:
  - Ramda
  - Ramda/eqBy
  - Ramda/uniqWith
feature: images/feature/ramda.png
date: 2019-05-14 18:36:07
---
Ramda 有些 Function 很少直接使用，而是用來產生 Predicate，`eqBy()` 就是其中之一。

<!-- more -->

## eqBy()

> **eqBy()**
> `(a → b) → a → a → Boolean`
>
> 若兩 value 經過 function 轉換後，使用 `equals()` 比較後相等，則回傳 `true`，否則回傳 `false`

`(a -> b)`：傳入要轉換的 function

`a`：要比較的 value

`a`：要比較的 value

`Boolean`：轉換後經過 `equals()` 比較，相等則回傳 `true`，否則回傳 `false`

```javascript
var eqBy = _curry3(function eqBy(f, x, y) {
  return equals(f(x), f(y));
});
export default eqBy;
```

`eqBy()` 若由官網解釋，可能很難了解，但若由其 source code 則很容易體會。

不過 `eqBy()` 通常很少直接拿來使用，而是用在產生其他 function 的 predicate。

## Predicate

```javascript
import { uniqWith } from 'ramda';

let data = [1, 2, 3, -1];

// fn :: [a] -> [a]
let fn = uniqWith((x, y) => Math.abs(x) === Math.abs(y));

console.log(fn(data));
```

`data` 中包含 `1` 與 `-1`，若`1` 與  `-1` 均視為相同而不想重複顯示，也就是我們希望結果只顯示 `[1, 2, 3]`。

若使用 `uniqWith()`，我們會提供 predicate： `(x, y) => Math.abs(x) === Math.abs(y)`。

![eqby000](/images/ramda/eqby/eqby000.png)

## Point-free

```javascript
import { uniqWith, eqBy } from 'ramda';

let data = [1, 2, 3, -1];

// fn :: [a] -> [a]
let fn = uniqWith(eqBy(Math.abs));

console.log(fn(data));
```

由於 `x` 與 `y` 同時經過 `Math.abs()` 運算，因此適合使用 `eqBy(Math.abs)` 產生 predicate 使其 point-free。

![eqby001](/images/ramda/eqby/eqby001.png)

## Conclusion

* `eqBy()` 通常不會直接使用，而是用來產生 predicate
* `eqBy()` 官網說明有點難懂，但直接去看 source code 則一目瞭然

## Reference

[Ramda](https://ramdajs.com), [eqBy()](https://ramdajs.com/docs/#eqBy)
[Ramda](https://ramdajs.com), [uniqWith()](https://ramdajs.com/docs/#uniqWith)