title: 自行組合 zipFunc()
tags:
  - Ramda
  - Ramda/zipWith
  - Ramda/call
  - Ramda/inc
  - Ramda/dec
  - Ramda/negate
  - Ramda/zipFunc
feature: images/feature/ramda.png
date: 2019-05-14 21:07:02
---
若想將 Array 中一系列 Function，套用到另一 Array，Ramda 並沒有提供適當 Function，此時我們可以自行組合出 `zipFunc()`。

<!-- more -->

## Version

VS Code 1.33.1
Quokka 1.0.212
Ramda 0.26.1

## Imperative

```javascript
import { inc, dec, negate } from 'ramda';

let fns = [inc, dec, negate];
let data = [1, 2, 3];

// zipFunc :: [a] -> [b] -> [c]
let zipFunc = args => arr => {
  let result = [];

  for (let i = 0; i < arr.length; i++) {
    result[i] = args[i](arr[i]);
  }

  return result;
};

console.log(zipFunc(fns)(data));
```

`fns` array 有一系列 function，會依序套用到 `data` array。

`inc()`、`dec()` 與 `negate()` 只是借用 Ramda 的 function，這不是重點，你也可以使用自己的 function。

Imperative 會使用 `for` loop 並搭配 `i` 變數，同時套用 `args` 與 `arr` 兩個 array。

![zipfunc000](/images/ramda/zipfunc/zipfunc000.png)

## zipFunc()

```javascript
import { zipWith, call, inc, dec, negate } from 'ramda';

let fns = [inc, dec, negate];
let data = [1, 2, 3];

// zipFunc :: [a] -> [b] -> [c]
let zipFunc = zipWith(call);

console.log(zipFunc(fns)(data));
```

Ramda 並沒有提供 `zipFunc()`，但我們可以自行組合。

由於要將兩個 array 配對合併成新 array，因此會想到 Ramda 的 `zip` 系列，該系列共有 `zip()`、`zipWith()` 與 `zipObj()`，其中 `zipObj()` 結果為 object，因此不予考慮，而 `zip()` 又過於制式，也無法使用，最後只剩下 `zipWith()`。

> **zipWith()**
> `((a, b) → c) → [a] → [b] → [c]`
> 自行提供 callback 的 `zip()`

`zipＷith()` 原意是要我們傳入兩個 array 經過 callback 運算，但有趣的是目前 function 已經在  array 中了，根本不需要 callback，所以 callback 只剩下一個目的：執行 array 中的 function。

第 6 行

```javascript
// zipFunc :: [a] -> [b] -> [c]
let zipFunc = zipWith(call);
```

因此 `zipWith()` 的 callback 並不是傳入 arrow function，而是傳入 Ramda 的 `call()`，它將會執行 array 中的 function，至於第二個 argument 已經被 point-free，因此不用提供。

> **call()**
> `(*… → a),*… → a`
> 執行第一個 argument 的 function，並將之後的 argument 當作該 function 的 argument list

![zipfunc001](/images/ramda/zipfunc/zipfunc001.png)

## Conclusion

* `call()` 看似不起眼，但搭配 `zipWith()` 卻出現很神奇效果，其實這也正是 FP 精神：將很多小 function 組合成大 function
* FP 強調 function as data，但 function 最終還是要執行，因此 `call()` 是 function 的啟動器

## Reference

[Ramda Cookbook](https://github.com/ramda/ramda/wiki/Cookbook), [Apply a list of functions in a specific order into a list of values](https://github.com/ramda/ramda/wiki/Cookbook#apply-a-list-of-functions-in-a-specific-order-into-a-list-of-values)
[Ramda](https://ramdajs.com), [zipWith()](https://ramdajs.com/docs/#zipWith)
[Ramda](https://ramdajs.com), [call()](https://ramdajs.com/docs/#call)
[Ramda](https://ramdajs.com), [inc()](https://ramdajs.com/docs/#inc)
[Ramda](https://ramdajs.com), [dec()](https://ramdajs.com/docs/#dec)
[Ramda](https://ramdajs.com), [negate()](https://ramdajs.com/docs/#negate)