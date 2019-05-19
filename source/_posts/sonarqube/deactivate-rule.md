title: 如何不啟用 SonarQube 內建的 Rule ?
tags:
  - SonarQube
feature: images/feature/sonarqube.png
date: 2019-05-19 16:01:46
---
SonarQube 內建很多檢查 Rule，但有些 Rule 可能不適合團隊，暫時不想啟用，該如何在 SonarQube 設定呢 ?

<!-- more -->

## Version

macOS High Sierra 10.13.6
Docker for Mac 18.06.1-ce-mac59 (26764)
SonarQube 7.1 (build 11001)

## Rules

![rule000](/images/sonarqube/deactivate-rule/rule000.png)

***Rule -> C#***

若第一個 rule :  `=+ should not be used instead of +=`  不適合團隊，想暫時不啟用檢查。

> 並不是這個 rule 不好，只是因為是第一個 C# rule，所以以此為範例

SonarQube 舊版允許你直接不啟用某個 rule，但新版取消了這個功能，無法直接不啟用。

## Quality Profiles

![rule001](/images/sonarqube/deactivate-rule/rule001.png)

***Quality Profiles -> C#***

SonarQube 預設使用的 Quality Profile 是 `Sonar way`，目前 `Sonar way` 無法取消 rule。

若要取消 rule，必須建立自己的 Quality Profile，然後才能取消。

![rule002](/images/sonarqube/deactivate-rule/rule002.png)

* 選擇右側  `option` 的 `Copy`

![rule003](/images/sonarqube/deactivate-rule/rule003.png)

1. 輸入新的 Quality Profile 名稱
2. 按 `Copy` 確定

![rule004](/images/sonarqube/deactivate-rule/rule004.png)

***Quality Profiles -> C#***

* 選擇 `My way`，選擇右側  `option` 的 `Set as Default`

![rule005](/images/sonarqube/deactivate-rule/rule005.png)

* `My way` 成為 C# project 預設的 Quality Profile

## Deactivate Rule

![rule006](/images/sonarqube/deactivate-rule/rule006.png)

***Quality Profiles -> C#***

* 點擊 `My way` Quality Profile

![rule007](/images/sonarqube/deactivate-rule/rule007.png)

1. 點擊 `Active` 下所有的 rule

![rule008](/images/sonarqube/deactivate-rule/rule008.png)

1. 找到我們想取消的 `=+ should not be used instead of +=` rule
2. 按 `Deactivate` 取消

![rule009](/images/sonarqube/deactivate-rule/rule009.png)

1.  `=+ should not be used instead of +=` rule 已被 `Deactivate`

## Conclusion

* 原本認為很簡單的功能，但因為 SonarQube 的設計有改變，竟然花了一些時間才搞定，特別記錄下來

