title: 如何使用 SonarQube 檢查 Angular 專案？
tags:
  - SonarQube
  - Angular
  - macOS
  - Homebrew
  - Jenkins
  - Slack
feature: images/feature/sonarqube.png
date: 2019-05-19 10:32:38
---
SonarQube 已經內建 SonarTS，可以直接對 TypeScript 進行檢查，本文將以 Angular 為例，並搭配 Jenkins 自動執行 SonarQube，將結果通知 Slack。

<!-- more -->

## Version

macOS High Sierra 10.13.4
SonarQube 6.7.2 LTS
Jenkins 2.107.1
Slack 3.0.5
Angular 5.2.9

## GitHub

將 Angular 專案放到 GitHub。

![lack00](/images/sonarqube/angular/slack000.png)

1. 本文將 Angular 專案放在 `https://github.com/oomusou/NG52JenkinsSonarQubeSlack`

> 當然也可以將 git repository 放在不同的 git server，如 Bitbucket

## SonarQube

### Installation

```shell
$ brew update
$ brew install sonarqube
```

使用 Homebrew 安裝 SonarQube。

![lack00](/images/sonarqube/angular/slack001.png)

* 若想在每次 Mac 重開機就自動執行 SonarQube，輸入 `brew services start sonarqube`

* 若想自行啟動 SonarQube，輸入 `sonar console`

### Start SonarConsole

```shell
$ sonar console
```

使用 `sonar console` 自行啟動 SonarQube。

![lack00](/images/sonarqube/angular/slack002.png)

### Test SonarQube

![lack00](/images/sonarqube/angular/slack003.png)

輸入 `localhost:9000`，若看到 SonarQube 首頁，則表示安裝成功

> 右上角 `Log in` 可登入管理設定 SonarQube，預設為 `admin/admin`

## SonarQube Scanner

SonarQube 雖然已經包含 SonarTS，但必須靠 SonarQube Scanner 才能執行，預設 SonarQube 並沒有包含 Scanner，必須自行安裝。

### Download Scanner

![hp00](/images/sonarqube/angular/slack004.png)

到 [Analyzing with SonarQube Scanner](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner) 下載 Scanner，選擇 `Mac OS X 64 bit` 下載。

![lack00](/images/sonarqube/angular/slack005.png)

下載後為一 `zip` 壓縮檔，解壓縮後可安裝在任何目錄。

![lack00](/images/sonarqube/angular/slack006.png)

1. 選擇 home directory
2. 將 `sonar-scanner-3.1.0.1141-macosx` 放在 home directory 下

### Configuration

**sonar-scanner.properties**

```shell
#----- Default SonarQube server
sonar.host.url=http://localhost:9000
```

設定 SonarQube server 位址，並將 `#` 註解拿掉。

![lack00](/images/sonarqube/angular/slack007.png)

1. `sonar-scanner.properties` 位於 `sonar-scanner-3.1.0.1141-macosx/conf/` 目錄下

### Test Scanner

將 SonarQube Scanner 加到 system path。

**.zshrc**![lack00](/images/sonarqube/angular/slack008.png)

1. 將 `~/sonar-scanner-3.1.0.1141-macosx/bin` 目錄加到 system path

```shell
$ sonar-scanner -Dsonar.projectKey=Angular52 -Dsonar.sources=. -Dsonar.projectName=Angular52 -Dsonar.projectVersion=1.0
```
使用 `sonar-scanner` 對 Angular 專案進行檢查。

* **-D** : 對 SonarQube 的 property 進行設定
* **sonar.projectKey**：SonarQube 對專案的 key，內部將以此 key 作為識別，必須唯一
* **sonar.sources**：SonarQube 要檢查的目錄，因為已經在專案目錄下，`.` 即為 `目前目錄`
* **sonar.projectName**：在 SonarQube 網頁上顯示的名稱
* **sonar.projectVersion**：在 SonarQube 網頁上顯示的版本編號

![lack00](/images/sonarqube/angular/slack009.png)

1. 在專案目錄下輸入 `sonar-scanner` 檢查 TypeScript

![lack01](/images/sonarqube/angular/slack010.png)

1. 若能看到 `EXECUTION SUCCESS`，則表示 SonarQube Scanner 安裝成功

![lack01](/images/sonarqube/angular/slack011.png)

進入 SonarQube 網頁，就可看到 `Angular52` 專案已經出現 SonarQube。

到目前為止，SonarQube 對 TypeScript 檢查已經完成，就算只將 SonarQube 裝在本機，也對 TypeScript 程式碼品質檢查有很大幫助。

> Q：Angular 專案有 `node_modules` 目錄，不需如 Lavavel 專案去排除 `vendor` 目錄嗎？

![lack05](/images/sonarqube/angular/slack050.png)

***Administration -> TypeScript***

預設已經排除了 `node_modules`，所以不須在 `sonar-scanner` 下特別指定。

## Slack

若每次 SonarQube 檢查完，都能將結果送到 Slack，讓 Slack 成為實質的 `持續整合` 中心，那就太好了。

