title: 如何將 Array 中每筆資料之後新增一筆資料 ?
tags:
  - Ramda
  - Ramda/map
  - Ramda/pipe
  - Ramda/flatten
  - Ramda/dropLast
  - Ramda/chain
  - Ramda/init
  - Ramda/of
  - Ramda/append
feature: images/feature/ramda.png
date: 2019-06-07 10:33:32
---
在使用 Vuetify 的 Lists 時，為了讓視覺呈現更加明顯，我們想要在每筆資料間加上一條分隔線，但由於 Lists 要求從 Data 動手，必須在兩筆資料間加上 `{ divider: true }`，這該如何實現呢 ?

<!-- more -->

## Version

VS Code 1.35.0
Quokka 1.0.224
Ramda 0.26.1

## Imperative

```javascript
let data = [ 
  { status: 14 },
  { status: 16  },
  { status: 17 },
];

// fn :: [a] -> [b]
let fn = arr => {
  let result = [];
  let index = 0;

  for(let elem of arr) {
    result = [...result, elem];

    if (index < arr.length - 1) {
      result.push({ divider: true });
    }

    index++;
  }

  return result;
};

console.dir(fn(data));
```

從 API 獲得資料如下：

```javascript
[ { status: 14 },
  { status: 16  },
  { status: 17 } ];
```

我們希望最後結果為：

```javascript
[ { status: 14 }, 
  { divider: true }, 
  { status: 16 }, 
  { divider: true }, 
  { status: 17 } ] 
```

也就是每一筆資料都新增了 `{ divider: true }`，但注意最後一筆又不新增。

第 7 行

```javascript
// fn :: [a] -> [b]
let fn = arr => {
  let result = [];
  let index = 0;

  for(let elem of arr) {
    result.push(elem);

    if (index < arr.length - 1) {
      result.push({ divider: true });
    }

    index++;
  }

  return result;
};
```

13 行

```javascript
result.push(elem);
```

Imperative 會先建立欲回傳的 `result` array，由於原來資料都要保留，所以使用了 `result.push(elem)` 保存。

15 行

```javascript
if (index < arr.length - 1) {
  result.push({ divider: true });
}
```

因為每一筆資料都新增了 `{ divider: true }`，但最後一筆又不新增，因此使用了 `(index < arr.length - 1)` 做判斷，如此可確保最後一筆不會新增 `{ divider: true }`。

> Imperative 寫法比較易錯的是 `index` 判斷，因為 `index` 為 zero-based，所以全部資料為 `index < arr.length`，不包含最後一筆為 `index < arr.length - 1`，這是很容易產生 bug 之處

![chain000](/images/ramda/chain-divider/chain000.png)

## Functional

```javascript
import { map, pipe, flatten, dropLast } from 'ramda';

let data = [ 
  { status: 14 },
  { status: 16  },
  { status: 17 }
];

// fn :: [a] -> [b]
let fn = pipe(
  map(x => [x, { divider: true }]),
  flatten,
  dropLast(1)
);

console.dir(fn(data));
```

我們知道需求為：

* 每一筆資料都新增了 `{ divider: true }`
* 最後一筆不新增  `{ divider: true }`

以 Functional 角度思考：

* 每一筆資料從一筆變兩筆
* 再刪除最後一筆資料

11 行

```javascript
map(x => [x, { divider: true }]),
```

使用 `map()` 將每一筆資料變成兩筆，回傳為 array。

> **map()**
> `Functor f => (a -> b) -> f a -> f b`
> 將 Functor a 轉換成 Functor b

12 行

```javascript
flatten,
```

因為每一筆都是 array，所以變成了如下的兩層 array。

```javascript
[ [ { status: 14 }, { divider: true } ], 
  [ { status: 16 }, { divider: true } ], 
  [ { status: 17 }, { divider: true } ] ] 
```

這並不是我們要的，因此須使用 `flatten()` 加以攤平。

> **flatten()**
> `[a] -> [b]`
> 將多層 array 攤平成一層 array

13 行

```javascript
dropLast(1)
```

