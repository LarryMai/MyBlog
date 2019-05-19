title: 如何在 macOS 安裝 SonarQube ?
tags:
  - Ramda
  - SonarQube
  - macOS
  - Homebrew
feature: images/feature/sonarqube.png
date: 2019-05-19 09:21:41
---
SonarQube 是一套 `程式碼品質檢查工具`，可以幫我們檢查 Code 的 Bugs、 Vulenrability、Code Smell 與 Duplication，也屬於 `持續整合` 重要的；透過 Homebrew，我們可以很簡單地安裝 SonarQube。

<!-- more -->

## Version

macOS High Sierra 10.13.4
Java 10.0.1 2018-04-17
SonarQube 7.1

##  Installation

```shell
$ brew install sonarqube
```

使用 Homebrew 安裝 SonarQube。

![ac00](/images/sonarqube/macos/mac000.png)

* 若想在每次 Mac 重開機就自動執行 SonarQube，輸入 `brew services start sonarqube`

* 若想自行啟動 SonarQube，輸入 `sonar console`

## Start SonarQube

```shell
$ sonar console
```

使用 `sonar console` 自行啟動 SonarQube。

![ac00](/images/sonarqube/macos/mac001.png)

## Test SonarQube

![ac00](/images/sonarqube/macos/mac002.png)

輸入 `localhost:9000`，若看到 SonarQube 首頁，則表示安裝成功

> 右上角 `Log in` 可登入管理設定 SonarQube，預設為 `admin/admin`

## Conclusion

* 雖然 SonarQube 主要是安裝在 Linux server 上，但透過安裝在 macOS，我們也可以在本機執行 SonarQube