### Add Channel

![lack01](/images/sonarqube/angular/slack012.png)

* 按下 `Channels` 右側的 `+` 新增 channel

![lack01](/images/sonarqube/angular/slack013.png)

1. **Privacy** : 設定為 `Public` 或 `Private` channel
2. **Name** : 設定 channel 名稱
3. **Purpose** : channel 的功能描述，可以不輸入
4. **Send invites to** : 設定 channel 成員，可以稍後再設定
5. 按 `Create Channel` 開始建立 channel

![lack01](/images/sonarqube/angular/slack014.png)

1. 按 `Got It!` 進入 channel

![lack01](/images/sonarqube/angular/slack015.png)

1. 正式進入 channel，將來 SonarQube 訊息會傳進此 channel

### Add Notification

![lack01](/images/sonarqube/angular/slack016.png)

1. 選擇右上方的 `option`
2. 選擇 `Add an app`

### Add Incoming WebHooks

![lack01](/images/sonarqube/angular/slack017.png)

Slack 將開啟 browser

1. 稍微往下捲輸入 `webhook`
2. 選擇 `Incoming WebHooks`

### Add Configuration

![lack01](/images/sonarqube/angular/slack018.png)

1. 按 `Add Configuration` 加入 Jenkins

### Add Integration

![lack01](/images/sonarqube/angular/slack019.png)

1. 按 `Add Incoming WebHooks Integration` 正式加入

### Setup Instructions

![lack02](/images/sonarqube/angular/slack020.png)

1. 介紹 SonarQube 設定流程

> Slack 部分已經設定完成，接下來是 SonarQube 的設定
>
> Slack 網頁先不要關閉，稍後會用到

## SonarQube Slack Notifier

