title: Ramda 之 unapply()
tags:
  - Ramda
  - Ramda/unapply
feature: images/feature/ramda.png
date: 2019-05-14 20:28:26
---
將原本單一 Argument  Function，透過 Ramda 的 `unapply()` 成為多 Argument  Function。

<!-- more -->

## Version

VS Code 1.31.1
Quokka 1.0.212
Ramda 0.26.1

## Function

```javascript
// add :: [a] -> a
let add = arr => arr[0] + arr[1] + arr[2];

console.log(add([1, 2, 3]));
```

`add()` 為普通 function，只有 1 個 array 為 argument。

![unapply000](/images/ramda/unapply/unapply000.png)

## unapply()

```javascript
import { unapply } from 'ramda';

// add :: [a] -> a
let add = arr => arr[0] + arr[1] + arr[2];

// fn :: (*...) -> a
let fn = unapply(add);

console.log(fn(1, 2, 3));
```

若想將原本 `add()` 的 argument 從 1 個 array，變成 3 個 argument，可使用 Ramda 的 `unapply()` 加以轉換。

> **unapply()**
> `([*…] → a) → (*… → a)`
> 使單一 argument function 成為多 argument function

`([*…] → a)`：原本以 array 為單一 argument function

`(*… → a)`：回傳多 argument function

![unapply001](/images/ramda/unapply/unapply001.png)

## Conclusion

* `apply()` 因為有 `a`，所以 argument 為 array；而 `unapply()` 因為沒有 `a`，所以 argument 不是 array，而是多 argument，這樣聯想可以幫助記憶
* `unapply()` 並沒有提供 currying，須自行再使用 `curry()` 

## Reference

[Ramda](https://ramdajs.com), [unapply()](https://ramdajs.com/docs/#unapply)