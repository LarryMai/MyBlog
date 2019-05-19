title: 如何在 Visual Studio 使用 SonarLint ?
tags:
  - SonarQube
  - SonarLint
  - Visual Studio
feature: images/feature/sonarqube.png
date: 2019-05-19 15:31:24
---
SonarQube 除了搭配 Jenkins 檢查程式碼品質外，還可以在 IDE 中使用 SonarLint，讓 developer 在程式開發階段就及早發現可能的 bugs、vulenrability、code smell 與 duplication，本文將介紹 SonarLint + Visual Studio。

<!-- more -->

## Version

Windows 10 Pro 1709 16299.371
SonarQube 6.7.2 LTS
Visual Studio Enterprise 2017 15.6.6
SonarLint 3.10.0.3095
C# 7.2
.NET Framework 4.6.1

## SonarLint

![sonarlint000](/images/sonarqube/vs/sonarlint000.png)

***Tools -> Extensions and Update...***

![sonarlint001](/images/sonarqube/vs/sonarlint001.png)

1. 選擇 `Online`

2. 輸入 `SonarLint`

3. 選擇 `SonarLint for Visual Studio` 下載

![sonarlint002](/images/sonarqube/vs/sonarlint002.png)

1. 關閉 Visual Studio 後，按 `Modify` 安裝 SonarLint for Visual Studio

![sonarlint003](/images/sonarqube/vs/sonarlint003.png)

安裝完第一次進入 Visual Studio 後

1. SonarLint 即將下載其他語言的 scanner，如 JavaScript，將 `Don't show this message again` 打勾
2. 按 `Yes` 繼續

## SonarQube Server

![sonarlint004](/images/sonarqube/vs/sonarlint004.png)

先開啟 solution。

***Analyze -> Manage SonarQube Connections...***

![sonarlint005](/images/sonarqube/vs/sonarlint005.png)

1. 按 `Connect...` 連接 SonarQube server

![sonarlint006](/images/sonarqube/vs/sonarlint006.png)

1. 輸入 SonarQube server 網址
2. Login / Password 填預設的 `admin/admin`

![sonarlint007](/images/sonarqube/vs/sonarlint007.png)

連上 SonarQube server 後，會出現目前 server 上所有的 project，選擇你要綁定的專案。

> 綁定後會從 server 下載 profile 與 rule 到本機

![sonarlint008](/images/sonarqube/vs/sonarlint008.png)

與 SonarQube 綁定後，在 solution 下會出現 `SonarQube`。

## Automatic Analysis

![sonarlint009](/images/sonarqube/vs/sonarlint009.png)

SonarQube 檢查出 `IPadAir.cs` 有 code smell，class 不該使用 `I` 開頭。

![sonarlint010](/images/sonarqube/vs/sonarlint010.png)

SonarLint 能在 Visual Studio 內即時的檢查出目前檔案的 issue。

![sonarlint011](/images/sonarqube/vs/sonarlint011.png)

點擊左側的小燈泡，SonarLint 會解釋該 rule 檢查的理由。

## Manual Analysis

![sonarlint012](/images/sonarqube/vs/sonarlint012.png)

SonarLint 亦可手動檢查整個 solution。

1. 點擊 solution 右鍵，選 `Analyze -> Run Code Analysis on Solution`

![sonarlint013](/images/sonarqube/vs/sonarlint013.png)

SonarLint 會將所有檢查到的 issue 顯示在下方。

## Conclusion

* 有了 SonarLint，developer 就能更即時的獲得 SonarQube 的建議，養成寫出 clean code 的好習慣

