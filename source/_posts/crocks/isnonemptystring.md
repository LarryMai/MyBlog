title: 自行組合 isNonEmptyString()
tags:
  - Ramda
  - Ramda/isEmpty
  - Ramda/complement
  - Ramda/allPass
  - Ramda/is
  - Crocks
  - Crocks/isString
  - Crocks/and
  - Crocks/not
  - Crocks/isEmpty
feature: images/feature/crocks.png
date: 2019-05-29 18:01:46
---
Ramda 與 Crocks 都沒有提供 `isNonEmptyString()`，但我們可自行組合。

<!-- more -->

## Version

VS Code 1.34.0
Quokka 1.0.216
Ramda 0.26.1
Crocks 0.11.1

## Ramda

```javascript
import { isEmpty, complement, allPass, is } from 'ramda';

// isNonEmptyString :: a -> Boolean
let isNonEmptyString = allPass([
  complement(isEmpty), 
  is(String)
]);

isNonEmptyString(''); // ?
isNonEmptyString('abc'); // ?
```

若使用 Ramda，要判斷 non empty string，有兩個條件：

1. 必須是 string：`complement(isEmpty)`
2. 必須是 non empty：`is(String)`

且兩者都必須成立：`allPass()`。

![nonempty000](/images/crocks/isnonemptystring/nonempty000.png)

## Crocks

```javascript
import { isString, and, not, isEmpty } from 'crocks';

// isNonEmptyString :: a -> Boolean
let isNonEmptyString = and(not(isEmpty), isString);

isNonEmptyString(''); // ?
isNonEmptyString('abc'); // ?
```

若使用 Crocks，判斷條件與 Ramda 一樣，不過使用 function 不太一樣：

1. 必須是 string：Crocks 直接提供 `isString()`，不需用 `is()` 與 `String()` 組合
2. 必須是 non empty：Crocks 也提供了 `isEmpty()`，與 Ramda 完全一樣，而 Crocks 的 `not()` 相當於 Ramda 的 `complement()`，而 `and()` 相當於 `allPass()`

![nonempty001](/images/crocks/isnonemptystring/nonempty001.png)

## Conclusion

* Ramda 與 Crocks 有不少功能是重複的，有些 function 名稱完全一樣，有些則不同
* Crocks 的 `not()` 與 Ramda 的 `complement()` 完全相同；而 Crocks 的 `and()` 也與 Ramda 的 `allPass()` 完全相同
* 若以 API 角度，Crocks 的 `not()` 與 `and()` 語意較佳，因為既然是 Functional Programming，所有的 function 都應該是針對其他 function 而來，而 Ramda 的 `not()` 是針對 value，也就是 `!`；`and()` 也是針對 value，也就是 `&&`，實務上很少使用，還不如學 Crocks 將 `not()` 與 `and()` 直接針對 function

## Reference

[Ramda](https://ramdajs.com), [isEmpty()](https://ramdajs.com/docs/#isEmpty)
[Ramda](https://ramdajs.com), [complement()](https://ramdajs.com/docs/#complement)
[Ramda](https://ramdajs.com), [allPass()](https://ramdajs.com/docs/#allPass)
[Ramda](https://ramdajs.com), [is()](https://ramdajs.com/docs/#is)
[Crocks](https://evilsoft.github.io/crocks/), [isString()](https://evilsoft.github.io/crocks/docs/functions/predicate-functions.html)
[Crocks](https://evilsoft.github.io/crocks/), [and()](https://evilsoft.github.io/crocks/docs/functions/logic-functions.html)
[Crocks](https://evilsoft.github.io/crocks/), [not()](https://evilsoft.github.io/crocks/docs/functions/logic-functions.html)
[Crocks](https://evilsoft.github.io/crocks/), [isEmpty()](https://evilsoft.github.io/crocks/docs/functions/predicate-functions.html)

