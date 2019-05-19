title: 如何解決 SonarLint 的 Analyzer Not Loaded 錯誤訊息 ?
tags:
  - SonarQube
  - SonarLint
  - PhpStorm
feature: images/feature/sonarqube.png
date: 2019-05-19 15:43:39
---
當在 IntelliJ 平台使用 SonarLint 時，只要在 `SonarLint General Settings` 下按 `Update Binding` 就會出現 `Analyzer Not Loaded` 的錯誤訊息，這該如何解決呢？

<!-- more -->

## Version

macOS High Sierra 10.13.4
SonarQube 6.7.2 LTS
SonarTS 1.6
PhpStorm 2018.1
SonarLint 3.3.0.2482

## Sympton

![sonarts000](/images/sonarqube/analyzer-not-loaded/sonarts000.png)

***PhpStorm -> Other Settings -> SonarLint General Settings***

1. 當按下 `Update binding`，欲將 SonarQube server 上的 project lists、rules、profile 下載到本機
2. 出現 `Analyzers Not Loaded` 錯誤訊息

 ## Root Cause

SonarLint 3.3.0.2482 必須搭配使用 SonarTS 1.5 以上，但是 SonarQube 6.7.2 LTS 預設提供為 SonarTS 1.1，因此 SonarLint 無法載入 TypeScript analyzer。

> SonarQube 7.1 預設就提供 SonarTS 1.6，就不會有這個問題

## Recipe

![sonarts001](/images/sonarqube/analyzer-not-loaded/sonarts001.png)

1. 到 SonarTS 的 [GitHub](https://github.com/SonarSource/SonarTS/releases) 下載最新版 SonarTS 1.6
2. 選擇 `.jar` 格式下載

![sonarts002](/images/sonarqube/analyzer-not-loaded/sonarts002.png)

1. 到 SonarQube 安裝目錄下的 `extensions/plugins` 
2. 將原本的 `sonar-typescript-plugin-1.1.0.1079.jar` 刪除，以 `sonar-typescript-plugin-1.6.0.2388.jar` 取代之
3. 重新啟動 SonarQube

在 PhpStorm 重新 `Update binding` 就不會出現錯誤訊息了。

## Conclusion

* 若使用 SonarQube 最新版就不會有這個問題，但若使用 SonarQube LTS，因為 SonarTS 的版本較舊，與 SonarLint 搭配就會有問題

