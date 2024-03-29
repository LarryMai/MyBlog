title: 如何在 Docker 使用 SonarQube 顯示 .NET Core 專案 Code Coverage ?
tags:
  - SonarQube
  - .NET Core
  - Docker
  - NUnit
  - Coverlet
  - OpenCover
  - Rider
feature: images/feature/sonarqube.png
date: 2019-05-19 15:49:51
---
SonarQube 除了能檢查 C# 品質外，還可以當成 Dashboard 使用，讓整個團隊有統一入口關注專案的 Code Coverage；SonarQube 支援 OpenCover 與 dotCover 兩種格式，本文將以 NUnit + Coverlet + OpenCover 介紹。

<!-- more -->

## Version

macOS High Sierra 10.13.6
Docker for Mac 18.06.0-ce-mac70 (26399)
.NET Core 2.1
SonarQube 7.1
SonarScanner 4.3.1.1372
NUnit 3.10.1
Coverlet 2.1.1
Rider 2018.1.3

## Coverlet

![coverage000](/images/sonarqube/code-coverage/coverage000.png)

在 NUnit 專案加入 Coverlet，這是個基於 .NET Core，且能跨平台計算 code coverage 的 package。

> OpenCover 與 dotCover 算 .NET 生態圈兩大最有名的 package，但目前都只能跑在 Windows 平台，Coverlet 算目前跨平台最佳 solution

## Dockerfile

```dockerfile
FROM microsoft/dotnet

ENV SCANNER_VERSION=4.3.1.1372
ENV SCANNER_HOME=/opt/scanner

WORKDIR /app

RUN apt-get update 
RUN apt-get install -y wget
RUN apt-get install -y unzip
RUN apt-get install -y openjdk-8-jre
RUN wget -q https://github.com/SonarSource/sonar-scanner-msbuild/releases/download/$SCANNER_VERSION/sonar-scanner-msbuild-$SCANNER_VERSION-netcoreapp2.0.zip -O /opt/sonar-scanner-msbuild.zip
RUN mkdir -p $SCANNER_HOME
RUN unzip /opt/sonar-scanner-msbuild.zip -d $SCANNER_HOME
RUN rm /opt/sonar-scanner-msbuild.zip
RUN chmod 775 $SCANNER_HOME/ -R

COPY ./ ./
ENTRYPOINT ["./scanner.sh"]
```

因為要在 .NET Core container 安裝 SonarScanner，所以要對 .NET Core 客製化 image。

第 1 行

```dockerfile
FROM microsoft/dotnet
```

使用 `FROM` 設定所使用的基底 image。

使用 Microsoft 官方的 `microsoft/dotnet` image。

第 3 行

```dockerfile
ENV SCANNER_VERSION=4.3.1.1372
ENV SCANNER_HOME=/opt/scanner
```

使用 `ENV` 設定 dockerfile 的環境變數。

`SCANNER_VERSION` : 因為 SonarScanner 的下載路徑，會與版本有關，所以特別設定成變數。

`SCANNER_HOME` : 設定 SonarScanner 所安裝的目錄。

第 6 行

```dockerfile
WORKDIR /app
```

設定 container 的工作目錄，也就是預設目錄都會在 `/app` 下。

第 8 行

```dockerfile
RUN apt-get update 
RUN apt-get install -y wget
RUN apt-get install -y unzip
RUN apt-get install -y openjdk-8-jre
```

使用 `RUN` 執行 CLI 指令。

* 使用 `wget` 下載 SonarScanner
* 使用 `unzip` 對 zip 解壓縮
* `SonarScanner` 會使用 Java 執行，需要安裝 Java Runtime

12 行

```dockerfile
RUN wget -q https://github.com/SonarSource/sonar-scanner-msbuild/releases/download/$SCANNER_VERSION/sonar-scanner-msbuild-$SCANNER_VERSION-netcoreapp2.0.zip -O /opt/sonar-scanner-msbuild.zip
```

使用 `wget` 下載 SonarScanner 壓縮檔，並下載到 `/opt` 目錄下。

13 行

```dockerfile
RUN mkdir -p $SCANNER_HOME
RUN unzip /opt/sonar-scanner-msbuild.zip -d $SCANNER_HOME
RUN rm /opt/sonar-scanner-msbuild.zip
```

建立 `/opt/scanner` 目錄，將 SonarScanner 壓縮檔解壓縮放到該目錄下，解壓縮完刪除 SonarScanner 壓縮檔。

