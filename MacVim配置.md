## MacVim配置

### 安装MacVim

1.下载MacVim dmg安装包并安装：https://github.com/macvim-dev/macvim/releases

2.安装完之后配置环境变量以便在Terminal中通过命令行直接启动MacVim：

a.在控制台中进入`~`目录下
```
$ cd ~
```

b.通过mac自带的vim打开`.bash_profile`文件
```
$ vim .bash_profile
```

c.添加`export PATH=${PATH}:/Applications/MacVim.app/Contents/bin`配置

这样在Terminal控制台中我们就可以通过`mvim`命令直接打开MacVim了。

### 配置MacVim

#### 创建`~/.bashrc`文件

**目的：通过命令行`vim`也能打开MacVim。**

1.在控制台中进入`~`目录下(该目录是当前用户目录)
```
$ cd ~
```

2.创建.bashrc文件
```
$ touch .bashrc
```
该文件是隐藏文件，通过`ls`命令是看不到的，我们可以通过`ls -a`命令查看

3.通过`mvim`命令打开.bashrc文件
```
$ mvim .bashrc
```

这样我们就进入到了.bashrc文件里面了，添加如下配置：
```
alias vim=mvim
```

保存文件并退出`:wq`。

4.使.bashrc文件立即生效
```
$ source .bashrc
```

5.测试配置是否成功
```
$ vim .bashrc
```

跳转到MacVim页面并打开.bashrc文件就代表着配置成功，以后我们通过命令行`vim`也能打开MacVim了。

#### 创建`~/.vimrc`和`~/.gvimrc`并配置

1.打开MacVim，输入`:versioin`，会出现以下信息：
```
...
   system vimrc file: "$VIM/vimrc"
     user vimrc file: "$HOME/.vimrc"
 2nd user vimrc file: "~/.vim/vimrc"
      user exrc file: "$HOME/.exrc"
  system gvimrc file: "$VIM/gvimrc"
    user gvimrc file: "$HOME/.gvimrc"
2nd user gvimrc file: "~/.vim/gvimrc"
       defaults file: "$VIMRUNTIME/defaults.vim"
    system menu file: "$VIMRUNTIME/menu.vim"
  fall-back for $VIM: "/Applications/MacVim.app/Contents/Resources/vim"   
...
```
`$VIM`/`$HOME`/`$VIMRUNTIME`表示的是系统变量，当我们不知道他们具体表示什么路径时，可以通过这个命令行查看`:echo $VIMRUNTIME`，比如`$VIMRUNTIME`就表示`/Applications/MacVim.app/Contents/Resources/vim/runtime`。

2.从系统文件`vimrc_example.vim`和`gvimrc_example.vim`中copy标准的内容，保存到用户的配置文件`~/.vimrc`和`~/.gvimrc`中。
```
:e $VIMRUNTIME/vimrc_example.vim  
:saveas ~/.vimrc  
:e $VIMRUNTIME/gvimrc_example.vim  
:saveas ~/.gvimrc
```

这样`~/.vimrc`和`~/.gvimrc`文件就创建完毕了。

3.或者可以不用步骤2中的方法创建`~/.vimrc`和`~/.gvimrc`文件，直接进入到用户目录下`~`，使用命令行`touch .vimrc`和`touch .gvimrc`手动创建，只不过创建出来的文件没有内容。

### 添加插件

我们使用`vundle`来管理vim插件

1.从git上下载`vundle`到`~/.vim/bundle/Vundle.vim`
```
$ git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

根据github上说明把配置加进`.vimrc`文件中，详细步骤见github。

至此我们就可以使用vundle来管理vim插件了。

### 参考

[macOS 下 MacVim 的安装与配置 以及插件安装](https://www.jianshu.com/p/2465e07dd59a)

[Mac开发利器之程序员编辑器MacVim学习总结(转）](https://www.cnblogs.com/vijozsoft/p/5608108.html)