title: 如何在 Angular 使用 Ramda ?
tags:
  - Angular
  - Ramda
feature: images/feature/ramda.png
date: 2018-05-17 21:23:43
---
Ramda 是 Clojure 在 ECMAScript 的實作，讓我們可以將更多 FP 特性在 ECMAScript 實現，本文將以 Angular 與 TypeScript 為例，示範如何在 Angular 使用 Ramda。

<!-- more -->

## Version

macOS High Sierra 10.13.4
Node.js 8.11.1
Angular 6.0.2
Ramda 0.25

## Installation

```
$ npm install ramda
```

使用 npm 安裝 `ramda`。

## Type Definition for Ramda

```
$ npm install types/npm-ramda --save-dev
```

使用 npm 安裝 Ramda 的 type definition，由於只有開發使用，所以加上 `--save-dev`。

## Ramda in Angular

**app.component.ts**

```typescript
import {Component, OnInit} from '@angular/core';
import compose from 'ramda/src/compose';
import toUpper from 'ramda/src/toUpper';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  title = 'app';

  ngOnInit(): void {
    const classyGreeting = (firstName, lastName) =>
      'The name\'s ' + lastName + ', ' + firstName + ' ' + lastName;

    const yellGreeting = compose(toUpper, classyGreeting);
    console.log(yellGreeting('James', 'Bond'));
  }
}
```

第 2 行

```typescript
import compose from 'ramda/src/compose';
import toUpper from 'ramda/src/toUpper';
```

為了 tree shaking，將 `compose()` 與 `toUpper()` 單獨 import。

14 行

```typescript
const classyGreeting = (firstName, lastName) =>
      'The name\'s ' + lastName + ', ' + firstName + ' ' + lastName;
```

宣告 `classyGreeting()`。

17 行

```typescript
const yellGreeting = compose(toUpper, classyGreeting);
```

為了將 `classyGreeting()` 的所有回傳值都變成大寫，使用了 Ramda 的 `toUpper()`，並將 `classyGreeting()` 與 `toUpper()` 兩者 `compose()` 成 `yellGreeting()` 。

> 對於 function composition，ECMAScript 並不像 Haskell 提供 `.` ，或 F# 的 `>>` 來組合 function，因此借助 Ramda 的 `compose()` 將 `toUpper()` 與 `classyGreeting()` 組合成 `yellGreeting()`

![ramda000](/images/ramda/angular/ramda000.png)

 ## Type Inference

![ramda001](/images/ramda/angular/ramda001.png)

由於 TypeScript 的 type inference 的限制，目前經過 Ramda 所 compose 的 function，在 TypeScript 只能顯示 `any`，無法如 Haskell 或 F# 能完整顯示 compose 後 function 的型別，是比較遺憾的地方。

## Conclusion

* 除了在 Angular 使用 OOP 外，有了 Ramda，我們就可以將很多 FP 技巧用在 Angular，同時發揮 OOP 與 FP 的優點

## Sample Code

完整的範例可以在我的 [GitHub](https://github.com/oomusou/NG6Ramda) 上找到

## Reference

[Jacob E. Dawson](https://medium.com/@jacobedawson), [Using Ramda.js with Angular 2+/Angular CLI](https://medium.com/@jacobedawson/using-ramda-js-with-angular-2-angular-cli-9580f64c1794)
[Ramda](http://ramdajs.com), [compose()](http://ramdajs.com/docs/#compose)