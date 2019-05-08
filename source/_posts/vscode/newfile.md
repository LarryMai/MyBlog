title: 重新定義 VS Code 之 New File Shortcut
tags:
  - VS Code
feature: images/feature/vscode.png
date: 2019-05-06 14:49:49
---
VS Code 使用 `⌘ N` 建立新檔，不過預設是 `Untitled`，檔名與檔案類型都未知，這使得 User Snippet 無法使用，必須先存檔確定檔案類型；既然如此，能否  `⌘ N` 就能直接輸入檔名，確定檔案類型呢 ?

<!-- more -->

## Version

VS Code 1.33.1

## Explorer.newFile

![newfile000](/images/vscode/newfile/newfile000.png)

其實在 Explorer 上的 `New File` icon，就是在建立新檔時直接輸入檔名，這符合大部分人使用習慣，不過  `⌘ N` 預設並不是使用 `Explorer.newFile`，而且還沒有 shortcut。

## Keyboard Shortcuts

![newfile002](/images/vscode/newfile/newfile002.png)

在 command palette 下輸入 `shortcut`，選擇 `Preferences: Open Keyboard Shortcuts`。

![newfile001](/images/vscode/newfile/newfile001.png)

輸入 `new file`，我們發現 `⌘ N` 綁定的是 `New Untitled File`，而非我們預期的 `explorer.newFile`。

![newfile003](/images/vscode/newfile/newfile003.png)

將 `⌘ N` 重新綁定到 `explorer.newFile`。

![newfile004](/images/vscode/newfile/newfile004.gif)

如此 `⌘ N` 是不是更符合使用習慣呢 ?

## Conclusion

* VS Code 優點就是彈性，因此若覺得預設 shortcut 不符合自己使用習慣，請大膽加以客製化