16 行

```dockerfile
RUN chmod 775 $SCANNER_HOME/ -R
```

將 SonarScanner 目錄所有檔案賦予執行權力。

18 行

```dockerfile
COPY ./ ./
```

將目前 solution 下所有檔案複製到 container 內，也就是 `/app` 目錄下。

19 行

```dockerfile
ENTRYPOINT ["./scanner.sh"]
```

最後會執行專案目錄下的 `./scanner.sh`，負責執行 SonarScanner 檢查。

## Docker-compose.yml

```yaml
version: "3"

services:
  net-core:
    build: .
    container_name: MyCore
    volumes:
      - "${HOST_DIR}:/code/"
    networks:
      - netcore
    depends_on:
      - sonarqube

  sonarqube:
    image: sonarqube:latest
    container_name: MySonarQube
    ports:
      - 9000:9000
      - 9002:9002
    networks:
      netcore:
        ipv4_address: 172.16.238.10
     
networks:
  netcore:
    ipam: 
      driver: default
      config:
        - subnet: 172.16.238.0/24
```

設定 .NET Core 與 SonarQube 兩個 container 同時啟動。

第 4 行

```yaml
net-core:
  build: .
```

使用 `build` 將同目錄下的 `Dockerfile` build 成 image，也就是剛才的 `Dockerfile`。

第 6 行

```yaml
container_name: MyCore
```

使用 `container_name` 設定 .NET Core 的 container 名稱。

第 7 行

```yaml
volumes:
  - "${HOST_DIR}:/code/"
```

使用 `volumes` 設定 host 與 container 的共用目錄，`:` 左方為 host 目錄，右方為 container 目錄。

目的是能在 .NET Core container 內抓到 host 的 project。

第 9 行

```yaml
networks:
  - netcore
```

使用 `networks` 設定 .NET Core 與 SonarQube 共用 `netcore` 內部網路。

11 行

```yaml
depends_on:
  - sonarqube
```

.NET Core container 相依於 SonarQube container。

14 行

```yaml
sonarqube:
  image: sonarqube:latest
```

使用 `image` 設定 container 所使用的 image。

16 行

```yaml
container_name: MySonarQube
```

使用 `container_name` 設定 SonarQube 的 container 名稱。

17 行

```yaml
ports:
  - 9000:9000
  - 9002:9002
```

使用 `ports` 設定 host 與 container 所 mapping 的 port，`:` 左側為 host port，右側為 container port。

為了能在 host 以瀏覽器連上 SonarQube，要將 host 的 9000 與 9002 兩個 port 給 host。

20 行

```yaml
networks:
  netcore:
    ipv4_address: 172.16.238.10
```

使用 `network` 設定使用 `netcore` 網路，並設定其 IP 為 `172.16.238.10`。

> 一般來說，我們不必為 container 設定固定 IP，只要使用 container 名稱就可彼此溝通，但稍後 SonarScanner 必須使用固定 IP 才能連上 SonarQube，所以在此特別要設定固定 IP

24 行

```yaml
networks:
  netcore:
    ipam: 
      driver: default
      config:
        - subnet: 172.16.238.0/24
```

設定 `netcore` 網路。

## .env

```yaml
HOST_DIR=~/Code/CSharp
```

將 `HOST_DIR` 以環境變數設定，為 host 與 .NET Core container 所共享的目錄。

## scanner.sh

```bash
#! /bin/bash
sleep 35
cd ./ClassLib.Test
dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=opencover 
cd ..
dotnet /opt/scanner/SonarScanner.MSBuild.dll begin /k:core2 /n:Core2 /v:1.0 /d:sonar.login=admin /d:sonar.password=admin /d:sonar.host.url=http://172.16.238.10:9000 /d:sonar.cs.opencover.reportsPaths=/app/ClassLib.Test/coverage.opencover.xml /d:sonar.coverage.exclusions=**/Program.cs
dotnet build
dotnet /opt/scanner/SonarScanner.MSBuild.dll end /d:sonar.login=admin /d:sonar.password=admin 
```

使用 SonarScanner 檢查 .NET Core 專案，`scanner.sh` 會放在專案根目錄。

第 2 行

```bash
sleep 30
```

主要是要等 SonarQube 啟動完成，比較好的方式是使用 health check 明確得知 SonarQube 已經啟動完成。

這裡暫時先 sleep 30 秒，等 SonarQube 先啟動，.NET Core 才開始執行 SonarScanner。

