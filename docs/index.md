# Index

## 说明

记录安全学习笔记。持续更新。

界面：https://h4m5t.github.io/SecNotes/

源地址：https://github.com/h4m5t/SecNotes


## 待学习/复习
### 工具
* Mysql
* Fuzz
* shellcode编写
* 免杀
* wireshark
* sqlmap
* wfuzz
* Ffuf
* Pwntools
* netcat/ncat/socat
* ettercap
* arpspoof
* postman
* vulnhub
* RASP
* Nuclei
* httpX
* Django
* AWVS
* Nessus
* log4j
* Dnslog
* Cobalt Strike
* C2:Empire，Sliver,Covenant,Villain,Havoc
* honeypot:Tpot,cowrie,dionaea,mhn,Ehoney
* NIST
* Yakit

### 靶场
* DVWA
* bWAPP
* vulstack
* Vulhub
* Vulnhub
* vulstudy
* vulfocus
* vulapps

## 文档构建

> GithubPages+Mkdocs+mkdocs-material+GitHubActions


### with GitHub Actions

Using [GitHub Actions](https://github.com/features/actions) you can automate the deployment of your project documentation. At the root of your repository, create a new GitHub Actions workflow, e.g. `.github/workflows/ci.yml`, and copy and paste the following contents:

```
name: ci 
on:
  push:
    branches:
      - master 
      - main
permissions:
  contents: write
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: 3.x
      - run: echo "cache_id=$(date --utc '+%V')" >> $GITHUB_ENV 
      - uses: actions/cache@v3
        with:
          key: mkdocs-material-${{ env.cache_id }}
          path: .cache
          restore-keys: |
            mkdocs-material-
      - run: pip install mkdocs-material 
      - run: mkdocs gh-deploy --force
```

Now, when a new commit is pushed to either the `master` or `main` branches, the static site is automatically built and deployed. Push your changes to see the workflow in action.

If the GitHub Page doesn't show up after a few minutes, go to the settings of your repository and ensure that the [publishing source branch](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site) for your GitHub Page is set to `gh-pages`.

Your documentation should shortly appear at `<username>.github.io/<repository>`.

### with MkDocs

If you prefer to deploy your project documentation manually, you can just invoke the following command from the directory containing the `mkdocs.yml` file:

```
mkdocs gh-deploy --force
```



## Referecnes

https://github.com/Bypass007/Safety-Project-Collection
https://github.com/satan1a/TheRoadOfSO
https://github.com/satan1a/awesome-cybersecurity-blueteam-cn

![](assets/images/test.jpg)

显示图片测试：assets/images/test.jpg



## Release Notes

2023/5/24

搭建文档，构建自动化发布流

2023/5/25

加入一些安全相关学习资料

2023/5/26

将文档换成文件夹内的形式，将资料放到对应文件夹，方便管理。

更换网页logo和favicon

<img src="assets/logo.png" style="zoom:20%;" align=left />


2023/6/23
增加企业安全建设几篇文章

