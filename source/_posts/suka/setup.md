title: 如何安裝 Suka for Hexo ?
tags:
  - Hexo
  - Suka
feature: images/feature/suka.png
date: 2019-04-29 09:23:43
---
[Suka](https://github.com/SukkaW/hexo-theme-suka) 是很漂亮的 Theme，但原本安裝的步驟叫繁瑣，很容易出錯，本文是使用自己改過的 Suka 版本，簡化了安裝步驟。

<!-- more -->

## Version

Node 10.15.3
Yarn 1.15.2
Hexo 3.8.0
Hexo-cli 1.1.0
Suka 1.3.2.1

## Create Blog

```
$ hexo init MyBlog
```

使用 Hexo 建立 `MyBlog` 目錄。

![setup009](/images/suka/setup/setup009.png)

## Installation

```
$ cd MyBlog
$ git clone https://github.com/oomusou/hexo-theme-suka.git themes/suka
```

將 Suka 下載到 `themes/suka` 目錄。

![setup000](/images/suka/setup/setup000.png)

```
$ cd themes/suka
$ yarn install
```

安裝 package。

![setup004](/images/suka/setup/setup004.png)

## Configuration

**_config.yml**

```yaml
# theme: landscape
theme: suka

suka_theme:
  search:
    enable: true
    path: search.json
    field: post # Page | Post | All. Default post
  prism:
    enable: false
    line_number: true
    theme: default  
```

第 2 行

```yaml
theme: suka
```

將 blog 的 `_config.yml` 的 `theme` 設定成 `suka`。

第 ４行

```yaml
suka_theme:
  search:
    enable: true
    path: search.json
    field: post # Page | Post | All. Default post
  prism:
    enable: false
    line_number: true
    theme: default  
```

將以上設定貼到 `_config.yml` 最下方，這是 Suka 所需要的設定。

![setup007](/images/suka/setup/setup007.png)

## Start Live Server

```
$ hexo server
```

啟動 Hexo 內建的 Live Server。

![setup010](/images/suka/setup/setup010.png)

![setup011](/images/suka/setup/setup011.png)

Suka 啟動成功。

## Conclusion

* 本來是想直接使用 Suka 就好，但因為需求不同，要改的東西太多，只好 fork 後自行修改
* 簡化了原本 Suka 的一些設定步驟，讓安裝更為簡單

## Reference

[hexo-theme-suka](https://github.com/SukkaW/hexo-theme-suka), [Install 安裝](https://github.com/SukkaW/hexo-theme-suka#install-安装)
[Suka](https://theme-suka.skk.moe), [開始使用](https://theme-suka.skk.moe/docs/)