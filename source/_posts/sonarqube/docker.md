title: 如何使用 Docker 安裝 SonarQube？
tags:
  - SonarQube
  - Docker
feature: images/feature/sonarqube.png
date: 2019-05-19 09:38:51
---
SonarQube 是一套 `程式碼品質檢查工具`，可以幫我們檢查 Code 的 Bugs、 Vulenrability、Code Smell 與 Duplication，也屬於 `持續整合` 重要一環，亦可使用 Docker 安裝，將來管理會更加容易。

<!-- more -->

## Version

Docker for Mac 18.03.0-ce-mac59 (23608)
SonarQube 6.7.2 (build 37468)

## Download Image

```shell
$ docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube:lts
```

使用 `docker run` 下載 image 並建立 container 並執行之。

* **-d**：`d` etach，建立 container 後，就脫離目前 process
* **--name**：替 container 取一個人能夠識別的名字
* **-p**：Docker 外部與 SonarQube 內部所對應的 port，其中左邊為外部 Docker 的 port，右邊為 SonarQube 內部的 port
* **sonarqube:lts**：SonarQube 的 LTS 版本，目前為 `6.7.2`

 若要下載最新版 SonarQube，可使用以下指令：

```shell
$ docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube
```

不指定為 LTS 版，則下載最新版 SonarQube。

![ocker00](/images/sonarqube/docker/docker000.png)

## Start SonarQube

```shell
$ docker start sonarqube
```

使用 `docker start` 啟動 SonarQube container。

![ocker00](/images/sonarqube/docker/docker003.png)

## Test SonarQube

![ocker00](/images/sonarqube/docker/docker001.png)

輸入 `localhost:9000`，若看到 SonarQube 首頁，則表示安裝成功。

## Stop SonarQube

```shell
$ docker stop sonarqube
```

使用 `docker stop` 停止 SonarQube container。

![ocker00](/images/sonarqube/docker/docker002.png)

## Conclusion

* 雖然在 macOS 安裝原生的 SonarQube 並不難，但使用 Docker 安裝 SonarQube 更簡單，且管理更方便

