title: Maybe 使 ECMAScript 更安全
tags:
  - Crocks
  - Maybe
feature: images/feature/crocks.png
date: 2019-05-13 17:42:16
---
ECMAScript 為 Dynamic Type Language，Function 的 Parameter 並不必指定 Type Hint，因此可以傳入任何 Type，理論上必須在 Runtime 使用 `typeof` 做 Type Check，否則依賴 Type Coercion 很容易產生 Bug；`Mabye` 提供了另外一種方式：只會將正確 Type 進行運算，而不需使用 `typeof` 檢查。

<!-- more -->

## Version

VS Code 1.33.1
Quokka 1.0.212
Croks 0.11.1

## Number

```javascript
// inc :: Number -> Number
let inc = x => x + 1;

// fn :: Number -> Number
let fn = n => inc(n);

console.log(fn(2));
```

很簡單的 `inc()`，當輸入為 `Number` `2` 時，毫無懸念結果為 `3`。

![maybe000](/images/crocks/maybe/maybe000.png)

## String

```javascript
// inc :: Number -> Number
let inc = x => x + 1;

// fn :: Number -> Number
let fn = n => inc(n);

console.log(fn('8'));
```

但傳入改為 `String` `8` 時，結果為 `81`，且 `81` 為 `String`，並不是預期的 `Number`。

> 由於 ECMAScript 為 dynamic type language，因此可以傳入任何 type，而當 `+` 遇到 `String` 時，會將兩個 operand 都轉成 `String`，`+` 從原本的 `add()` 變成 `concat()`，因此最後結果為 `String` `81`

![maybe001](/images/crocks/maybe/maybe001.png)

## Undefined

```javascript
// inc :: Number -> Number
let inc = x => x + 1;

// fn :: Number -> Number
let fn = n => inc(n);

console.log(fn(undefined));
```

當傳入為 `undefined` 時，結果為 `NaN`。

> `81` 與 `NaN` 都不是我們預期結果，要避免這些情形發生，唯一的方法就是確認輸入只能是 `Number` 才能使用 `inc()` 運算，其他 type 都不執行 `inc()`

![maybe002](/images/crocks/maybe/maybe002.png)

## typeof

```javascript
// inc :: Number -> Number
let inc = x => x + 1;

// fn :: Number -> Number
let fn = n => typeof n === 'number' ? inc(n) : 0;

console.log(fn(2));
console.log(fn('8'));
console.log(fn(undefined));
```

為了確保輸入一定是 `Number`，我們當然可以在 `fn()` 加上 `typeof` 判斷，確定是 `Number` 才執行 `inc()`，否則傳回 default 值 `0`。

如此結果就不會是 `String` 或 `NaN` 這些不是我們預期結果。

> 但這種寫法只能算是 short term solution：
>
> * 每個 function 都必須檢查其 parameter type，弄髒了原本邏輯
> * 無法在一般情況就發現錯誤
> * 必須考驗 coder 細心程度，只要不夠細心，unit test 一樣測不到

![maybe003](/images/crocks/maybe/maybe003.png)

## Wrap into Maybe

```javascript
import { Maybe } from 'crocks';

let { Just } = Maybe;

// inc :: Number -> Number
let inc = x => x + 1;

// fn :: Maybe Number -> Maybe Number
let fn = n => n.map(inc);

console.log(fn(Just(2)));
```

引進了 `Maybe`，它包含兩種 type：`Just` 與 `Nothing`：

```javascript
Maybe a = Nothing | Just a
```

* `Just`：我們所預期 type，將會執行運算
* `Nothing`：我們不預期 type，不會執行運算

以本例而言，我們預期 type 是 `Number`，是故為 `Just`；而 `String` 與 `undefined` 都不是我們預期 type，是故為 `Nothing`。

第 1 行

```javascript
import { Maybe } from 'crocks';
```

由 `crocks` import 進 `Maybe`，它提供了我們使用 `Maybe` 所需要的 function。

第 3 行

```javascript
let { Just } = Maybe;
```

從 `Maybe` destructure 出 `Just`，將來不必再使用 `Maybe.Just`。

第 7 行

```javascript
// fn :: Maybe Number -> Maybe Number
let fn = n => n.map(inc);
```

Argument `n` 為 `Maybe`，而 `Maybe` 自帶 `map()`，可傳入 function，`map()` 會幫我們將 `Maybe` 內的 value 透過傳入的 `inc()` 改變之，最後回傳 `Maybe` 。

