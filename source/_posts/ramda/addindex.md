title: Ramda 之 addIndex()
tags:
  - Ramda
  - Ramda/addIndex
  - Ramda/map
feature: images/feature/ramda.png
date: 2019-05-15 04:23:43
---
若想在 `Array.prototype.map()` 的 Callback 得知目前 Element 的 Index，其 Callback 的第二個參數就是 Index，但 Ramda 的 `map()` 並沒有提供如此功能，該如何解決呢 ?

<!-- more -->

## Version

VS Code 1.33.1
Quokka 1.0.209
Ramda 0.26.1

## Array.prototype.map()

```javascript
let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

// fn :: [a] -> [b]
let fn = arr => arr.map((x, i) => `${i}.${x.title}`);
console.dir(fn(data));
```

第 7 行

```javascript
// fn :: [a] -> [b]
let fn = arr => arr.map((x, i) => `${i}.${x.title}`);
```

第二個參數 `i` 就是 index，可直接使用。

![index000](/images/ramda/addindex/index000.png)

## addIndex()

```javascript
import { map, addIndex } from 'ramda';
let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

// mapIndex :: (a -> b) -> [a] -> [b]
let mapIndex = addIndex(map);

// fn : (a -> b) -> [a] -> [b]
let fn = mapIndex((x, i) => `${i}.${x.title}`);
console.dir(fn(data));
```

第 8 行

```javascript
// mapIndex :: (a -> b) -> [a] -> [b]
let mapIndex = addIndex(map);
```

`map()` 必須經過 `addIndex()` 的包裝後，其 callback 才有 index 可用。

>**addIndex()**
>`((a … → b) … → [a] → *) → ((a …, Int, [a] → b) … → [a] → *)`
>將 callback 加上 index

![index001](/images/ramda/addindex/index001.png)

## Point-free

```javascript
import { map, addIndex, useWith, concat, identity, prop, flip, pipe } from 'ramda';

let data = [
  { title: 'FP in JavaScript', price: 100 },
  { title: 'RxJS in Action', price: 200 },
  { title: 'Speaking JavaScript', price: 300 }
];

// mapIndex :: (a -> b) -> [a] -> [b]
let mapIndex = addIndex(map);

// title :: Object -> String
let title = pipe(prop('title'), concat('.'));

// index :: Object -> String
let index = pipe(identity, String);

// fn :: (a -> b) -> [a] -> [b]
let fn = mapIndex(useWith(
  flip(concat), [title, index])
);
console.dir(fn(data));
```

也可以將 callback 部分進一步 point-free。

> 這個 point-free 在 production code 就不建議使用，因為太複雜了，可讀性不高，僅適合拿來練習 point-free

![index002](/images/ramda/addindex/index002.png)

## Conclusion

* 其他沒有 index 的 callback，也可使用 `addIndex()` 加上 index
* 有些 point-free 會使 callback 更為複雜，已失去其本意，在 production code 不建議使用，只適合拿來練功

## Reference

[Ramda](https://ramdajs.com), [map()](https://ramdajs.com/docs/#map)
[Ramda](https://ramdajs.com), [addIndex()](https://ramdajs.com/docs/#addIndex)