因為最後一筆不新增  `{ divider: true }`，所以使用了 `dropLast(1)` 刪除之。

> **dropLast()**
> `Number -> [a] -> [a]`
> `Number -> String -> String`
> 將 array 或 string 的最後幾筆資料刪除後回傳

我們發現使用 FP 後，透過 `pipe()` 組合 `map()`、`flatten()` 與 `dropLast()`，整個演算法清楚可見，不必如 imperative 須 trace code 才知道演算法為何。

![chain001](/images/ramda/chain-divider/chain001.png)

## Refactoring

```javascript
import { pipe, init, chain } from 'ramda';

let data = [ 
  { status: 14 },
  { status: 16 },
  { status: 17 }
];

// fn :: [a] -> [b]
let fn = pipe(
  chain(x => [x, { divider: true }]),
  init
);

console.dir(fn(data));
```

其實以上的寫法已經非常清楚，可再稍微重構簡化。

11 行

```javascript
chain(x => [x, { divider: true }]),
```

`chain()` 相當於 `map()` + `flatten()`，可再簡化成 `chain()`。

> **chain()**
> `Chain m => (a -> m b) -> m a -> m b`
> 相當於 `flatten()` 與 `map()` 組合

12 行

```javascript
init
```

目標是不要最後一筆，`init()` 則是傳回不包含最後一筆的所有資料，因此也可簡化成 `init()`。

> **init()**
> `[a] -> [a]`
> `String -> String`
> 回傳不包含最後一筆資料的 array 或 string

![chain002](/images/ramda/chain-divider/chain002.png)

## Point-free

```javascript
import { pipe, init, chain, of, append } from 'ramda';

let data = [ 
  { status: 14 },
  { status: 16  },
  { status: 17 },
];

// addDivider :: a -> b
let addDivider = pipe(
  of,
  append({ divider: true }), 
);

// fn :: [a] -> [b]
let fn = pipe(
  chain(addDivider),
  init
);

console.dir(fn(data));
```

若想連 callback 部分也想 point-free，可再進一步重構。

21 行

```javascript
chain(addDivider),
```

將 `chain()` 的 callback 抽成 `addDivider()`。

13 行

```javascript
// addDivider :: a -> b
let addDivider = pipe(
  of,
  append({ divider: true }), 
);
```

原本寫法為：

```javascript
x => [x, { divider: true }]
```

經過抽絲剝繭，其實相當於

* 將 `x` 變成 `[x]`
* 新增 `{ divider: true }` 到 array

只要將以上想法組合起來即可。

```javascript
of
```

使用 `of()` 將 `x` 變成 `[x]` 。

> **of()**
> `a -> [a]`
> 將單一值變成 array

```javascript
append({ divider: true }), 
```

使用 `append()` 將 `{ divider: true }` 新增至 array。

> **append()**
> `a -> [a] -> [a]`
> 將新資料新增到 array 尾部

![chain003](/images/ramda/chain-divider/chain003.png)


## Conclusion

* `map()` + `flatten()` 組合在實務上常常見到，可使用 `chain()` 加以重構
* 實務上最後一筆常有特殊需求，可使用 `dropLast()` 刪除之
* 是否要連 callback 也要 point-free 屬於品味問題，可自行決定是否使用

## Reference

[Ramda](https://ramdajs.com), [map()](https://ramdajs.com/docs/#map)
[Ramda](https://ramdajs.com), [pipe()](https://ramdajs.com/docs/#pipe)
[Ramda](https://ramdajs.com), [flatten()](https://ramdajs.com/docs/#flatten)
[Ramda](https://ramdajs.com), [dropLast()](https://ramdajs.com/docs/#dropLast)
[Ramda](https://ramdajs.com/docs/#append), [chain()](https://ramdajs.com/docs/#chain)
[Ramda](https://ramdajs.com/docs/#append), [init()](https://ramdajs.com/docs/#init)
[Ramda](https://ramdajs.com/docs/#append), [of()](https://ramdajs.com/docs/#of)
[Ramda](https://ramdajs.com/docs/#append), [append()](https://ramdajs.com/docs/#append)