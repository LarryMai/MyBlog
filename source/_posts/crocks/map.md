title: 使用 map() 改變 Maybe 內的值
tags:
  - Crocks
  - Maybe
feature: images/feature/crocks.png
date: 2019-05-23 17:06:15
---
當值被包進 `Maybe`，該如何改變 `Maybe` 內的值呢 ? `Maybe` 自帶 `map()`，將原來 Function 傳進 `map()` 即可。

<!-- more -->

## Version

VS Code 1.34.0
Quokka 1.0.216
Ramda 0.26.1
Crocs 0.11.1

## Imperative

```javascript
import { inc, multiply } from 'ramda';

// fn :: Number -> Number
let fn = n => {
  let incremented = inc(n);
  return multiply(2, incremented);
};

console.log(fn(1));
```

`fn()` 要先經過 `inc()`，然後再將結果傳入 `multiply(2)`。

Imperative 會先用變數保存 `inc()` 計算完的資料，再將變數傳入 `multiply()`。

![maybe000](/images/crocks/map/map000.png)

## Functional

```javascript
import { pipe, inc, multiply } from 'ramda';

// fn :: Number -> Number
let fn = pipe(
  inc,
  multiply(2)
);

console.log(fn(1));
```

Functional 會使用 `pipe()` 將 `inc()` 與 `multiply()` 加以組合。

但 ECMAScript 是 dynamic type language，雖然我們希望是 `Number -> Number`，但若傳入 string，結果就是 `NaN`，並不是我們預期結果。

![maybe001](/images/crocks/map/map001.png)

## Maybe

```javascript
import { inc, multiply } from 'ramda';
import { safe, isNumber } from 'crocks';

// fn :: Number -> Number
let fn = n => safe(isNumber)(n)
  .map(inc) 
  .map(multiply(2))
  .option(0);

console.log(fn(1));
```

使用 `safe(isNumber)` 將 `n` 包成 `Maybe` 後，必須使用其自帶的 `map()` 才能改變 `Maybe` 內的值。

由於需經過 `inc()` 與 `multiply()` 運算，因此使用了兩次 `map()`。

最後再使用 `option()` 將 `Maybe` 轉回 number。

![maybe002](/images/crocks/map/map002.png)

```javascript
import { pipe, inc, multiply } from 'ramda';
import { safe, isNumber } from 'crocks';

// fn :: Number -> Number
let fn = pipe(
  inc,
  multiply(2)
);

// fn2 :: Number -> Number
let fn2 = n => safe(isNumber)(n)
  .map(fn)
  .option(0);

console.log(fn2(1));
```

使用兩次 `map()` 雖然可行，但原來的 function 已經被打散了。

事實上我們可以維持原本的 function 不變，只需一次 `map()` 即可。

![maybe003](/images/crocks/map/map003.png)

## Conclusion

* 不用擔心使用 `Maybe` 後，原本的 function 需要打散後再各自 `map()`，事實上可將整個 function 傳進 `map()`，維持原本的 function 完全不用修改

## Reference

[egghead.io](https://egghead.io/), [Unwrap Values from a Maybe](https://egghead.io/lessons/javascript-unwrap-values-from-a-maybe)
[Crocks](https://evilsoft.github.io/crocks/), [Maybe](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html)
[Crocks](https://evilsoft.github.io/crocks/), [map()](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html#map)