這裡還有改善的空間，也可以自行調整 sleep 時間。

第 4 行

```bash
dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=opencover 
```

執行 NUnit 單元測試。

* **/p:CollectCoverage=true** : 命令 Coverlet 計算 code coverage
* **/p:CoverletOutputFormat=opencover** : 命令 Coverlet 以 OpenCover 格式產出

第 6 行

```bash
dotnet /opt/scanner/SonarScanner.MSBuild.dll begin /k:core2 /n:Core2 /v:1.0 /d:sonar.login=admin /d:sonar.password=admin /d:sonar.host.url=http://172.16.238.10:9000 /d:sonar.cs.opencover.reportsPaths=/app/ClassLib.Test/coverage.opencover.xml /d:sonar.coverage.exclusions=**/Program.cs
```

使用 `dotnet` 執行 `SonarScanner.MSBuild.dll`，一開始以 `begin` 開頭：

* **/k** : SonarQube 對專案的 key，內部將以此 key 作為辨別，必須唯一
* **/n** : 在 SonarQube 網頁上顯示的專案名稱
* **/v** : 在 SonarQube 網頁上顯示的版本編號
* **/d:sonar.login** : 指定 SonarQube 帳號
* **/d:sonar.password** : 指定 SonarQube 密碼
* **/d:sonar.host.url** : 指定 SonarQube server IP 與 port
* **/d:sonar.cs.opencover.reportsPaths** : 設定 OpenCover 格式的 XML 檔案位置
* **/d:sonar.coverage.exclusions** : 設定 SonarQube 排除 code coverage 計算的檔案

> 目前 SonarQube 設定 exclusions 的 regex 有些 bug，可能無法如文件那般設定 regex，假如遇到這個 bug，就先用最原始的方式指定 exclusions

第 7 行

```bash
dotnet build
```

使用 `dotnet build` 編譯專案。

> Script 語言不用編譯，可以直接使用 SonarQube Scanner 就可以檢查，但 C# 需要編譯，因此必須 dotnet build

第 8 行

```bash
dotnet /opt/scanner/SonarScanner.MSBuild.dll end /d:sonar.login=admin /d:sonar.password=admin 
```

最後需加上 `end`，scanner 正式將 `dotnet build` 檢查出的結果寫入 SonarQube project

* 仍必須指定 SonarQube 的帳號密碼

## Run SonarScanner

```bash
SonarScanner $ docker-compose up --build
```
啟動 .NET Core 與 SonarQube container，並且自動執行 SonarScanner。

* **--build** : 重新建立 .NET Core 自訂 image，若有任何修改，將重新包進 image，確保 `docker-compose` 是最新的 image

![core000](/images/sonarqube/code-coverage/coverage001.png)

![coverage002](/images/sonarqube/code-coverage/coverage002.png)

1. Coverlet 計算出 code coverage，並且產生 `coverage.opencover.xml`

![coverage003](/images/sonarqube/code-coverage/coverage003.png)

1. 在 host 以 `http://localhost:9000` ，將可看到 SonarQube 的檢查結果，並且正確顯示 code coverage

## Stop Container

```bash
$ docker-compose down
```

![core003](/images/sonarqube/code-coverage/coverage004.png)

## Conclusion

* 由於 SonarScanner 已經被包在 dockerfile 內，host 就不用再安裝 SonarScanner
* 沒有將 .NET Core 安裝在 SonarQube container 內，而是分別使用 .NET Core container 與 SonarQube container，符合 microservice 精神
* Coverlet 為目前唯一的 .NET Core 跨平台 code coverage 解決方案，並且可以產出 OpenCover 格式 XML
* 只要執行 `docker-compose up --build`，就會執行 NUnit 與 Coverlet 計算 code coverage，且 SonarScanner 也會自動進行檢查

## Sample Code 

完整範例可以在我的 [GitHub](https://github.com/oomusou/sonarscanner) 上找到

## Reference

[Tonerdo](https://github.com/tonerdo), [Coverlet](https://github.com/tonerdo/coverlet)
[Shryne Boyer](https://tattoocoder.com), [Cross platform code coverage arrives for .NET Core](https://tattoocoder.com/cross-platform-code-coverage-arrives-for-net-core/)
[SonarQube](https://docs.sonarqube.org), [Code Coverage Result Import (C#, VB.NET)](https://docs.sonarqube.org/pages/viewpage.action?pageId=6389770)