![maybe004](/images/crocks/maybe/maybe004.png)

由於 `fn()` 回傳為 `Maybe`，因此印出 `Just 3`，而非原本的 `3`。

> 目前我們已經將 `Number` 包進 `Maybe`，但印出也是 `Maybe`，而非原本的` number` `3`，稍後會從 `Maybe` 萃取出來

## Just

```javascript
import { Maybe } from 'crocks';

let { Just } = Maybe;

// inc :: Number -> Number
let inc = x => x + 1;

// fn :: Maybe Number -> Maybe Number
let fn = n => n.map(x => console.log('calling inc()') || inc(x));

console.log(fn(Just(2)));
```

我們雖然將 `inc()` 傳入 `Maybe.map()`，但 `inc()` 真的有被執行嗎 ?

第 8 行

```javascript
// fn :: Maybe Number -> Maybe Number
let fn = n => n.map(x => console.log('calling inc()') || inc(x));
```

特別在 `Maybe.map()` 的 callback 加上 `console.log()`，測試 `inc()` 是否有被執行。

由於 `console.log()` 會回傳 `undefined`，視為 falsy value，因此一定會執行 `||` 右側的 `inc(x)`。

> `console.log()` 配合 `||` 為 ECMAScript 常用測試 callback 手法

![maybe005](/images/crocks/maybe/maybe005.png)

印出了 `calling inc()`，表示若輸入為 `Just`， 真的有執行 `Mapbe.map()` 的 callback。

## Nothing

```javascript
import { Maybe } from 'crocks';

let { Nothing } = Maybe;

// inc :: Number -> Number
let inc = x => x + 1;

// let fn :: Maybe Number -> Maybe Number
let fn = n => n.map(x => console.log('calling inc()') || inc(x));

console.log(fn(Nothing()));
```

`inc()` 與 `fn()` 完全不變，只是改將 `Nothing` 傳入 `fn()`。

![maybe006](/images/crocks/maybe/maybe006.png)

沒顯示 `calling inc()`，表示 `inc()` 根本沒執行，直接回傳 `Nothing`。

> 只有 `Just` 才會執行 `Maybe` 自帶的 function： `map()`，`Nothing` 完全不執行，這確保了只有正確 type 才會執行 `inc()`，不需要我們自己做 `typeof` 檢查，也不會產生不預期結果

## safeNum()

```javascript
import { Maybe } from 'crocks';

let { Just, Nothing } = Maybe;

// inc :: Number -> Number
let inc = x => x + 1;

// safeNum :: x -> Maybe Number
let safeNum = v => typeof v === 'number' ? Just(v) : Nothing();

// fn :: * -> Maybe Number
let fn = n => safeNum(n).map(inc);

console.log(fn(2));
console.log(fn('8'));
console.log(fn(undefined));
```

第 11 行

```javascript
// fn :: * -> Maybe Number
let fn = n => safeNum(n).map(inc);
```

由 `Just` 與 `Nothing` 我們可發現，只要我們能將 value 包進 `Maybe`，儘管不做 `typeof` 檢查，也能確保沒有不預期結果，所以我們需要 `safeNum()` 將任何 value 包進 `Maybe`。

第 8 行

```javascript
// safeNum :: x -> Maybe Number
let safeNum = v => typeof v === 'number' ? Just(v) : Nothing();
```

使用 `typeof` 判斷是否為 `Number`，若是 `Number`，則包成 `Just`，否則包成 `Nothing`。

> Ｑ：到底還是使用了 `typeof` ?

底層雖然會使用 `typeof`，但即將包成 higher order function，請繼續看下去。

![maybe007](/images/crocks/maybe/maybe007.png)

## Higher Order Function

```javascript
import { Maybe } from 'crocks';

let { Just, Nothing } = Maybe;

// inc :: Number -> Number
let inc = x => x + 1

// isNumber :: * -> Boolean
let isNumber = v => typeof v === 'number';

// safe :: (a -> Boolean) -> a -> Maybe a
let safe = pred => v => pred(v) ? Just(v) : Nothing();

// fn :: * -> Maybe Number
let fn = n => safe(isNumber)(n).map(inc);

console.log(fn(2));
console.log(fn('8'));
console.log(fn(undefined));
```

第 8 行

```javascript
// isNumber :: * -> Boolean
let isNumber = v => typeof v === 'number';
```

將 `Number` 寫死在 `safeNum()` 當然不好，可將此部分抽成 `isNumber()`，將來可透過 function compostion 由 `safe(isNumber)` 組合出 `safeNum()`，重複使用程度更高。

