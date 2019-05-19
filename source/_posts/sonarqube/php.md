title: 如何使用 SonarQube 檢查 PHP 專案？
tags:
  - SonarQube
  - PHP
  - Laravel
  - Jenkins
  - macOS
  - Homebrew
feature: images/feature/sonarqube.png
date: 2019-05-19 10:09:16
---
SonarQube 已經內建 SonarPHP，可以直接對 PHP 進行檢查，本文將以 Laravel 為例，並搭配 Jenkins 自動執行 SonarQube。

<!-- more -->

## Version

macOS High Sierra 10.13.4
SonarQube 6.7.2 LTS
SonarPHP 2.1.3
SonarQube Scanner 3.1
Jenkins 2.107.1
SonarQube Scanner for Jenkins 2.6.1
PHP 7.1.14
Laravel 5.6.14

## GitHub

將 Laravel 專案放到 GitHub。

![hp01](/images/sonarqube/php/php015.png)

1. 本文將 Laravel 專案放在 `https://github.com/oomusou/Laravel56Demo`

> 當然也可以將 git repository 放在不同的 git server，如 Bitbucket

## SonarQube

### Installation

```shell
$ brew update
$ brew install sonarqube
```

使用 Homebrew 安裝 SonarQube。

![hp01](/images/sonarqube/php/php016.png)

* 若想在每次 Mac 重開機就自動執行 SonarQube，輸入 `brew services start sonarqube`

* 若想自行啟動 SonarQube，輸入 `sonar console`

### Start SonarQube

```shell
$ sonar console
```

使用 `sonar console` 自行啟動 SonarQube。

![hp02](/images/sonarqube/php/php025.png)

### Test SonarQube

![hp01](/images/sonarqube/php/php017.png)

輸入 `localhost:9000`，若看到 SonarQube 首頁，則表示安裝成功

> 右上角 `Log in` 可登入管理設定 SonarQube，預設為 `admin/admin`

## SonarQube Scanner

SonarQube 雖然已經包含 SonarPHP，但必須靠 SonarQube Scanner 才能執行，預設 SonarQube 並沒有包含 Scanner，必須自行安裝。

### Download Scanner

![hp00](/images/sonarqube/php/php000.png)

到 [Analyzing with SonarQube Scanner](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner) 下載 Scanner，選擇 `Mac OS X 64 bit` 下載。

![hp01](/images/sonarqube/php/php018.png)

下載後為一 `zip` 壓縮檔，解壓縮後可安裝在任何目錄。

![hp01](/images/sonarqube/php/php019.png)

1. 選擇 home directory
2. 將 `sonar-scanner-3.1.0.1141-macosx` 放在 home directory 下

### Configuration

**sonar-scanner.properties**

```
#----- Default SonarQube server
sonar.host.url=http://localhost:9000
```

設定 SonarQube server 位址，並將 `#` 註解拿掉。

![hp02](/images/sonarqube/php/php020.png)

1. `sonar-scanner.properties` 位於 `sonar-scanner-3.1.0.1141-macosx/conf/` 目錄下

### Test Scanner

將 SonarQube Scanner 加到 system path。

**.zshrc**

![hp02](/images/sonarqube/php/php022.png)

1. 將 `~/sonar-scanner-3.1.0.1141-macosx/bin` 目錄加到 system path

```shell
$ sonar-scanner -Dsonar.projectKey=Laravel56 -Dsonar.sources=. -Dsonar.projectName=Laravel56 -Dsonar.projectVersion=1.0 -Dsonar.exclusions=vendor/**
```

使用 `sonar-scanner` 對 Laravel 專案進行檢查。

* **-D** : 對 SonarQube 的 property 進行設定
* **sonar.projectKey**：SonarQube 對專案的 key，內部將以此 key 作為辨別，必須唯一
* **sonar.sources**：SonarQube 要檢查的目錄，因為已經在專案目錄下，`.` 即為 `目前目錄`
* **sonar.projectName**：在 SonarQube 網頁上顯示的名稱
* **sonar.projectVersion**：在 SonarQube 網頁上顯示的版本編號
* **sonar.exclusions**：不受 SonarQube 檢查的目錄，如 PHP 套件的放在 `vendor` 目錄下，實務上我們不會想讓 SonarQube 去檢查 package 的程式碼品質，所以會加以排除

> SonarQube 預設也會檢查 JavaScript，若你有 JavaScript 的套件目錄不想檢查，也可以設定在 `sonar.exclusions`

![hp02](/images/sonarqube/php/php023.png)

1. 在專案目錄下輸入 `sonar-scanner` 檢查 PHP

![hp02](/images/sonarqube/php/php024.png)

1. 若能看到 `EXECUTION SUCCESS`，則表示 SonarQube Scanner 安裝成功

![hp00](/images/sonarqube/php/php001.png)

進入 SonarQube 網頁，就可看到 `Laravel56` 專案已經出現 SonarQube。

到目前為止，SonarQube 對 PHP 的檢查已經完成，就算只將 SonarQube 裝在本機，也對 PHP 程式碼品質的檢查有很大的幫助。

## Jenkins

實務上 SonarQube 還會與其他 CI server 合作，接下來將以 Jenkins 為例，介紹如何以 Jenkins 自動化 SonarQube。

### Installation

```shell
$ brew install jenkins-lts
```

使用 Homebrew 安裝 Jenkins。

![hp02](/images/sonarqube/php/php021.png)