SonarQube 預設並沒有辦法直接對 Slack 發出 notification，必須另外安裝 [CKS Sonar Slack Notifier Plugin](https://github.com/kogitant/sonar-slack-notifier-plugin)。

### Download Plugin

![lack02](/images/sonarqube/angular/slack021.png)

### Add Plugin

![lack02](/images/sonarqube/angular/slack022.png)

將 `cks-slack-notifier-2.1.2.jar` 複製到 `/usr/local/Cellar/sonarqube/7.0/libexec/extensions/plugins` 目錄下。

> 因為 Homebrew 會將 app 安裝在 `/usr/local/Cellar/` 目錄下

### Setup Plugin

![lack02](/images/sonarqube/angular/slack023.png)

重新啟動 SonarQube：

***Administration -> Slack***

1. 在 `Administration` 下會發現多了 `Slack`
2. 選擇 `Slack`
3. **Slack web integration hook**：設定 `Slack web integration hook`
4. **Plugin enabled**：`enable`

> Q： `Slack web integration hook` 該填什麼呢？

![lack02](/images/sonarqube/angular/slack024.png)

回到 Slack 最後的網頁往下捲到 `Webhook URL`

1. 將 `Webhook URL` 的一長串 URL 複製，貼到 SonarQube 的 `Slack web integration hook`

最後按 `Save` 存檔。

### Setup Project & Channel

雖然已經設定了 SonarQube 的 Webhook URL，但還要繼續設定 project 與所對應的 Slack channel，才會正式開啟 notification。

![lack02](/images/sonarqube/angular/slack025.png)


1. **Project Key**： 設定 SonarQube 的 project key
2. **Slack channel**：設定所要通知 Slack channel

最後按 `Save` 存檔。

### Test Plugin

```shell
$ sonar-scanner -Dsonar.projectKey=Angular52 -Dsonar.sources=. -Dsonar.projectName=Angular52 -Dsonar.projectVersion=1.0
```

重新執行 `sonar-scanner`。

![lack02](/images/sonarqube/angular/slack027.png)

1. 重新在專案目錄下輸入 `sonar-scanner` 檢查 TypeScript

![lack02](/images/sonarqube/angular/slack026.png)

1. Slack 會收到 SonarQube 檢查的結果

## Jenkins

實務上 SonarQube 還會與其他 CI server 合作，接下來將以 Jenkins 為例，介紹如何以 Jenkins 自動執行 SonarQube。

### Installation

```shell
$ brew install jenkins-lts
```

使用 Homebrew 安裝 Jenkins。

![lack02](/images/sonarqube/angular/slack028.png)

* 若想在每次 Mac 重開機就自動執行 Jenkins，輸入 `brew services start jenkins-lts`

* 若想自行啟動 Jenkins，輸入 `jenkins-lts`

### Start Jenkins

```shell
$ jenkins-lts
```

使用 `jenkins-lts` 自行啟動 Jenkins。

![lack02](/images/sonarqube/angular/slack029.png)

### Unlock Jenkins

![lack03](/images/sonarqube/angular/slack030.png)

1. 輸入 `localhost:8080`，看到 Unlock Jenkins，開始設定 Jenkins
2. 預設密碼放在 `./jenkins/secrets/initialAdminPassword`

### Customize Jenkins

![lack03](/images/sonarqube/angular/slack031.png)

1. 選擇 `Install suggested plugins` 即可

### Getting Started

![lack03](/images/sonarqube/angular/slack032.png)

安裝 default plugin 中。

### Create Admin User

![lack03](/images/sonarqube/angular/slack033.png)

* 建立管理者帳號

### Jenkins is Ready

![lack03](/images/sonarqube/angular/slack034.png)

* Jenkins 安裝完成，按 `Start using jenkins` 開始使用 Jenkins

### Welcome to Jenkins

![lack03](/images/sonarqube/angular/slack035.png)

* 進入 Jenkins 管理介面，如此 Jenkins 已經設定成功

## SonarQube Scanner for Jenkins

要讓 Jenkins 能自動執行 SonarQube Scanner，必須另外安裝 plugin。

### Installation

![lack03](/images/sonarqube/angular/slack036.png)

1. 左側選擇 `Manage Jenkins`
2. 右側選擇 `Manage Plugins`

![lack03](/images/sonarqube/angular/slack037.png)

1. 選擇 `SonarQube Scanner`
2. 按 `Download now and install after restart`

![lack03](/images/sonarqube/angular/slack038.png)

安裝 plugin 中。

1. 將 `Restart Jenkins when installation is complete and no jobs are running` 打勾

### SonarQube Configuration

![lack03](/images/sonarqube/angular/slack039.png)

1. 左側選擇 `Manage Jenkins`
2. 右側選擇 `Configure System`

![lack04](/images/sonarqube/angular/slack040.png)

1. 找到 `SonarQube servers` 區段
2. 將 `Enable injection of SonarQube server configuration as build environment variables` 打勾
3. 設定 SonarQube server 名稱與網址

最後按 `Apply` 與 `Save` 儲存設定。

### SonarQube Scanner Configuration

![lack04](/images/sonarqube/angular/slack041.png)

1. 左側選擇 `Manage Jenkins`
2. 右側選擇 `Global Tool Configuration`

![lack04](/images/sonarqube/angular/slack042.png)

1. 找到 `SonarQube Scanner` 區段
2. 設定 SonarQube Scanner 名稱

最後按 `Apply` 與 `Save` 儲存設定。

## Jenkins Job

### Source Code Management

![lack04](/images/sonarqube/angular/slack043.png)

1. 選擇 Job，按左側 `Configure`

![lack04](/images/sonarqube/angular/slack044.png)

1. 找到 `Source Code Management` 區段
2. 將 Git 的 `Repository URL` 設定到 `https://github.com/oomusou/NG52JenkinsSonarQubeSlack`

### Build

![lack04](/images/sonarqube/angular/slack045.png)

1. 找到 `Build` 區段
2. 在 `Add build step` 選擇 `Execute SonarQube Scanner`

![lack04](/images/sonarqube/angular/slack046.png)

```shell
sonar.projectKey=Angular52
sonar.sources=. 
sonar.projectName=Angular52 
sonar.projectVersion=1.0
```

在 `Analysis properties` 加上以上設定。

```shell
~/MyProject $ sonar-scanner -Dsonar.projectKey=Angular52 -Dsonar.sources=. -Dsonar.projectName=Angular52 -Dsonar.projectVersion=1.0
```

還記得之前曾經下過以上指令嗎？就是將這些 SonarQube property 設定在 Jenkins，改由 Jenkins 幫我們傳。

![lack04](/images/sonarqube/angular/slack047.png)

1. 按左側 `Build Now` 執行 Job
2. 執行成功後會出現 `OK` 與 `Success`。

![lack04](/images/sonarqube/angular/slack048.png)

剛剛 Jenkins 執行的 Project 出現在 SonarQube 上。

![lack04](/images/sonarqube/angular/slack049.png)

* Slack 也再次收到 SonarQube 檢查的結果

## Conclusion

- SonarQube 已經內建 SonarTS，也可以用來檢查 TypeScript 與 Angular
- 就算不將 SonarQube 安裝在 server，安裝在本機也能有效檢查 TypeScript 程式碼品質
- SonarQube 也能將檢查結果推送到 Slack
- 藉由 Jenkins 幫忙，我們就可以自動化執行 SonarQube

## Sample Code

完整範例可以在我的 [GitHub](https://github.com/oomusou/NG52JenkinsSonarQubeSlack) 上找到

## Reference

[SonarQube](https://www.sonarqube.org), [SonarQube Downloads](https://www.sonarqube.org/downloads/)
[SonarQube](https://docs.sonarqube.org/), [Analyzing with SonarQube Scanner](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner)
[SonarQube](https://docs.sonarqube.org/), [Analying with SonarQube Scanner for Jenkins](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+Jenkins)
[Jenkins](https://jenkins.io/), [SonarQube Scanner](https://plugins.jenkins.io/sonar)
[Antti Koivisto](https://github.com/kogitant), [CKS Sonar Slack Notifier Plugin](https://github.com/kogitant/sonar-slack-notifier-plugin)

