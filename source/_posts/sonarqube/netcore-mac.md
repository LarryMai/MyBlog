title: 如何使用 SonarQube 檢查 .NET Core 專案？(macOS)
tags:
  - SonarQube
  - .NET Core
  - Jenkins
  - macOS
  - Homebrew
feature: images/feature/sonarqube.png
date: 2019-05-19 10:51:22
---
SonarQube 已經內建 SonarC#，可以直接對 C# 進行檢查，本文將以 .NET Core 為例，並搭配 Jenkins 自動執行 SonarQube。

<!-- more -->

## Version

macOS High Sierra 10.13.4
SonarQube 7.1
Jenkins 2.107.1
.NET Core 2.0.7

## GitHub

將 .NET Core 專案放到 GitHub。

![core000](/images/sonarqube/netcore-mac/core000.png)

1. 本文將 .NET Core 專案放在 `https://github.com/oomusou/Core2SonarQubeJenkins`

> 當然也可以將 git repository 放在不同的 git server，如 Bitbucket

## SonarQube

### Installation

```shell
$ brew install sonarqube
```

使用 Homebrew 安裝 SonarQube。

![core001](/images/sonarqube/netcore-mac/core001.png)

* 若想在每次 Mac 重開機就自動執行 SonarQube，輸入 `brew services start sonarqube`
* 若想自行啟動 SonarQube，輸入 `sonar console`

### Start SonarQube

```shell
$ sonar console
```

使用 `sonar console` 自行啟動 SonarQube。

![core002](/images/sonarqube/netcore-mac/core002.png)

### Test SonarQube

![core007](/images/sonarqube/netcore-mac/core007.png)

輸入 `localhost:9000`，若看到 SonarQube 首頁，則表示安裝成功

> 右上角 `Log in` 可登入管理設定 SonarQube，預設為 `admin/admin`

## SonarQube Scanner for MSBuild

SonarQube 雖然已經包含 SonarC#，但必須靠 SonarQube Scanner for MSBuild 才能執行，預設 SonarQube 並沒有包含 Scanner，必須自行安裝。

> 其實 SonarQube Scanner for MSBuild 包含 SonarQube Scanner，只是為了 C# 要編譯的特性再包了一層

### Download Scanner

![hp00](/images/sonarqube/netcore-mac/core004.png)

* 到 [Analyzing with SonarQube Scanner for MSBuild](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+MSBuild) 下載 Scanner，選擇 `Download for .NET core 2.0` 下載。

![lack00](/images/sonarqube/netcore-mac/core005.png)

下載後為一 `zip` 壓縮檔，解壓縮後可安裝在任何目錄。

![lack00](/images/sonarqube/netcore-mac/core006.png)

將 `zip` 解開後，放到 home directory 下，並重新命名為 `SonarScannerMsbuild` 。

> 重新命名只為了縮短目錄名稱而已

### Configuration

**SonarQube.Analysis.xml**

```xml
<Property Name="sonar.host.url">http://localhost:9000</Property>
<Property Name="sonar.login">admin</Property>
<Property Name="sonar.password">admin</Property>
```

編輯 `～/SonarScannerMsbuild/SonarQube.Analysis.xml` ，修改 `sonar.host.url`、`sonar.login` 與 `sonar.password` 三個 property。

![core008](/images/sonarqube/netcore-mac/core008.png)

1. 編輯 `SonarQube.Analysis.xml`
2. 預設是註解，將註解拿掉設定 server 與 login

### Test Scanner

```shell
$ dotnet ~/SonarScannerMsbuild/SonarScanner.MSBuild.dll begin /k:core2 /n:Core2 /v:1.0
```
使用 SonarQube Scanner for MSBuild 對 .NET Core 專案進行檢查。

由於 .NET Core 是跨平台，`SonarScanner.MSBuild.dll` 必須由 `dotnet` 執行。

* `SonarScanner.MSBuild.dll` 需指定完整路徑，無法透過 `$PATH` 設定
* 需加上 `begin`，`dotnet build` 必須包在 scanner 內
* **/k**：SonarQube 對專案的 key，內部將以此 key 作為辨別，必須唯一
* **/n**：在 SonarQube 網頁上顯示的專案名稱
* **/v**：在 SonarQube 網頁上顯示的版本編號


![lack00](/images/sonarqube/netcore-mac/core009.png)

1. 在專案目錄下使用 `SonarScanner.MSBuild.dll` 檢查 C#

```shell
$ dotnet build
```

使用 `dotnet build` 編譯專案。

> Script 類不用編譯，可以直接使用 SonarQube Scanner 就可以檢查，但 C# 需要編譯，因此必須 dotnet build

![lack01](/images/sonarqube/netcore-mac/core010.png)

1. 輸入 `dotnet build` 編譯專案
2. Scanner 檢查出警告

```shell
$ dotnet ~/SonarScannerMsbuild/SonarScanner.MSBuild.dll end
```

