title: 使用 alt() 提早處理 Nothing
tags:
  - Crocks
  - Maybe
  - Maybe/alt
feature: images/feature/crocks.png
date: 2019-05-30 17:04:10
---
當使用 `Maybe` 後，`Nothing` 都交由使用端的 `option()` 處理，但有時候我們想在 Function 內提早處理 `Nothing`，此時可使用 `alt()`。

<!-- more -->

## Version

VS Code 1.34.0
Quokka 1.0.216
Ramda 0.26.1
Crocks 0.11.1

## Maybe

```javascript
import { isString, and, not, isEmpty, prop } from 'crocks';
import { compose, join, split, toLower, concat } from 'ramda';

// createUrlSlug :: String -> String
let createSlug = compose(join('-'), split(' '), toLower);

// appendSlug :: String -> String
let appendSlug = concat('https://oomusou.io/crocks/');

// createUrl :: String -> String
let createUrl = compose(appendSlug, createSlug);

let data = {
  title: 'FP in JavaScript',
  price: 100,
};

// fn :: Object -> Maybe String
let fn = obj => prop('title', obj)
  .map(createUrl);

fn(data).option('https://oomusou.io'); // ?
```

13 行

```javascript
let data = {
  title: 'FP in JavaScript',
  price: 100,
};
```

我們希望將 `data.title` 轉成 `https://oomusou.io/crocks/fp-in-javascript` 的 url 格式。

18 行

```javascript
// fn :: Object -> Maybe String
let fn = obj => prop('title', obj)
  .map(createUrl);
```

Crocks 的 `prop()` 回傳為 `Maybe`，使用 `map()` 傳入實際處理 url 為 `createUrl()`。

11 行

```javascript
// createUrl :: String -> String
let createUrl = compose(appendSlug, createSlug);
```

`createUrl()` 組合了 `createSlug()` 與 `appendSlug()`。

第 4 行

```javascript
// createUrlSlug :: String -> String
let createSlug = compose(join('-'), split(' '), toLower);
```

`createSlug()` 組合了 `toLower()`、`split()` 與 `join()`，負責將 `FP in JavaScript` 轉成 `fp-in-javascript`。

第 ７行

```javascript
// appendSlug :: String -> String
let appendSlug = concat('https://oomusou.io/crocks/');
```

`appendSlug()` 將 `fp-in-javascript` 組合成 `https://oomusou.io/crocks/fp-in-javascript`。

22 行

```javascript
fn(data).option('https://oomusou.io'); // ?
```

由於 `fn()` 回傳為 `Maybe`，使用端使用 `option()` 轉回 string，若為 `Nothing`，則顯示 `https://oomusou.io/`。

![alt000](/images/crocks/maybe/alt/alt000.png)

## chain()

```javascript
import { isString, and, not, isEmpty, prop, safe } from 'crocks';
import { compose, join, split, toLower, concat } from 'ramda';

// isNonEmptyString :: a -> Boolean
let isNonEmptyString = and(not(isEmpty), isString);

// createUrlSlug :: String -> String
let createSlug = compose(join('-'), split(' '), toLower);

// appendSlug :: String -> String
let appendSlug = concat('https://oomusou.io/crocks/');

// createUrl :: String -> String
let createUrl = compose(appendSlug, createSlug);

let data = {
  title: '',
  price: 100,
};

// fn :: Object -> Maybe String
let fn = obj => prop('title', obj)
  .chain(safe(isNonEmptyString))
  .map(createUrl);

fn(data).option('https://oomusou.io'); // ?
```

需求改變，希望若 `title` 不存在或者 `title` 為 empty string 時，都顯示成 `https://oomusou.io/crocks/page-not-found` 。

也就是 `Nothing` 不能再由使用端處理，必須在 `fn()` 就處理成 `page-not-found`。

16 行

```javascript
let data = {
  title: '',
  price: 100,
};
```

