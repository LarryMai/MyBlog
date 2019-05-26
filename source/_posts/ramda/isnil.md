title: Ramda 之 isNil()
tags:
  - Ramda
  - Ramda/isNil
  - Ramda/isEmpty
feature: images/feature/ramda.png
date: 2019-05-25 15:48:20
---
由於 ECMAScript 同時存在了 `undefined` 與 `null`，因此實務上常常需要同時判斷這兩個值，Ramda 提供了 `isNil()` 讓我們一次判斷，語意也更清楚。

<!-- more -->

## Version

VS Code 1.34.0
Quokka 1.0.216
Ramda 0.26.1

## isNil()

```javascript
// isNil :: * -> Boolean
let isNil = x => (x === undefined || x === null);

console.log(isNil(undefined));
console.log(isNil(null));
```

傳統要同時判斷 `undefined` 或 `null`，會使用 `||` 搭配 `===` 各自判斷 。

![isnil000](/images/ramda/isnil/isnil000.png)

```javascript
import { isNil } from 'ramda';

console.log(isNil(undefined));
console.log(isNil(null));
```

事實上 Ramda 已經提供 `isNil()`，可直接使用。

> **isNil()**
> `* -> Boolean`
> 判斷是否為  `undefined` 或  `null`

![isnil001](/images/ramda/isnil/isnil001.png)

## Conclusion

- Ramda 的 `isNil()` 算實務上最推薦寫法，不只語意清楚，還一次判斷  `null` 與 `undefined` 兩個 nullable
- 若要連 empty value 也一起判斷，就得同時使用 Ramda 的 `isEmpty() && isNil()`

## Reference

[Ramda](https://ramdajs.com), [isNil()](https://ramdajs.com/docs/#isNil)
[Ramda](https://ramdajs.com), [isEmpty()](https://ramdajs.com/docs/#isEmpty)