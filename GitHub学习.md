# GitHub

## Git的入门使用

+ **Git的开始**

桌面上，右键 `Git Bash Here` 打开Git控制台；

配置用户名MIhu： `git config --global user.name MIhu' `

配置邮箱（不需要真实存在）：`git config --global user.email mihu@123.com`

+ **Git实现代码管理**

有两种方式：`git clone` 及 `git init`

1. `git clone`  （从github（或其他支持git的代码托管网站）上下载源码的管理）

在想要保存源码的地方，右键 `Git Bash Here` 打开Git控制台，在控制台输入 `git clone +源代码链接`   即可

2. `git init` （自己写的代码）

在想要保存代码的文件夹中，打开控制台，并输入 `git init` ，此时完成.git文件夹的建立（不要直接擅自更改）；

代码写完后，在当前文件夹中在控制台中输入 `git add .` (把当前文件夹内的所有文件和非空文件夹设置为准备提交的状态，不可省)，再输入 `git commit -m "备注" ` （备注中可写如“功能1已完成”提示性信息；提交成功后git会把源代码以数据库的形式保存在仓库中），`git push` 可将本地代码上传到github上。输入 `git log` 可查看提交的历史记录。

`git pull`可拉取github代码。（github代码若有更改，需要先拉去再上传自己编辑后的代码即先pull后push）

若代码不小心被修改，在控制台中输入 `git checkout HEAD main.py ` ,可从最后一次的提交里把main.py复制到文件夹的工作区（会覆盖），由此恢复文件。

**备注：**

若只想对文件夹中某一个文件夹提交到仓库中，可在控制台中输入 `git add main.py` 、`git commit -m "功能2已完成" `

## 流畅访问GitHub

+ **网易UU加速器**

在官网下载PC高清探索版，安装后打开。

登录用户后，搜索学术，选择学术资源，几秒钟后学术资源加速即可开启，之后可流畅使用Github.

+ **steam++**

在steampp.net网站下载安装、打开。

在左侧下滑找到Github，右侧点击全部启用，最后点击左上角一键加速即可。
