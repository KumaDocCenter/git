# FAQ

### Git 切换分支对工作区的影响

切换分支时，未纳入版本控制的，和忽略的，文件（文件夹）  ，不会清除，会携带至目标分支。



### git从当前分支的某一个commit开始创建新分支

从某一个commit开始创建本地分支

```
// 通过checkout 跟上commitId 即可创建制定commit之前的本地分支
git checkout commitId -b 本地新branchName
```

上传到远程服务器

```
// 依然通过push 跟上你希望的远程新分支名字即可
git push origin HEAD:远程新branchName
```

 

### Github上fork的项目怎么更新同步原项目

 参考阅读： [github上fork了别人的项目后，再同步更新别人的提交](https://blog.csdn.net/qq1332479771/article/details/56087333)

 

### git提示“warning: LF will be replaced by CRLF”

用`core.autocrlf`来打开此项功能，如果是在Windows系统上，把它设置成true，这样当签出代码时，LF会被转换成CRLF：

```
$ git config --global core.autocrlf true
```

Linux或Mac系统使用LF作为行结束符，因此你不想 Git 在签出文件时进行自动的转换；当一个以CRLF为行结束符的文件不小心被引入时你肯定想进行修正，把`core.autocrlf`设置成input来告诉 Git 在提交时把CRLF转换成LF，签出时不转换：

```
$ git config --global core.autocrlf input
```

这样会在Windows系统上的签出文件中保留CRLF，会在Mac和Linux系统上，包括仓库中保留LF。

如果你是Windows程序员，且正在开发仅运行在Windows上的项目，可以设置false取消此功能，把回车符记录在库中：

```
$ git config --global core.autocrlf false
```

**参考阅读** 

 [关于git提示“warning: LF will be replaced by CRLF”终极解答](https://www.jianshu.com/p/450cd21b36a4)

[Git warning: LF will be replaced by CRLF 解决方案](https://blog.csdn.net/wz947324/article/details/81220182)



### Push到Github时出现 POST git-receive-pack (chunked) 的解决办法

使用SourceTree向Github push提交代码时，出现以下提示： 

 `POST git-receive-pack (chunked)  `

在SourceTree>设置>远程仓库>编辑配置文件中，增加相应代码：

```
[http] 
    postBuffer = 524288000
```

重新push，记得耐心等待上传完成，如果时间过长，可以考虑重新上传:) 
原因： 
因为仓库过大导致出现POST git-receive-pack (chunked) ，那么即使重新修改了配置文件，仍然要耐心等待一定时间。 

尤其是国内网络问题，大文件push到github往往需要较长时间。



### 如何把GIT仓库的子目录独立成新仓库

**参考阅读：** 

[如何把GIT仓库的子目录独立成新仓库](https://blog.csdn.net/lujun9972/article/details/46944733)

[把 git 仓库的子目录独立成新仓库](https://segmentfault.com/a/1190000012277504)

[Git 仓库拆拆拆](https://segmentfault.com/a/1190000002548731)



我有一个名为MyLisp的仓库,里面存放的是一些我自己写的elisp脚本,仓库地址是~/MyLisp.

其中我使用elisp模仿rake写了一个新的构建工具名为elake,存放在~/MyLisp/elake目录中. 某一天我想把elake独立出来作为一个仓库来使用,则有两种方法可以实现:



git 1.7.11之后使用 git subtree 指令可以很簡單地把單一資料夾相關的 commit 都抽出來

1. 将MyLisp仓库中关于elake的提交信息抽出为新的branch

   ```
   cd ~/MyLisp
   git subtree split -P elake -b elake
   ```

2. 新建elake仓库,并从MyLisp的elake branch中拉内容 

    ```
      cd ~/
      mkdir elake
      git init
      git pull ~/MyLisp elake
    ```

   

### git subtree 拆库，提交，建立关联

```
git根目录 
    project_a/
    project_b/
    ...

git拆库：
# 拉一个新分支
git checkout -b project_a

# 重构本分支的log，将project_a目录提为根目录并去掉其他文件和log
git filter-branch -f --prune-empty --subdirectory-filter project_a/

# 将新的远端代码库添加到当前工作目录
git remote add project_a git://xxxxxx

# 将新的分支push到新的代码库的master分支
git push -f project_a project_a:master

# 建立关联（本地没有该子库文件夹）：
git subtree add --prefix=project_a project_a master --squash

# 从远程仓库更新子目录
git fetch ai master 
git subtree pull --prefix=project_a project_a master --squash

# 从子目录push到远程仓库（确认你有写权限）
git subtree push --prefix=project_a project_a dev --squash
```

已存在库，建立关联，需要先建立分支，在外面单独提交，再建立subtree add关联：
```
cd git根目录
git subtree split -P project_a -b tempproject_a
# (這會把 project_a 這個資料夾抽出來成為一個叫 tempproject_a的 branch)


cd ..
mkdir project_a
cd project_a
git clone 子仓库（可 git checkout dev分支）

git pull ../git根目录 tempproject_a --allow-unrelated-histories
# (從 git根目录中把 tempproject_a這個 branch 的資料拉回來)


git commit -m ""
# (遇到冲突merge，可以用git mergetool解决冲突) 

git push origin master

最后切回主工程，删除子文件夹 
git commit -m "remove moudle add subtree"
git remote add project_a <git@github.com:my-user/new-repo.git>  master --squash

git subtree pull -P project_a project_a master --squash
```





### Git获取某个commit的特定文件夹或者文件

```
git checkout [commit] <filePath>
```

直接弄出某个commit的特定文件夹。

如果不知道commit是多少，可以git log看看，然后复制前面6个数字，就是这个commit的名称。



### git 从其他分支checkout文件到当前分支

使用场景，把当前分支的某个文件替换为其他分支的文件

执行命令

```git
git checkout <branch name> -- path
```

path 就是你想要替换的目录或文件



### git根据提交新建分支

即，检出为新分支。

如果不知道commit是多少，可以git log看看，然后复制前面6个数字，就是这个commit的名称。

**指定commit ID 检出为新分支**

```
git checkout [commit] -b <branch name>
```

不指定，则检出最近一次的提交。

**示例**

```
git checkout  9fe8e1 -b md
```



### Git提交空目录的可行性方案

[原文](https://blog.csdn.net/szq2k08/article/details/73867394)

git和 svn不同，仅仅跟踪文件的变动，不跟踪目录。所以，一个空目录，如果里面没有文件，即便 `git add`这个目录，另外在别处 `check out` 的时候，是没有这个空目录的。

只跟踪文件变化，不跟踪目录，这么设计是有原因的。但这会带来一些小麻烦。有时候，确实需要在代码仓库中保留某个空目录。比如测试时需要用到的空目录。下面来看看如何解决。



**其实这里有两种情况：**

**一、目录是空的**

这种情况下只需要在目录下创建`.gitkeep`文件，然后在项目的`.gitignore`中设置不忽略`.gitkeep`

**.gitkeep 是一个约定俗成的文件名并不会带有特殊规则**

**二、目录中已经存在文件**

那就需要首先在根目录中设置`!.gitignore`，然后在目标目录也创建一个`.gitignore`文件，并在文件中设置

```.gitignore
*
!.gitignore
```



### Git提交空文件夹的技巧

 [Git提交空文件夹的技巧](https://www.cnblogs.com/EasonJim/p/9152919.html)

==适用于： linux系统下或在git bash环境下执行 。==

**原理**是在每个空文件夹新建一个.gitignore文件，然后提交。 

git 没有跟踪空目录，所以需要跟踪那么就需要添加文件，方法如下:

```bash
find . -type d -empty -exec touch {}/.gitignore \;
```

给所有的子空目录都添加gitignore文件；

此命令翻译：

```
查找 所有文件， 类型为 目录， 结果为空目录的 ， 将执行  touch 命令，输出空内容到 .gitignore 文件
```

**注意：**

`   \;`  斜杠前有空格，分号不能漏



### Git命令行添加整个文件夹及目录

git add 文件夹/            添加整个文件夹及内容

git add *.文件类型       添加目录中所有此文件类型的文件



### Git 以分支的方式同时管理多个项目

参考阅读： [Git 以分支的方式同时管理多个项目](https://www.cnblogs.com/huangtailang/p/4748075.html)



### git 钩子存放机制？

答：

钩子在存在于每一个 Git 仓库，且当我们运行 `git clone `时它们 ==不会== 被复制到新仓库中。

维护一个开发团队的钩子可是有点棘手，**因为 .git/hooks 目录不会随着项目的其他部分复制的，也不会被版本控制**。对上面俩问题的最简单的解决方案就是把钩子存在实际的项目目录中（.git 目录之外），这就可以让我们像任何其他版本控制文件一样来编辑钩子。为了安装钩子，我们可以创造一个链接将其链接到 .git/hooks；或者是当有更新的时候，简单的将其复制粘贴到 .git/hooks 目录中。



### shell脚本获取仓库目录名

**实现原理**

参考阅读： [Shell 脚本获取当前目录 和 获得 文件夹名](https://www.cnblogs.com/anloveslife/p/8619031.html)


```bash
#!/bin/bash

project_path=$(cd `dirname $0`; pwd)
project_name="${project_path##*/}"
echo $project_path
echo $project_name
```

> \`dirname $0`   :  反引号 (按键 ~ )
>
> 在命令行状态下单纯执行 $ cd \`dirname $0` 是毫无意义的。因为他返回当前路径的"."。 
>
> 这个命令写在脚本文件里才有作用，他返回这个脚本文件放置的目录，并可以根据这个目录来定位所要运行程序的相对位置（绝对位置除外）。
>
>  $0：当前Shell程序的文件名 
>
> dirname $0  : 获取当前Shell脚本的路径 
>
> cd \`dirname $0`  : 进入当前Shell脚本的目录 

> pwd  命令用于显示工作目录。  如， `/home/admin/test`

> \#:表示从左开始算起，并且截取第一个匹配的字符
>
> ​    ##:表示从左开始算起，并且截取最后一个匹配的字符
>
> ​      %:表示从右开始算起，并且截取第一个匹配的字符
>
>    %%:表示从右开始算起，并且截取最后一个匹配的字符
>
> 这里的 截取 == 去除。
>
> `project_path##*/` 表示：从左开始算起，到最后一个匹配的`/`之间的字符都去除，剩下的是当前目录名。



**实现代码**

获取代码脚本所在的目录的目录名(仓库目录名)

```bash
#!/bin/bash

project_path=$(cd `dirname $0`; pwd)
project_name="${project_path##*/}"
echo $project_path
echo $project_name
```

`$project_name`  为所需结果

当前代码的脚本文件需要放置在git仓库根目录。



git钩子代码获取仓库目录名

```bash
#!/bin/sh

project_path=$(pwd)
project_name="${project_path##*/}"
echo $project_path
echo $project_name 
```

`$project_name`  为所需结果

因为钩子，git工作目录为仓库根目录，所以不需要 cd





### git clone 指定分支

**因为clone下来的一般为master分支** ，可使用 参数 `--single-branch`   来克隆指定分支。

```
git clone -b dev --single-branch git@github.com:tancolo/MOOC.git 
```

git工程文件很大情况，推荐使用 `git clone -b git_仓库_分支 --single-branch git_仓库_url`。 缺点是看不到其他分支。 

其它参阅： [git clone几种可选参数的使用与区别](https://blog.csdn.net/shrimpcolo/article/details/80164741)  << 已下载为pdf >>



### 第三方工具 SourceTree ，在Windows下突然打不开？

可能的原因：

Git For Windows在环境变量中配置了多个git.exe 路径，SourceTree不知哪个路径才是，就无法调用。

Windows下`path`通常为 `<install path>\cmd` 

