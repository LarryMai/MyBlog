title: 如何讓 SonarQube 不檢查特定目錄與檔案？
tags:
  - SonarQube
feature: images/feature/sonarqube.png
date: 2019-05-19 15:58:01
---
在使用 SonarQube 對專案做檢查時，有些目錄與檔案不想檢查，最典型的就是在 MVC 專案時，不想檢查 JavaScript Package，畢竟我們關心的是團隊的程式碼品質，而不是 Package，我們該如何設定排除呢？

<!-- more -->

## Version

SonarQube 7.1

## Setting

***Administration -> General Settings -> Analysis Scope -> File***

* **Source File Exclusions** : 如 `Scripts/**`

> `**` 表該目錄下所有子目錄的檔案都套用

若要設定多個目錄，可按 `Add value` 繼續新增。

最後記得 `Save Files Settings`，重新跑 Jenkins Job `Build Now`。

## Conclusion

* 排除掉不想檢查的目錄與檔案後，則 SonarQube 所顯示的資訊將更為客觀