`data.title` 特別改成 empty string。

21 行

```javascript
// fn :: Object -> Maybe String
let fn = obj => prop('title', obj)
  .chain(safe(isNonEmptyString))
  .map(createUrl);
```

加上了 `safe(isNonEmptyString)()` 判斷是否為 empty string，但由於 `safe()` 也是回傳 `Maybe`，為了避免兩層 `Maybe`，特別使用 `chain()` 攤平，然後繼續原來的 `map(createUrl)`。

![alt001](/images/crocks/maybe/alt/alt001.png)

由於 empty string 為 `Nothing`，最後顯示為 `https://oomusou.io`，但我們需求是顯示 `https://oomusou.io/crocks/page-not-found`，很明顯與需求不合。

## alt()

```javascript
import { isString, and, not, isEmpty, prop, safe, Maybe } from 'crocks';
import { compose, join, split, toLower, concat } from 'ramda';

let { of } = Maybe;

// isNonEmptyString :: a -> Boolean
let isNonEmptyString = and(not(isEmpty), isString);

// createUrlSlug :: String -> String
let createSlug = compose(join('-'), split(' '), toLower);

// appendSlug :: String -> String
let appendSlug = concat('https://oomusou.io/crocks/');

// createUrl :: String -> String
let createUrl = compose(appendSlug, createSlug);

let data = {
  title: '',
  price: 100,
};

// fn :: Object -> Maybe String
let fn = obj => prop('title', obj)
  .chain(safe(isNonEmptyString))
  .alt(of('page-not-found'))
  .map(createUrl);

fn(data).option('https://oomusou.io'); // ?
```

23 行

```javascript
// fn :: Object -> Maybe String
let fn = obj => prop('title', obj)
  .chain(safe(isNonEmptyString))
  .alt(of('page-not-found'))
  .map(createUrl);
```

因為 `chain(safe(isNonEmptyString))` 可能回傳 `Nothing`，很明顯不能再交給使用端處理，而要在 `fn()` 內提早處理 `Nothing`。

使用了 `alt()`，當 `Maybe` 為 `Nothing` 時，使用傳入 `alt()` 的新 `Maybe` 取代，`of()` 會將任意值轉成 `Maybe`，我們即將把 `page-not-found` 塞進 `Maybe`。

最後繼續使用原來的 `map(createUrl)`。

> **alt()**
> `Maybe a ~> Maybe a -> Maybe a`
> 當 `Maybe` 為 `Nothing` 時，以 `alt()` 傳入的 `Maybe` 取代之

`Maybe a`：data 為 `Maybe`

`Maybe a`：當 `Maybe` 為 `Nothing` 時，以新的 `Maybe` 取代

`Maybe b`：回傳新的 `Maybe`

![alt002](/images/crocks/maybe/alt/alt002.png)

使用端寫法完全不變，但已經顯示 `https://oomusou.io/crocks/page-not-found`，因為 `alt()` 在 `fn()` 內提前處理了 `Nothing`。

## Conclusion

* `option()` 與 `alt()` 都能處理 `Noting`，但差異是 `option()` 處理完為 ECMAScript 原生型別；而 `alt()` 處理完還是 `Maybe`
* `alt()` 適合在演算法過程中處理 `Nothing`，而且還要繼續使用 `Maybe`；`option()` 適合使用端最後將 `Maybe` 轉回 ECMAScript 原生型別，順便處理 `Nothing`
* 當在 `Maybe` 中還要加上其他邏輯判斷時，可使用 `chain()` + `alt()` 組合，讓演算法繼續使用 `Maybe`

## Reference

[Andy Van Slaars](https://egghead.io/instructors/andrew-van-slaars), [Recover from a Nothing with the alt() method](https://egghead.io/lessons/javascript-recover-from-a-nothing-with-the-alt-method)
[Crocks](https://evilsoft.github.io/crocks/), [alt()](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html#alt)

