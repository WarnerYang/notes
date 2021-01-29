
# 常用git命令
- 工具下载 Git GUI [官网下载](https://git-scm.com/download/)

## 首先配置SHHkey

1. 创建SSH Key。在用户主目录下，看看有没有`.ssh`目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```bash
ssh-keygen -t rsa -C "your email" //这里换成自己github绑定的邮箱
```

然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

2. 登陆GitHub，打开“Account settings”，“SSH Keys”页面。然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容。

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。


## Git 全局设置:
```bash
//配置github用户名
git config --global user.name "WarnerYang"

//配置github邮箱
git config --global user.email "your email"

```

## 取消全局 username, email
```bash
git config --global --unset user.name
git config --global --unset user.email
```

## 本地创建git仓库（如：learn）:
```bash
mkdir learn    //创建目录
cd learn       //打开目录
git init       //初始化
touch README.md     //创建README文件
git add README.md   //添加
git commit -m "first commit" //提交及说明    
git remote add origin git@github.com:WarnerYang/learn.git //提交到远程
git push -u origin master   //推送

```

由于远程库是空的，我们第一次推送master分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`maste`r分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

## 如果在github上已经创建仓库（如：learn）
```bash
git remote add origin
git clone git@github.com:WarnerYang/learn.git  //从远程库克隆

```

如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。

你也许还注意到，GitHub给出的地址不止一个，还可以用`https://github.com/WarnerYang/wx.git`这样的地址。实际上，Git支持多种协议，默认的`git://`使用`ssh`，但也可以使用`https`等其他协议。

使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放`http`端口的公司内部就无法使用`ssh`协议而只能用`https`。

## 添加
```bash
git add 文件名或文件夹名  //提交整个目录 git add .
```

## 提交
```bash
git commit -m  "提交说明"
```

## 推送
```bash
git push  origin master
```

## 删除远程分支
```bash
git push origin --delete [branchName]
```

## 本地同步线上已经删除的分支
```bash
git remote prune origin
```

## 回滚到指定的版本
```bash
git reset --hard e377f60e28c8b84
```

## 让Git显示颜色
```bash
git config --global color.ui true
```

## 配置别名
```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
```
## windows配置别名
```bash
alias ll='ls -l'
```

## gitk 乱码修改
windows找到git配置文件
`C:\Users\WarnerYang\.gitconfig`

在末尾添加
```bash
[gui]
 	encoding = utf-8
```

## 如何 clone git 项目到一个非空目录
```bash
如果我们往一个非空的目录下 clone git 项目，就会提示错误信息：

fatal: destination path '.' already exists and is not an empty directory.

解决的办法是：

1. 进入非空目录，假设是 /d/WWW/dnmp

2. git clone --no-checkout git@github.com:WarnerYang/dnmp.git temp

3. mv tmp/.git .   #将 tmp 目录下的 .git 目录移到当前目录

4. rmdir tmp

5. git reset --hard HEAD
```

## gitignore 不起作用的解决办法 && 已经上传的怎么加入gitignore
```bash
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
```

## Git多账号配置

https://www.cnblogs.com/popfisher/p/5731232.html

前提要取消全局 username, email

配置config
```bash
Host github.com
	HostName  github.com
	User      Warner
	IdentityFile  C:/Users/Warner/.ssh/id_rsa_github

Host newtype
	HostName  gitlab.atsolution.com.hk
	User      Warner
	IdentityFile  C:/Users/Warner/.ssh/id_rsa
```

再单独配置仓库
```bash
git config user.name "Warner"
git config user.email "your email"
```

```bash
git config user.name "Warner"
git config user.email "warner.yang@newtype.io"
```

## 获取最新tag
```bash
git describe --tags `git rev-list --tags --max-count=1`
```

## 基于最新tag拉取分支
```bash
git checkout -b ATBAU-XXX $(git describe --tags $(git rev-list --tags --max-count=1))
```

## 暂存
```bash
git stash
git stash pop  //恢复并删除
git stash apply //恢复
git stash list
git stash drop //删除
```

## 推送tag
```bash
//打tag
git tag v1.0
// push单个tag，命令格式为：git push origin [tagname]
git push origin v1.0
// push所有tag，命令格式为：git push [origin] --tags
git push origin --tags
```

## 回滚已经push的提交(没有提交记录)
```bash
git checkout -b origin develop
git reset --hard f3769a22c2fab287530e3c2d4e406bc2724e3ca7
git push origin develop --force
```

## 回滚错误的分支合并（生成新的提交）
```bash
git revert <版本号>  //这里是指有bug那个commit
```

## 查看本地分支和远程分支的跟踪关系
```bash
git branch -vv
```

## 停止追踪分支
```bash
git branch --unset-upstream
```

## 停止追踪已经记录的文件
```bash
git rm -r --cached [filename]
```

## 查看分支历史
```bash
git log | grep ACP-1234
```

## 查看分支有没有合并进来
```bash
git branch --merged
git branch --no-merged
```

## vscode 提示 would clobber existing tag
```bash
git fetch --tags -f
```

>[参考](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)