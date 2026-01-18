# xhu-acm
西华大学acm基础知识库

### git的使用说明

方式一：直接在网页上创建一个新的分支更改。

方式二：拉取项目到本地做修改

这里只介绍方式二

1. 获取项目

```
git clone https://github.com/xhu-acm/xhu-acm.git
```
在本地随便一个目录（比如projects）下打开cmd,输入上面的命令。成功之后该仓库就在这个目录下面了。

<img width="1143" height="392" alt="image" src="https://github.com/user-attachments/assets/13ab4da2-ef87-4745-8d54-e0c795e250cf" />

2. 写题解要求

这里我们先打开本地该项目的文件夹，这里我推荐直接vscode打开，这里vscode可以不用去配c++的环境下载好直接用就可以了。

vscode打开项目之后是这样的：

<img width="2560" height="1368" alt="image" src="https://github.com/user-attachments/assets/f31f93ef-0693-493e-9250-0bd8e0f91b96" />

打开cmd：

<img width="2560" height="1368" alt="image" src="https://github.com/user-attachments/assets/778235f9-4aa8-4dde-8e73-ebb165365132" />

在命令行输入：
```
git branch
```
<img width="581" height="184" alt="image" src="https://github.com/user-attachments/assets/4cdeab2b-87db-404c-b267-8a776933c6ec" />

可以看到现在我们在main分支

一般我们不要在main分支上修改，自己新建一个分支在修改。比如下面的命令，以当前分支为基础创建一个名为test-shn的分支并切换过去（注意为了好看是谁写的分支可以加一个 '-你的名字' 的后缀）：
```
git checkout -b test-shn
```
<img width="629" height="146" alt="image" src="https://github.com/user-attachments/assets/4c526535-12a2-44f7-a224-3188ce696d7d" />

写下写题解的话直接在算法题的目录下创建一个以自己名字命名的文件夹之后你的所有题解都写在这个文件夹下面（题解必须是 .md 结尾）。

比如我要写一个题目名为： 小红的字符串构造（hard） 的题解

<img width="656" height="113" alt="image" src="https://github.com/user-attachments/assets/2c4475ec-4dfd-4530-8179-f1db983c8301" />

<img width="1060" height="584" alt="image" src="https://github.com/user-attachments/assets/f3ad6310-4811-402b-a006-52822f1a49f5" />

这里每个题解都要创一个新的分支（分支名和题目名一样）。

自己了解一下下面的命令

```
git branch
git checkout -b
git checkout
git fetch
git rebase
git reset --hard
git status
git add
git commit -m
git commit --amend
git push origin 分支名
git push origin 分支名 -f
```

3. 写好题解之后推到远程准备合并

执行下面的命令保存：
```
git add 你的题解的路径写到这里
git commit -m "这里里面写简要介绍"
git push origin 分支名
```
<img width="1086" height="650" alt="image" src="https://github.com/user-attachments/assets/6a35f054-653d-427f-9d12-11a0f175fb1c" />

这样就成功推到远端了。

<img width="2560" height="1368" alt="image" src="https://github.com/user-attachments/assets/1e73a586-f686-4122-add0-79d7ffa81244" />

然后之后pull request

<img width="2560" height="1368" alt="image" src="https://github.com/user-attachments/assets/1292dc78-3e06-4dbd-888b-b25f33317c1a" />

<img width="2560" height="1368" alt="image" src="https://github.com/user-attachments/assets/f6a8b650-fa29-45b5-b0b2-ee1ef09634e8" />

最后点 create pull request 

有人审核同意之后才会合并到main分支。注意审核的时候可能会提建议要全部回复完才可以，可能还要solved才可以。


题解要求：

尽量用c++,命名不要用拼音，不要有太多冗余。
