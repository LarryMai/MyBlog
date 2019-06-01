title: 使用 coalesce() 提早提供 Function 處理 Nothing
tags:
  - Crocks
  - Maybe
  - Maybe/coalesce
  - Maybe/alt
feature: images/feature/crocks.png
date: 2019-05-31 17:04:56
---
在 [使用 alt() 提早處理 Nothing](/crocks/maybe/alt/) 一文中，我們在 `fn()` 使用了 `alt()` 處理 `Nothing`，但眼尖讀者會發現我們竟然 Hardcode String，實務上使用 Function 的機會更多，此時可使用 `coalensce()`。

<!-- more -->

## Version

VS Code 1.34.0
Quokka 1.0.216
Ramda 0.26.1
Crocks 0.11.1

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

使用了 `chain()` 做判斷，當 `Maybe` 為 `Nothing` 時，則 `Maybe` 內改由 `page-of-found` 取代。

若使用端可對 `fn()` 傳入 `true` 或 `false`，當 `true` 時為 `page-not-found`，`false` 時為 `url-not-found`，該如何改寫呢 ?

![coalesce000](/images/crocks/maybe/coalesce/coalesce000.png)

## coalesce()

```javascript
import { isString, and, not, isEmpty, prop, safe, Maybe } from 'crocks';
import { compose, join, split, toLower, concat, identity, always } from 'ramda';

let { of } = Maybe;

// isNonEmptyString :: a -> Boolean
let isNonEmptyString = and(not(isEmpty), isString);

// createUrlSlug :: String -> String
let createSlug = compose(join('-'), split(' '), toLower);

// appendSlug :: String -> String
let appendSlug = concat('https://oomusou.io/crocks/');

// defaultUrl :: Boolean -> (* -> String)
let defaultUrl = flag => flag ? 
  always('page-not-found') : 
  always('url-not-found');

// createUrl :: String -> String
let createUrl = compose(appendSlug, createSlug);

let data = {
  title: '',
  price: 100,
};

// fn :: Object -> Boolean -> Maybe String
let fn = obj => flag => prop('title', obj)
  .chain(safe(isNonEmptyString))
  .coalesce(defaultUrl(flag), identity)
  .map(createUrl);

fn(data)(true).option('https://oomusou.io'); // ?
```

28 行

```javascript
// fn :: Object -> Boolean -> Maybe String
let fn = obj => flag => prop('title', obj)
  .chain(safe(isNonEmptyString))
  .coalesce(defaultUrl(flag), identity)
  .map(createUrl);
```

將 `alt()` 改成 `coalesce()`。

> **coalesce()**
> `Maybe a ~> ((() -> b), (a -> b))) -> Maybe b`
> 傳入兩個 function，當 `Nothing` 時執行第一個 function，`Just` 時執行第二個 function

`Maybe a`：data 為 `Maybe`

`((() -> b), (a -> b)))`：傳入兩個 function，當 `Nothing` 時執行 `() -> b`；當 `Just` 時執行 `a -> b`。

`Maybe b`：回傳新的 `Maybe`

與 `alt()` 比較：

> **alt()**
> `Maybe a ~> Maybe a -> Maybe a`
> 當 `Maybe` 為 `Nothing` 時，以 `alt()` 傳入的 `Maybe` 取代之

`alt()` 要求你傳入 `Maybe`，因此需使用 `of()` 轉成 `Maybe`。

但 `coalesce()` 則不必，只要提供 `() -> b` 與 `a -> b` 兩個 funtion 即可，會自動轉成 `Maybe b`。

```javascript
.coalesce(defaultUrl(flag), identity)
```

當 `Nothing` 時執行 `defaultUrl(flag)` 回傳新的 `Maybe`。

當 `Just` 時不變，因此傳入 Ramda 的 `identity()`。

15 行

```javascript
// defaultUrl :: Boolean -> (* -> String)
let defaultUrl = flag => flag ? 
  always('page-not-found') : 
  always('url-not-found');
```

`coalesce()` 要求傳入 function，因此 `page-not-found` 與 `url-not-found` 要透過 Ramda 的 `always()` 包成 function 回傳。

![coalesce001](/images/crocks/maybe/coalesce/coalesce001.png)

## Conclusion

* `alt()` 與 `coalesce()` 功能相同，都是在 function 內提早處理 `Nothing`
* `alt()` 直接傳入 `Maybe`；而 `coalesce()` 則要求你傳入兩個 function，當 `Nothing` 時執行第一個 function，`Just` 時執行第二個 function

## Reference

[Andy Van Slaars](https://egghead.io/instructors/andrew-van-slaars), [Recover from a Nothing with the coalesce() Method](https://egghead.io/lessons/javascript-recover-from-a-nothing-with-the-coalesce-method)
[Crocks](https://evilsoft.github.io/crocks/), [alt()](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html#alt)
[Crocks](https://evilsoft.github.io/crocks/), [coalesce()](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html#coalesce)
