---
layout: post
title: 将本地新项目上传到GitHub步骤
category: other
tags: [other]
---

## 将本地新项目上传到GitHub步骤

第一步
打开Github网站登陆自己账号进入

第二步
点击 Your profile 新建仓库

第三步：
创建仓库名

第四步：<br/>
1.在命令行中，输入“git init”，使Test文件夹加入git管理；<br/>
2.输入“git add .”（不要漏了“.”），将Test文件夹全部内容添加到git。<br/>
3.输入“git commit -m "first commit"”（“git commit -m "提交信息"”）。<br/>
4.输入“git remote add origin https://github.com/username/Test.git”（git remote add origin 你自己的https地址），连接你的guthub仓库。<br/>
5.输入“git push -u origin master”，上传项目到Github。这里会要求输入Github的账号密码，按要求输入就可以。<br/>

在第五步push时候报错截图
```
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
解决办法：<br/>
$ git pull origin master<br/>
$ git push origin master<br/>

### 添加文件到git命令
git add 文件名 -f<br/>
git commit -m '添加备注’<br/>
git pull<br/>
git push <br/>
用户名<br/>
密码<br/>