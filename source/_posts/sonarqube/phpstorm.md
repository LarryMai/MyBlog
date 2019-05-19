title: 如何在 PhpStorm 使用 SonarLint ?
tags:
  - SonarQube
  - SonarLint
  - PhpStorm
feature: images/feature/sonarqube.png
date: 2019-05-19 15:23:07
---
SonarQube 除了搭配 Jenkins 檢查程式碼品質外，還可以在 IDE 中使用 SonarLint，讓 developer 在程式開發階段就及早發現可能的 bugs、vulenrability、code smell 與 duplication，本文將介紹 SonarLint + PhpStorm。

<!-- more -->

## Version

macOS High Sierra 10.13.4
SonarQube 6.7.2 LTS
PhpStorm 2018.1
SonarLint 3.3.0.2482
PHP 7.1.14
Laravel 5.6.17

## SonarLint

![sonarlint000](/images/sonarqube/phpstorm/sonarlint000.png)

1. ***PhpStorm -> Preferences ... -> Plugins***
2. 按 `Browse repositories…`

![sonarlint001](/images/sonarqube/phpstorm/sonarlint001.png)

1. 輸入 `SonarLint`
2. 按 `Install` 安裝

![sonarlint002](/images/sonarqube/phpstorm/sonarlint002.png)

1. 下載完 SonarLint 後，按 `Restart PhpStorm`

## General Setting

### Settings

![sonarlint003](/images/sonarqube/phpstorm/sonarlint003.png)

1. ***PhpStorm -> Preferences -> Other Settings -> SonarLint General Settings***
2. 選擇 `Settings` tab
3. 勾選 `Automatically trigger analysis`，SonarLint 將會自動檢查目前的檔案
4. 在 `SonarQube servers` 按 `+` 新增 server

![sonarlint004](/images/sonarqube/phpstorm/sonarlint004.png)

1. 為設定取一個名字
2. 輸入 SonarQube 的網址
3. 按 `Next` 繼續

![sonarlint005](/images/sonarqube/phpstorm/sonarlint005.png)

1. `Authentication type` 選擇 `Login / Password`
2. Login / Password 填預設的  `admin/admin`
3. 按 `Next` 繼續

![sonarlint006](/images/sonarqube/phpstorm/sonarlint006.png)

1. 按 `Finish` 完成設定

![sonarlint007](/images/sonarqube/phpstorm/sonarlint007.png)

1. 按 `Update binding` 從 SonarQube server 更新 project list、profile、rule 到本機

### File Exclusions

![sonarlint008](/images/sonarqube/phpstorm/sonarlint008.png)

1. 選擇 `File Exclusions` tab，若每個專案都有特定的目錄不想被 SonarLint 檢查，可統一設定在此

## Project Setting

### Bind to SonarQube project

![sonarlint009](/images/sonarqube/phpstorm/sonarlint009.png)

1. ***PhpStorm -> Preferences -> Other Settings -> SonarLint Project Settings***
2. 選擇 `Bind to SonarQube project` tab
3. 將 `Enable binding to remote SonarQube server` 打勾，如此本地專案將與 SonarQube server 端綁定
4. `Bind to server` 選擇剛剛建立的 `MySonarQube`
5. `SonarQube project` 選擇欲綁定的 `SonarQube` 專案

### File Exclusion

![sonarlint010](/images/sonarqube/phpstorm/sonarlint010.png)

1. 選擇 `File Exclusions` tab，若專案中有特定的目錄不想被 SonarLint 檢查，可設定在此
2. 按 `OK` 儲存設定

## Automatic Analysis

![sonarlint011](/images/sonarqube/phpstorm/sonarlint011.png)

SonarQube 檢查出在 `app/Console/Kernel.php` 有 code smell，不該在程式碼有註解掉的 code，而應該直接刪除，由 Git 作版控。

![sonarlint012](/images/sonarqube/phpstorm/sonarlint012.png)

SonarLint 能在 PhpStorm 內即時的檢查出目前檔案的 issue。

1. 下方選擇 `SonarLint` tab
2. 有問題的 code
3. SonarLint 的錯誤訊息，與 SonarQube server 完全一樣，
4. 解釋該 rule 檢查理由

## Conclusion

* 有了 SonarLint，developer 就能更即時的獲得 SonarQube 的建議，養成寫出 clean code 的好習慣