title: Maybe 也能使用 Function Composition
tags:
  - Crocks
  - Maybe
  - Maybe/alt
  - Maybe/chain
  - Maybe/option
  - Maybe/coalesce
feature: images/feature/crocks.png
date: 2019-06-01 11:29:06
---
之前都使用 `Maybe` 自帶的 Method，以 OOP 的 Method Chaining 處理 `Maybe`，事實上也能完全以 FP 的 Function Composition 處理。

<!-- more -->

## Version

VS Code 1.34.0
Quokka 1.0.216
Ramda 0.26.1
Crocks 0.11.1

## OOP

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

在 [使用 alt() 提早處理 Nothing](/crocks/maybe/alt/) 一文中，由於 `prop()` 回傳 `Maybe`，我們使用了一系列的 `chain()`、`alt()`、`map()` 處理 `Maybe`，這些都是 `Maybe` 自帶 method，呈現出 OOP 風格的 method chaining，我們能否使用 FP 的 function composition 處理 `Maybe` 呢 ?

![compose000](/images/crocks/maybe/function-composition/compose000.png)

## FP

```javascript
import { isString, and, not, isEmpty, prop, safe, Maybe, chain, alt, map, option } from 'crocks';
import { compose, join, split, toLower, concat, pipe } from 'ramda';

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
let fn = pipe(
  prop('title'),
  chain(safe(isNonEmptyString)),
  alt(of('page-not-found')),
  map(createUrl)
);

option('https://oomusou.io')(fn(data)); // ?
```

第 1 行

```javascript
import { isString, and, not, isEmpty, prop, safe, Maybe, chain, alt, map, option } from 'crocks';
```

從 Crocks import 進 `chain()`、`alt()` 與 `option()`，改用 function 操作 `Maybe`。

第 2 行

```javascript
import { compose, join, split, toLower, concat, pipe } from 'ramda';
```

從 Ramda import 進 `pipe()`。

> 也可以使用 Crocks 的 `compose()` 與 `pipe()` 

23 行

```javascript
// fn :: Object -> Maybe String
let fn = pipe(
  prop('title'),
  chain(safe(isNonEmptyString)),
  alt(of('page-not-found')),
  map(createUrl)
);
```

`pipe()` 將 `obj` parameter 給 point-free 了，且完全不使用 method chaining。

31 行

```javascript
option('https://oomusou.io')(fn(data)); // ?
```

`option()` 也可改用 function 方式。

![compose001](/images/crocks/maybe/function-composition/compose001.png)

## Function Composition

```javascript
import { isString, and, not, isEmpty, prop, safe, Maybe, chain, alt, map, option } from 'crocks';
import { compose, join, split, toLower, concat, pipe } from 'ramda';

let { of } = Maybe;

// isNonEmptyString :: a -> Boolean
let isNonEmptyString = and(not(isEmpty), isString);

// createUrlSlug :: String -> String
let createSlug = compose(join('-'), split(' '), toLower);

// appendSlug :: String -> String
let appendSlug = concat('https://oomusou.io/crocks/');

// getSlugIfEmptyString :: Maybe String -> Maybe String
let getSlugIfEmptyString = pipe(
  chain(safe(isNonEmptyString)),
  alt(of('page-not-found')),
);

// createUrl :: String -> String
let createUrl = compose(appendSlug, createSlug);

let data = {
  title: '',
  price: 100,
};

// fn :: Object -> Maybe String
let fn = pipe(
  prop('title'),
  getSlugIfEmptyString,
  map(createUrl)
);

option('https://oomusou.io')(fn(data)); // ?
```

29 行

```javascript
// fn :: Object -> Maybe String
let fn = pipe(
  prop('title'),
  getSlugIfEmptyString,
  map(createUrl)
);
```

> Q：從 method chaining 改成 function composition 只是語法差異，還有什麼實際用途嗎 ?

`chain()` 與 `alt()` 是為了處理 empty string 特別邏輯，可將這部分單獨抽出成 `getSlugIfEmptyString()`，其語義更為清楚。

15 行

```javascript
// getSlugIfEmptyString :: Maybe String -> Maybe String
let getSlugIfEmptyString = pipe(
  chain(safe(isNonEmptyString)),
  alt(of('page-not-found')),
);
```

反之若使用 method chaining，則 `chain()` 與 `alt()` 很難單獨抽出來，因為都綁在 `Maybe` 上了，這就是 FP 將 data 與 function 分離的優勢。。

![compose002](/images/crocks/maybe/function-composition/compose002.png)

## Conclusion

* 在 [使用 coalesce() 提早提供 Function 處理 Nothing](/crocks/maybe/coalesce/) 一文中的 `coalesce()` 也可改用 function
* 處理 `Maybe` 時，不見得一定得用其自帶的 method，也可以使用同名 function，方便 function composition

## Reference

[Andy Van Slaars](https://egghead.io/instructors/andrew-van-slaars), [Compose Function for Reusability with the Maybe Type](https://egghead.io/lessons/javascript-compose-functions-for-reusability-with-the-maybe-type)
[Crocks](https://evilsoft.github.io/crocks/), [chain()](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html#chain)
[Crocks](https://evilsoft.github.io/crocks/), [alt()](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html#alt)
[Crocks](https://evilsoft.github.io/crocks/), [coalesce()](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html#coalesce)
[Crocks](https://evilsoft.github.io/crocks/), [option()](https://evilsoft.github.io/crocks/docs/crocks/Maybe.html#option)