11 行

```javascript
// safe :: (a -> Boolean) -> a -> Maybe a
let safe = pred => v => pred(v) ? Just(v) : Nothing();
```

將 `safeNum()` 重構成更一般性的 `safe()` higher order function，可透過傳入 predicate，組合出能將任何 value 包進 `Maybe` 的 function。

14 行

```javascript
// fn :: * -> Number
let fn = n => safe(isNumber)(n).map(inc);
```

由 `safe(isNumber)` 組合出 `safeNum()`，如此可安全將任何 value 包進 `Maybe`。

![maybe008](/images/crocks/maybe/maybe008.png)

## Helper Function

```javascript
import { inc } from 'ramda';
import { safe, isNumber } from 'crocks';

// fn :: * -> Maybe Number
let fn = n => safe(isNumber)(n).map(inc);

console.log(fn(2));
console.log(fn('8'));
console.log(fn(undefined));
```

事實上 Crocks 已經提供了 `safe()` 與 `isNumber()`，可直接使用。

連 `inc()` 也順便使用 Ramda 版本。

> 目前只剩下最後一哩路： `fn()` 回傳 `Maybe`，所以印出結果是錯的，不過也因為能在一般狀況下就發現錯誤，我們很容易修正

![maybe009](/images/crocks/maybe/maybe009.png)

## Unwrap from Maybe

```javascript
import { inc } from 'ramda';
import { safe, isNumber } from 'crocks';

// fn :: * -> Number
let fn = n => safe(isNumber)(n).map(inc).option(0);

console.log(fn(2));
console.log(fn('8'));
console.log(fn(undefined));
```

Crocks 提供了 `Maybe.options()`，讓我們提供當 `Maybe` 為 `Nothing` 時該回傳的 default value，因為 `Nothing` 完全不會經過 `inc()` 運算。

![maybe010](/images/crocks/maybe/maybe010.png)

`fn()` 的 signature 完全沒變，結果也完全沒變。

## Comparison

比較這兩種寫法，我們得到了什麼 ?

### Imperative

```javascript
// fn :: * -> Number
let fn = v => typeof v === 'number' ? inc(v) : 0;
```

我們必須時時刻刻想著要使用 `typeof` 檢查，但儘管粗心沒檢查，`一般情況` 也不會錯，但特殊狀況就會錯，這就是 bug 來源。

### Functional

```javascript
// fn :: * -> Number
let fn = n => safe(isNumber)(n).map(inc).option(0);
```

若使用 FP 的 `Maybe`，思維就不一樣了：

* 先組合出能包進 `Maybe` 的 function，因此會先寫出 `safe(isNumber)`，就算你很粗心也被逼的要寫出來，否則無法繼續
* 接下來要使 `Maybe` 能經過運算，一定得使用 `map()` 傳入 function
* 若很粗心忘記寫 `option()`，一般情況顯示 `Maybe` 就是錯的，一定會回來補上 `option(0)`

也就是每個步驟都被逼著走，且在 `一般情況` 就會發現錯誤，不像 imperative 的 `typeof` 在一般情況不會錯，只有在特殊狀況才會出錯，這就是 bug 來源。

`Maybe` 還可讓你在寫 unit test 時不用想很奇怪的 test case，因為 `Maybe` 會確保沒有不預期結果。

## Conclusion

* `Maybe` 事實上就是 FP 的 Monad，但不知道 Monad 也沒關係
* `Maybe` 在 ECMAScript 的重要性遠超過其他語言，可避免 dynamic type language 產生不預期結果，大幅降低 bug 產生，使 ECMAScript 更安全
* Crocks 已經提供了 `Maybe` 所需要的 helper function，可直接使用

## Reference

[Egghead.io](https://egghead.io), [Understanding the Maybe Data Type](https://egghead.io/lessons/javascript-understand-the-maybe-data-type)
[Egghead.io](https://egghead.io), [Create a Maybe with a Safe Utility Function](https://egghead.io/lessons/javascript-create-a-maybe-with-a-safe-utility-function)
[Egghead.io](https://egghead.io), [Unwrap Values from a Maybe](https://egghead.io/lessons/javascript-unwrap-values-from-a-maybe)
[Gilad Shoham](https://blog.bitsrc.io/@giladshoham), [Get Better Type Checking in JavaScript with the Maybe Type](https://blog.bitsrc.io/get-better-type-checking-in-javascript-with-the-maybe-type-e7f70b23b505)