* 需加上 `end`，scanner 正式將 `dotnet build` 檢查出的結果寫入 SonarQube project

![core054](/images/sonarqube/netcore-mac/core012.png)

* 在專案目錄下使用 `SonarScanner.MSBuild.dll` 結束檢查

![lack01](/images/sonarqube/netcore-mac/core011.png)

進入 SonarQube 網頁，就可看到 `Core2` 專案已經出現 SonarQube，也顯示剛剛 `dotnet build` 檢查出的 `1` 個 code smell 警告。

到目前為止，SonarQube 對 C# 的檢查已經完成，就算只將 SonarQube 裝在本機，也對 C# 程式碼品質的檢查有很大的幫助。

若能搭配 Jenkins 自動執行 SonarQube，那就更好了。

## Jenkins

### Installation

```shell
$ brew install jenkins
```

使用 Homebrew 安裝 Jenkins。

![lack02](/images/sonarqube/netcore-mac/core013.png)

* 若想在每次 Mac 重開機就自動執行 Jenkins，輸入 `brew services start jenkins`

* 若想自行啟動 Jenkins，輸入 `jenkins`

### 啟動 Jenkins

```shell
$ jenkins
```

使用 `jenkins` 自行啟動 Jenkins。

![lack02](/images/sonarqube/netcore-mac/core014.png)

### Unlock Jenkins

![lack03](/images/sonarqube/netcore-mac/core015.png)

1. 輸入 `localhost:8080`，看到 Unlock Jenkins，開始設定 Jenkins
2. 預設密碼放在 `~/jenkins/secrets/initialAdminPassword`
3. 將預設密碼貼上
4. 按 `Continue` 繼續

### Customize Jenkins

![lack03](/images/sonarqube/netcore-mac/core016.png)

1. 選擇 `Install suggested plugins` 即可

### Getting Started

![lack03](/images/sonarqube/netcore-mac/core017.png)

安裝 suggested plugin 中。

### Create Admin User

![lack03](/images/sonarqube/netcore-mac/core018.png)

1. 建立管理者帳號
2. 按 `Save and Finish` 繼續

### Jenkins is Ready

![lack03](/images/sonarqube/netcore-mac/core019.png)

1. Jenkins 安裝完成，按 `Start using jenkins` 開始使用 Jenkins

### Welcome to Jenkins

![lack03](/images/sonarqube/netcore-mac/core020.png)

1. 進入 Jenkins 管理介面，如此 Jenkins 已經設定成功

## Jenkins Job

### Create Job

![core028](/images/sonarqube/netcore-mac/core028.png)

* 按左側 `New Item` 建立新 job


![core028](/images/sonarqube/netcore-mac/core029.png)

1. 輸入 Job 名稱
2. 選擇 `Freestyle project`
3. 按 `OK` 繼續

### Source Code Management

![lack04](/images/sonarqube/netcore-mac/core030.png)

1. 找到 `Source Code Management` 區段
2. 將 Git 的 `Repository URL` 設定到 `https://github.com/oomusou/Core2SonarQubeJenkins`


### Build Environment

![lack04](/images/sonarqube/netcore-mac/core031.png)

1. 找到 `Build Environment` 區段
2. 勾選 `Delete workspace before build starts`

### Build

![lack04](/images/sonarqube/netcore-mac/core032.png)

1. 找到 `Build` 區段
2. 選擇 `Add build step`
3. 新增 `Execute shell`

![lack04](/images/sonarqube/netcore-mac/core033.png) 

1. 講以下 command 貼上


```shell
dotnet /Users/oomusou/SonarScannerMsbuild/SonarScanner.MSBuild.dll begin /k:core2 /n:Core2 /v:1.0
dotnet build
dotnet /Users/oomusou/SonarScannerMsbuild/SonarScanner.MSBuild.dll end
```

2. 按 `Save` 儲存設定


![lack04](/images/sonarqube/netcore-mac/core021.png)

1. 按左側 `Build Now` 執行 Job


![lack04](/images/sonarqube/netcore-mac/core022.png)

1. Job 執行成功會出現 `藍燈`

![lack04](/images/sonarqube/netcore-mac/core023.png)

剛剛 Jenkins 執行的 Project 出現在 SonarQube 上。

## Conclusion

- SonarQube 已經內建 SonarC#，也可以用來檢查 C# 與 .NET Core
- 就算不將 SonarQube 安裝在 server，安裝在本機也能有效的檢查 C# 程式碼品質
- 藉由 Jenkins 幫忙，我們就可以自動化執行 SonarQube

## Sample Code

完整的範例可以在我的 [GitHub](https://github.com/oomusou/Core2SonarQubeJenkins) 上找到

## Reference

[SonarQube](https://docs.sonarqube.org/), [Analyzing with SonarQube Scanner for MSBuild](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+MSBuild)
[SonarQube](https://docs.sonarqube.org/), [Analying with SonarQube Scanner for Jenkins](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner+for+Jenkins)

