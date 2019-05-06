title: 如何將 Suka 發佈到 GitHub ?
tags:
  - Hexo
  - Suka
feature: images/feature/suka.png
date: 2019-05-02 13:23:43
---
當使用 Suka 快速建立 Blog 後，最後一哩路就是發佈到 GitHub。

<!-- more -->

## Version

Hexo 3.8.0

## Hexo-deployer-git

```
$ yarn add hexo-deployer-git
```

Hexo 預設並沒有包含發佈到 GitHub 的 package，需另外安裝。

使用 Yarn 安裝 `hexo-deployer-git`。

![deploy000](/images/suka/deployment/deploy000.png)

## Configuration

**_config.yml**

```ymal
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:oomusou/oomusou.github.io.git
```

在 blog 根目錄的 `_config.yml` 的 `deploy` section，將 `type` 設定為 `git`，將 `repo`設定到 GitHub 的 repository。

![deploy002](/images/suka/deployment/deploy002.png)

## Deploy to GitHub

```
$ hexo deploy
```

![deploy003](/images/suka/deployment/deploy003.png)

![deploy001](/images/suka/deployment/deploy001.png)

Suka 成功發佈到 GitHub。

## Conclusion

* 發佈到 GitHub 屬於 Hexo 功能，Suka 並沒有著墨，與其他 theme 發佈方式一樣

## Reference

[Hexo](https://hexo.io), [Deployment](https://hexo.io/docs/deployment)