title: VS Code Editing Shortcut
tags:
  - VS Code
feature: images/feature/vscode.png
date: 2019-04-24 09:23:43
---
VS Code 主要功能就是在寫 Code，因此熟悉編輯用 Shortcut 是首要任務。本文整理出自己 Editing 所使用 Shortcut，參考 VS Code 與 WebStorm 各自優點整合而來，以 `兩鍵` 與 `順手` 為原則。

<!-- more -->

## Version

VS Code 1.33.1

## Word Editing

**Select Word**

`⌘D`

**Delete Word**

`⌘T`

> 需要 `Macros` extension 自訂 shortcut

## Line Editing

**Select Line**

`⌘L`

**Copy Line**

`⌘C`

**Cut Line**

`⌘X`

**Delete Line**

`⌘⌫`

**Delete to Beginning / End of Line**

`⌃⌫` / `⌥⌫`

> 需要 `Macros` extension 自訂 shortcut

**Duplicate Line**

`⌘U`

**Move Line Up / Down**

`⌥⌘↑` / `⌥⌘↓`

**Join Line**

`⌘J`

## Position

**Go to Beginning / End of Line**

`⌘←` / `⌘→` 

`fn←` / `fn→` 

**Go to Beginning / End of File**

`⌘↑` / `⌘↓`

**Go to PgUp / PgDn of File**

`fn↑` / `fn↓`

**Scroll Line Up / Down**

`fn⌃↑` / `fn⌃↓` 

**Scroll PageUp / PgDn**

`fn⌘↑` / `fn⌘↓` 

**Jump to Matching Bracket**

`⇧⌘\`	


## Selection

**Expand / Shrink Selection**

`⌥↑` / `⌥↓`

> 可用於 select word

**Select to Bracket**

`⇧⌥\`

**Select All Occurrences**

`⇧⌘L`

**Select All Occurrences of Current Word**

`⇧⌘W`

## Column Selection

**By Mouse**

`⌥⇧` + mouse drag

**Keyboard Up / Down / Left / Right**

`⇧⌥⌘↑` / `⇧⌥⌘↓` / `⇧⌥⌘←` / `⇧⌥⌘→` 

## Region

**Fold / Unfold Region**

`⌥⌘[` / `⌥⌘]` 

**Fold / Unfold All Subregions**

`⌘K ⌘[` / `⌘K ⌘]`

**Fold / Unfold All Regions**

`⌘K ⌘0` / `⌘K ⌘J`

## Toggle

**Toggle Line Comment**

`⌥/`
**Toggle Word Wrap**

`⌥Z`

## Multi-cursor

**Insert Cursor**

`⌥` + click

**Insert Cursor Above**

`⌃↑`

**Insert Cursor Below**

`⌃↓`

**Undo Cursor**

`⌃U`

**Insert Cursor at Each Selected Line**

`⇧⌥I`

> 常搭配 `⌘L` 先 select line

## Misc

**Copy All**

`⌥A`

> 需要 `Macros` extension 自訂 shortcut

**Cut All**

`⌃A`

> 需要 `Macros` extension 自訂 shortcut

## Conclusion

* VS Code 與 macOS 雖然內建很多 shortcut，若自己真的不常用，可以嘗試移除之，以常用的 action 取代，然後不斷根據自己的 workflow 調整
* Editing 最高境界就是不要使用 trackpad，完全以 keyboard 操作

## Reference

[VS Code](https://code.visualstudio.com), [Basic Editing](https://code.visualstudio.com/docs/editor/codebasics)
[VS Code](https://code.visualstudio.com), [Editing hacks](https://code.visualstudio.com/docs/getstarted/tips-and-tricks#_editing-hacks)
[Geddski](https://gedd.ski), [Macros](https://marketplace.visualstudio.com/items?itemName=geddski.macros)