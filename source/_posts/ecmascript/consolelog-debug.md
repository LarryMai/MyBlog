title: 如何優雅地 Debug Callback ?
tags:
  - ECMAScript
feature: images/feature/ecmascript.png
date: 2019-05-06 20:15:22
---
ECMAScript 是大量使用 Callback 的語言，實務上我們常想針對 Callback 加以 `console.log()` 協助 Debug，該如何優雅地使用 `console.log()` 呢 ?

<!-- more -->

## Version

VS Code 1.33.1
Quokka 1.0.209
ECMAScript 2015

## Callback

實務上對於 callback 可能有兩種情境需要 debug：

* 得知 callback 的 parameter 值
* 確認 callback 是否被執行過

## 得知 Callback 的 Parameter 值

```javascript
import { map } from 'ramda';

let data = [1, 2, 3];

let fn = map(x => x * 2);
console.dir(fn(data));
```

在 `map()` 使用 callback，我們想得知 `x` 到底為多少 ?

![log003](/images/ecmascript/consolelog-debug/log003.png)

```javascript
import { map } from 'ramda';

let data = [1, 2, 3];

let fn = map(x => {
  console.log(x);
  return x * 2;
});
console.dir(fn(data));
```

原本 callback 只有 `x => x + 2`，因此不需 `{}` 與 `return`，這是 arrow function 特色。

但為了加上 `console.log()`，就只能加上 `{}` 與 `return` 而打回原形，不能再使用 arrow function。

![log004](/images/ecmascript/consolelog-debug/log004.png)

```javascript
import { map } from 'ramda';

let data = [1, 2, 3];

let fn = map(x => console.log(x) || x * 2);
console.dir(fn(data));
```

可將 `console.log()` 搭配 `||`，接上 `inc()`，如此不需 `{}`，也不需要 `return`，還能繼續使用 arrow function。

因為 `console.log()` 會回傳 `undefined`，視為 falsy value，而 `||` 是 `false` 才會繼續執行，所以一定會繼續執行 `x * 2`。

![log005](/images/ecmascript/consolelog-debug/log005.png)

## 確認 Callback 是否被執行過

```javascript
import { Maybe } from 'crocks';

let inc = n => n + 1;
let getResult = val => val.map(n => inc(n));

let result = getResult(Maybe.Nothing());
console.log(result);
```

當 `getResult()` 傳入 `Nothing` 時，結果也是 `Nothing`，此時對於 `Maybe.map()` 的 callback 是否被執行過，我們會有疑慮。

![log000](/images/ecmascript/consolelog-debug/log000.png)

```javascript
import { Maybe } from 'crocks';

let inc = n => n + 1;
let getResult = val => val.map(n => {
  console.log('Calling inc()');
  return inc(n);
});

let result = getResult(Maybe.Nothing());
console.log(result);
```

原本 callback 只有 `inc()`，因此不需 `{}` 與 `return`，這是 arrow function 特色。

但為了加上 `console.log()`，就只能加上 `{}` 與 `return` 而打回原形，不能再使用 arrow function。

![log001](/images/ecmascript/consolelog-debug/log001.png)

證明傳入 `Nothing` 時，callback 完全沒有執行，因此沒有印出 `Calling inc()`。

> 若看不懂 Maybe 沒關係，本文主要是討論 `console.log()` 的寫法

```javascript
import { Maybe } from 'crocks';

let inc = n => n + 1;
let getResult = val => val.map(n => console.log('Calling inc()') || inc(n));

let result = getResult(Maybe.Nothing());
console.log(result);
```

可將 `console.log()` 搭配 `||`，接上 `inc()`，如此不需 `{}`，也不需要 `return`，還能繼續使用 arrow function。

因為 `console.log()` 會回傳 `undefined`，視為 falsy value，而 `||` 是 `false` 才會繼續執行，所以一定會繼續執行 `inc()`。

![log002](/images/ecmascript/consolelog-debug/log002.png)

## Conclusion

* Arrow function 優美之處就是不需要 `{}` 與 `return`，卻又能優雅地表現語意，一旦加入 `console.log()` debug 後，卻又打回原形；巧妙地善用 `console.log()` 回傳 `undefined` 搭配 `||` 特性，就能繼續優雅的使用 arrow function

## Reference

[Samantha Ming](https://medium.com/@samanthaming), [Quick Debug using || with console.log](https://medium.com/@samanthaming/quick-debug-using-with-console-log-246adf7d8502)

