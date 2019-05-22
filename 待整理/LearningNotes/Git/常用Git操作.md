# Git 操作

### 1. Git初始化操作

#### 1.1 Git初始操作

```java
git config --global user.name [用户名]
git config --global user.email [邮箱名]
```

上面两行命令是设置用户名和邮箱，因为我们使用Git一般都是多人协作，必须有标识来表示哪些提交修改是我们做的。`--global`命令是针对所有的仓库的，换句话说就是所有的仓库都会使用我们上面设置的用户名和邮箱。

一些公司的项目是存放在私有的Gitlab上的，这个时候，我们使用全局配置的用户名和邮箱就不对了(因为每个公司可能都有自己的规范)，因此，我们就需要在单独为公司的项目仓库设置用户名和邮箱了，可以按下面这样做：

```
//进入项目的根目录(含有.git文件夹的目录)
git config user.name [用户名]
git config user.email [邮箱名]
```

查看配置：

```
git config --list
```

![git_config_list.png](https://upload-images.jianshu.io/upload_images/5231076-6858ee6352d67e62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 1.2 Git仓库初始化

```
初始化空仓库
git init
```

![git_init.png](https://upload-images.jianshu.io/upload_images/5231076-a803837bbedc7787.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

git仓库在初始化完之后会在文件夹下自动生成一个.git的文件夹，这个文件夹就是用来跟踪管理版本库的，不要随便修改，不然会造成版本库损坏，通常它都是隐藏文件。

#### 1.3 Git本地仓库推送到远程

Step1: 先在远程服务器(比如Github)新建一个空仓库，比如：

![空仓库.png](https://upload-images.jianshu.io/upload_images/5231076-f968f6a4ba1c6a7c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Step2: 在本地仓库中新建一个README.md文件并推送到远程仓库

在本地空仓库中新建一个README.md文件后，该文件默认属于master分支，这样我们也应该把他推送到远程的master分支。

新建README.md文件：

```
echo "# GitTest" >> README.md
说明：
自动新建README.md文件并把文本"# GitTest"写入到文件中
```

![echo.png](https://upload-images.jianshu.io/upload_images/5231076-cdc810396d6d55c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

推送到远程空仓库：

```
git push -u origin master
说明：
origin是远程仓库地址的别名，我们必须要通过下面这条命令来设置，不然的话我们只能使用远程仓库地址的全称:
git remote add origin https://github.com/Sunny8519/GitTest.git

master就是远程仓库的分支名，这样也会在远程仓库新建一个叫master的分支。
```

![推送.png](https://upload-images.jianshu.io/upload_images/5231076-722e36e5a56bb693.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

期间可能需要我们输入Github(Gitlab)的账号和密码，输入进去就行了。

这样我们再次刷新Github或者Gitlab的仓库，本地的文件就在远程仓库中了。其实仓库从本地到远程一共就两步：

```
git remote add origin https://github.com/Sunny8519/GitTest.git
git push -u origin master
```





Cherry-pick功能

#### 1. 常用git 命令

- 本地分支关联远程分支

```
git branch --set-upstream-to [local-branch] origin/[remote-branch]
```

- 查看所有本地分支与远程分支的对应关系

```java
git branch -vv
```

* 创建本地仓库

```
git init
```

* 获取远程仓库

```
git clone [url]
例：git clone https://github.com/you/yourpro.git
```

* 创建远程仓库

```
// 添加一个新的 remote 远程仓库，可用于修改远程仓库的地址(比如远程仓库的地址变更后)
git remote add [remote-name] [url]
例：git remote add origin https://github.com/you/yourpro.git
origin：相当于该远程仓库的别名

// 列出所有 remote 的别名
git remote

// 列出所有 remote 的 url(本地仓库跟踪的远程仓库)
git remote -v

// 删除一个 remote
git remote rm [name]

// 重命名 remote
git remote rename [old-name] [new-name]
```

* 从本地仓库中删除

```
git rm file.txt         // 从版本库中移除，删除文件
git rm file.txt -cached // 从版本库中移除，不删除原始文件
git rm -r xxx           // 从版本库中删除指定文件夹
```

- 把本地仓库上传到Github

```
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/Sunny8519/LearningViews.git
git push -u origin master
```

* 从本地仓库中添加新的文件

```
git add .               // 添加所有文件
git add file.txt        // 添加指定文件
```

* 提交，把缓存内容提交到 HEAD 里

```
git commit -m "注释"
```

* 撤销

```
// 撤销最近的一个提交.
git revert HEAD

// 取消 commit + add
git reset --mixed

// 取消 commit
git reset --soft

// 取消 commit + add + local working
git reset --hard
```

* 把本地提交 push 到远程服务器

```
git push [remote-name] [loca-branch]:[remote-branch]
例：git push origin master:master
```

* 查看状态

```
git status
```

* 从远程库中下载新的改动

```
git fetch [remote-name]/[branch]
```

* 合并下载的改动到分支

```
git merge [remote-name]/[branch]
```

* 从远程库中下载新的改动

```
pull = fetch + merge

git pull [remote-name] [branch]
例：git pull origin master
```

* 分支

```
// 列出分支
git branch

// 创建一个新的分支
git branch (branch-name)

// 删除一个分支
git branch -d <本地分支名>
// 强制删除本地分支
git branch -D <本地分支名>


// 删除 remote 的分支
git push <远程仓库名> :<远程分支名>
or
git push <远程仓库名> --delete <远程分支名>
```

* 切换分支

```
// 切换到一个分支
git checkout [branch-name]

// 创建并切换到该分支
git checkout -b [branch-name]
```

- 同步远程仓库到本地

```
git fetch --all
```



####2. 与github建立ssh通信，让Git操作免去输入密码的繁琐

*   首先呢，我们先建立ssh密匙。
> ssh key must begin with 'ssh-ed25519', 'ssh-rsa', 'ssh-dss', 'ecdsa-sha2-nistp256', 'ecdsa-sha2-nistp384', or 'ecdsa-sha2-nistp521'.  -- from github

根据以上文段我们可以知道github所支持的ssh密匙类型，这里我们创建ssh-rsa密匙。
在command line 中输入以下指令:``ssh-keygen -t rsa``去创建一个ssh-rsa密匙。如果你并不需要为你的密匙创建密码和修改名字，那么就一路回车就OK，如果你需要，请您自行Google翻译，因为只是英文问题。

```
$ ssh-keygen -t rsa
Generating public/private rsa key pair.
//您可以根据括号中的路径来判断你的.ssh文件放在了什么地方
Enter file in which to save the key (/c/Users/Liang Guan Quan/.ssh/id_rsa):
```

* 到 https://github.com/settings/keys 这个地址中去添加一个新的SSH key，然后把你的xx.pub文件下的内容文本都复制到Key文本域中，然后就可以提交了。
* #### 添加完成之后 我们用``ssh git@github.com`` 命令来连通一下github，如果你在response里面看到了你github账号名，那么就说明配置成功了。


#### 3. gitignore
在本地仓库根目录创建 .gitignore 文件。Win7 下不能直接创建，可以创建 ".gitignore." 文件，后面的标点自动被忽略；

```
/.idea          // 过滤指定文件夹
/fd/*           // 忽略根目录下的 /fd/ 目录的全部内容；
*.iml           // 过滤指定的所有文件
!.gitignore     // 不忽略该文件
```

#### 4. Github上fork别人的代码并同步更新被人的提交

解决方案是为本地仓库添加两个远程仓库，一个远程仓库是别人的代码仓库，一个是自己fork后的仓库，这样当别人在他的仓库中提交代码后，可以在本地更新他的仓库中的代码然后提交了自己fork的仓库中。

![添加远程仓库.png](https://upload-images.jianshu.io/upload_images/5231076-3325d344273aea6a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![拉取远程分支代码.png](https://upload-images.jianshu.io/upload_images/5231076-c6dfc1a47f33585f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![push到自己fork的仓库中.png](https://upload-images.jianshu.io/upload_images/5231076-89bfdbb8c54fbe62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