* 若想在每次 Mac 重開機就自動執行 Jenkins，輸入 `brew services start jenkins-lts`

> 若想自行啟動 Jenkins，輸入 `jenkins-lts`

### Start Jenkins

```
$ jenkins-lts
```

使用 `jenkins-lts` 自行啟動 Jenkins。

![hp02](/images/sonarqube/php/php026.png)

### Unlock Jenkins

![hp02](/images/sonarqube/php/php028.png)

1. 輸入 `localhost:8080`，看到 Unlock Jenkins，開始設定 Jenkins
2. 預設密碼放在 `./jenkins/secrets/initialAdminPassword`

### Customize Jenkins

![hp02](/images/sonarqube/php/php029.png)

1.  選擇 `Install suggested plugins` 即可

### Getting Started

![hp03](/images/sonarqube/php/php030.png)

安裝 default plugin 中。

### Create Admin User

![hp03](/images/sonarqube/php/php031.png)

* 建立管理者帳號

### Jenkins is Ready

![hp03](/images/sonarqube/php/php032.png)

* Jenkins 安裝完成，按 `Start using jenkins` 開始使用 Jenkins

### Welcome to Jenkins

![hp03](/images/sonarqube/php/php033.png)

* 進入 Jenkins 管理介面，如此 Jenkins 已經設定成功

## SonarQube Scanner for Jenkins

要讓 Jenkins 能自動執行 SonarQube Scanner，必須另外安裝 plugin。

### Installation

![hp01](/images/sonarqube/php/php011.png)

1. 左側選擇 `Manage Jenkins`
2. 右側選擇 `Manage Plugins`

![hp00](/images/sonarqube/php/php002.png)

1. 選擇 `SonarQube Scanner`
2. 按 `Download now and install after restart`

![hp00](/images/sonarqube/php/php003.png)

安裝 plugin 中。

* 將 `Restart Jenkins when installation is complete and no jobs are running` 打勾

### Configuration

![hp00](/images/sonarqube/php/php006.png)

1. 左側選擇 `Manage Jenkins`
2. 右側選擇 `Configure System`

![hp00](/images/sonarqube/php/php005.png)

1. 找到 `SonarQube servers` 區段
2. 將 `Enable injection of SonarQube server configuration as build environment variables` 打勾
3. 設定 SonarQube server 名稱與網址

最後按 `Apply` 與 `Save` 儲存設定。

### Environment Variable

![hp00](/images/sonarqube/php/php009.png)

1. 左側選擇 `Manage Jenkins`
2. 右側選擇 `Global Tool Configuration`

![hp00](/images/sonarqube/php/php008.png)

1. 找到 `SonarQube Scanner` 區段
2. 設定 SonarQube Scanner 名稱

最後按 `Apply` 與 `Save` 儲存設定。

## Jenkins Job

### Source Code Management

![hp00](/images/sonarqube/php/php007.png)

1. 選擇 Job，按左側 `Configure`

![hp02](/images/sonarqube/php/php027.png)

1. 找到 `Source Code Management` 區段
2. 將 Git 的 `Repository URL` 設定到 `http://github.com/oomusou/Laravel56Demo`


### Build

![hp00](/images/sonarqube/php/php004.png)

1. 找到 `Build` 區段
2. 在 `Add build step` 選擇 `Execute SonarQube Scanner`

![hp01](/images/sonarqube/php/php010.png)

```
sonar.projectKey=Laravel56 
sonar.sources=. 
sonar.projectName=Laravel56 
sonar.projectVersion=1.0 
sonar.exclusions=vendor/**
```

在 `Analysis properties` 加上以上設定。

```
~/MyProject $ sonar-scanner -Dsonar.projectKey=Laravel56 -Dsonar.sources=. -Dsonar.projectName=Laravel56 -Dsonar.projectVersion=1.0 -Dsonar.exclusions=vendor/**
```

還記得之前曾經下過以上指令嗎？就是將這些 SonarQube property 設定在 Jenkins，改由 Jenkins 幫我們傳。


![hp01](/images/sonarqube/php/php013.png)

* 按左側 `Build Now` 執行 Job

![hp01](/images/sonarqube/php/php012.png)

* 執行成功後會出現 `OK` 與 `Success`。

![hp01](/images/sonarqube/php/php014.png)

也會看到剛剛 Jenkins 執行的 Project 出現在 SonarQube 上。

## Conclusion

* SonarQube 已經內建 SonarPHP，也可以用來檢查 PHP
* 就算不將 SonarQube 安裝在 server，安裝在本機也能有效的檢查 PHP 程式碼品質
* 藉由 Jenkins 幫忙，我們就可以自動化執行 SonarQube

## Sample Code

完整的範例可以在我的 [GitHub](https://github.com/oomusou/Laravel56Demo) 上找到

## Reference

[SonarQube](https://www.sonarqube.org), [SonarQube Downloads](https://www.sonarqube.org/downloads/)
[SonarQube](https://docs.sonarqube.org/), [SonarPHP](https://docs.sonarqube.org/display/PLUG/SonarPHP)
[SonarQube](https://docs.sonarqube.org/), [Analyzing with SonarQube Scanner](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner)
[SonarQube](https://docs.sonarqube.org/), [Analying with SonarQube Scanner for Jenkins](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+Jenkins)
[Jenkins](https://jenkins.io/), [SonarQube Scanner](https://plugins.jenkins.io/sonar)