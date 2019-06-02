title: 自行組合 isNotNil() 同時判斷不是 undefined 與 null
tags:
  - Ramda
  - Ramda/isNil
  - Ramda/isNotNil
  - Ramda/not
  - Ramda/complement
feature: images/feature/ramda.png
date: 2019-05-25 18:13:04
---
Ramda 提供 `isNil()`，可同時判斷 `undefined` 或 `null`，但並沒有提供 `isNotNil()`，但我們可自行組合。

<!-- more -->

## Version

VS Code 1.34.0
Quokka 1.0.216
Ramda 0.26.1

## isNotNil()

```javascript
import { compose, not, isNil } from 'ramda';

// isNotNil :: * -> Boolean
let isNotNil = compose(not, isNil);

console.log(isNotNil(1));
console.log(isNotNil(undefined));
console.log(isNotNil(null));
```

將 `not()` 與 `isNil()` 組合。

> **isNotNil()**
> `* -> Boolean`
> 判斷是否不是 `undefined` 或 `null`

![isnotnil000](/images/ramda/isnotnil/isnotnil000.png)

```javascript
import { complement, isNil } from 'ramda';

// isNotNil :: * -> Boolean
let isNotNil = complement(isNil);

console.log(isNotNil(1));
console.log(isNotNil(undefined));
console.log(isNotNil(null));
```

也可使用 `complement()` 與 `isNil()` 組合。

![isnotnil001](/images/ramda/isnotnil/isnotnil001.png)

## Conclusion

* 兩種方式都可以組合出 `isNotNil()`，可自行挑選喜愛的方式

## Reference

[Ramda](https://ramdajs.com), [isNil()](https://ramdajs.com/docs/#isNil)
[Ramda](https://ramdajs.com), [not()](https://ramdajs.com/docs/#not)
[Ramda](https://ramdajs.com), [complement()](https://ramdajs.com/docs/#complement)




