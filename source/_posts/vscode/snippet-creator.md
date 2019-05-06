title: 如何在 VS Code 快速建立 User Snippet ?
tags:
  - VS Code
feature: images/feature/vscode.png
date: 2019-05-06 17:31:35
---
雖然可以到 Ｍarketplace 下載別人建立好的 Snippet，但實務上常常會有自己特殊的 Code Snippet 不斷出現，此時就得自行建立 User Snippet。是否有更快速方式建立 User Snippet 呢 ?

<!-- more -->

## Version

VS Code 1.33.1
Snippet-creator 0.0.5

## User Snippet

![snippet000](/images/vscode/snippet-creator/snippet000.png)

VS Code 預設的 user snippet 如上圖所示，必須在 `body` 內每一行以 string 方式建立 array。

若是 ECMAScript，因為 snippet 通常是幾行而已，問題不大。

但若是 HTML，由於行數較多，若一一建立 string array，則相當沒有效率，因此我們需要更有效率方式建立 user snippet。

## Snippet Creator

![snippet001](/images/vscode/snippet-creator/snippet001.png)

輸入 `snippet creator`，選擇 `snippet-creator`。

**bs4.html**

```html
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">

    <title>Hello, world!</title>
  </head>
  <body>
    <h1>Hello, world!</h1>

    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
  </body>
</html>
```

以 Bootstrap 4.3 的 starter template 為例，我們希望輸入 `bs4` 就會出現以上 HTML，若要自己產生 `body` 所需要的 string array 就太辛苦了。

![snippet002](/images/vscode/snippet-creator/snippet002.gif)

* `⌘A` 全選 Bootstrap starter template
* `⌘⇧P` 啟動 command palette，輸入 `create`，選擇 `Create Snipper`
* 輸入 `html` 選擇建立 HTML template
* 輸入 `bs4` 為 snippet name，為 key 必須為一不重複
* 輸入 `bs4` 為 prefix，也就是實際要輸入的文字
* Description 輸入空白，也可稍後再補上
* `⌘⇧P` 啟動 command palette，輸入 `user snippet`，選擇 `Preferences: Configure User Snippets`
* 已自動建立 user snippet 在 `~/Library/Application Support/Code/User/snippets/html.json`

## VS Code Snippet

**html.json**

```json
{
  "bs4": {
    "prefix": "bs4",
    "body": [
      "<!doctype html>",
      "<html lang=\"en\">",
      "  <head>",
      "    <!-- Required meta tags -->",
      "    <meta charset=\"utf-8\">",
      "    <meta name=\"viewport\" content=\"width=device-width, initial-scale=1, shrink-to-fit=no\">",
      "",
      "    <!-- Bootstrap CSS -->",
      "    <link rel=\"stylesheet\" href=\"https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css\" integrity=\"sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T\" crossorigin=\"anonymous\">",
      "",
      "    <title>Hello, world!</title>",
      "  </head>",
      "  <body>",
      "    $1",
      "",
      "    <!-- Optional JavaScript -->",
      "    <!-- jQuery first, then Popper.js, then Bootstrap JS -->",
      "    <script src=\"https://code.jquery.com/jquery-3.3.1.slim.min.js\" integrity=\"sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo\" crossorigin=\"anonymous\"></script>",
      "    <script src=\"https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js\" integrity=\"sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1\" crossorigin=\"anonymous\"></script>",
      "    <script src=\"https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js\" integrity=\"sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM\" crossorigin=\"anonymous\"></script>",
      "  </body>",
      "</html>"
    ],
    "description": "Default Bootstrap 4 starter template"
  }
}
```

- `prefix` ：定義 snippet 名稱，也是我們要在 VS Code 要輸入的文字
- `body`：將原本的 startup template 貼到 `body` 下，為 array，每一行為 string
- `description`：輸入 prefix 時，在 VS Code 會顯示的註解

17 行

```html
"  <body>",
"    $1",
"",
```

可加上 `$1`、`$2` … 等代表 cursor 依次停留之處。

28 行

```html
"description": "Default Bootstrap 4 starter template"
```

補上剛剛未輸入的 description。

![snippet003](/images/vscode/snippet-creator/snippet003.gif)

* 在 HTML 輸入 `bs4`，可出現 snippet 預覽，也可看到剛剛建立的 description
* cursor 停留在剛剛的 `$1` 處

## Conclusion

* Snippet-creator 雖然已經不再維護，但目前為止功能依舊正常，在沒有新的工具前，可以先頂著用
* 一但 workflow 經常出現 copy & paste，就該善用 VS Code 的 user snippet，可省下 copy & paste 時間，大幅提高生產力

## Reference

[Nikita Kunevich](https://marketplace.visualstudio.com/publishers/nikitaKunevich), [Snippet-creator](https://marketplace.visualstudio.com/items?itemName=nikitaKunevich.snippet-creator)

