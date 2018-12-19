# FAQ

> git subtree与git submodule 的相关问题汇总





### 子模块也支持稀疏检出

子模块也支持稀疏检出，切换到子模块仓库，然后根据 **稀疏检出** 方法 进行配置即可。

**注意：** 子模块的 `.git`  文件夹在父仓库中 的  `/.git/modules/<subModules>`

**注意：**  子模块的稀疏检出 ，是检出到本地的的内容是部分的，在线上仓库是指向子模块仓库的某次提交，是直接跳转到子模块仓库的，是整个仓库完整的内容。



### 为什么使用git subtree[^1]

使用git subtree 有以下几个原因：

* 旧版本的git也支持(最老版本可以到 v1.5.2).
* git subtree与git submodule不同，它不增加任何像`.gitmodule`这样的新的元数据文件.
* git subtree对于项目中的其他成员透明，意味着可以不知道git subtree的存在.

当然，git subtree也有它的缺点，但是这些缺点还在可以接受的范围内：

* 必须学习新的指令(如：git subtree).
* 子仓库的更新与推送指令相对复杂。



### 更新后每个子模块并非在指定分支上，而是关联最近一次commitID  [^2] 





### SubModule与SubTree的差异[^2]

1. 核心区别 

   git submodule类似于引用，而git subtree类似于拷贝，比如你在一篇博客中想用到你另一篇博客的内容，git submodule是使用那篇博客的链接，而git subtree则是将内容完全copy过来。

2. 优劣

| /                | submodule                                                    | subtree                          | 结果                               |
| ---------------- | ------------------------------------------------------------ | -------------------------------- | ---------------------------------- |
| 远程仓库空间占用 | submodule只是引用，基本不占用额外空间                        | 子模块copy，会占用较大的额外空间 | submodule占用空间较小，略优        |
| 本地空间占用     | 可根据需要下载                                               | 会下载整个项目                   | 所有模块基本都要下载，二者差异不大 |
| 仓库克隆         | 克降后所有子模块为空，需要注册及更新，同时更新后还需切换分支 | 克隆之后即可使用                 | submodule步骤略多，subtree占优     |
| 更新本地仓库     | 更新后所有子模块后指向最后一次提交，更新后需要重新切回分支，所有子模块只需一条更新语句即可 | 所有子模块需要单独更新           | 各有优劣，相对subtree更好用一些    |
| 提交本地修改     | 只需关心子模块即可，子模块的所有操作与普通git项目相同        | 提交执行命令相对复杂一些         | submodule操作更简单，submodule占优 |



### git强制更新所有submodule

```git
git submodule foreach 'git checkout -f'
```



### 忽略submodule中的修改或新增文件[^4]

我们引用第三方的project，大多数情况都是想以“只读”的方式引用，不关心第三方project抓取下来之后是不是被修改，或者是在其目录中添加了untracked的file， 因为我们只是拉取第三方的project，而不会(往往时不能或不允许)对第三方project进行提交。 

我们要做的就是在`.gitmodules`的`[submodule “ProjectName”]`中添加一个`ignore`子项，这个ignore子项可以有3个可选的值，`untracked`, `dirty`和`all`, 它们的意思分别是：

* **untracked** ：忽略 在子模块 新添加的，未受版本控制内容
* **dirty** ： 忽略对\<ProjectName>目录下受版本控制的内容进行了修改
* **all** ： 同时忽略untracked和dirty





### 子模块和子树，在其它位置检出时，能否识别？

子模块可以识别，子树不能识别。



### git 子模块实操记录

> ```
> git clone <repository> --recursive 递归的方式克隆整个项目
> git submodule add <repository> <path> 添加子模块
> git submodule init 初始化子模块
> git submodule update 更新子模块
> git submodule foreach git pull 拉取所有子模块
> ```



> **添加子模块**
>
> > **git submodule add**
>
> ```
> git submodule add -b master https://github.com/KumaDocCenter/Ajax.git  <SubModule>
> git submodule add  https://github.com/KumaDocCenter/Ajax.git  <SubModule>
> ```
>
> `git submodule add` 命令不管有没有`-b master` 指定分支，都会克隆仓库下来。默认`master`分支是在服务器端指定的。
>
> 添加子模块后，会在如下产生记录
>
> ```
> .git\config
> .git\modules\<SubModule>
> .gitmodules
> <SubModule>
> ```
>
> `.git\config`  ： 子模块配置项
>
> `.gitmodules` ： 子模块配置项 
>
> `.git\modules\<SubModule>` ： 子模块版本数据库
>
> * `\<SubModule>\.git` 文件指向此位置相应的数据库 
> * 此数据库通常情况是不会被删除，在重新添加同名子模块时会造成一定的麻烦，所以需要自己编写脚本删除目录，或使用`-f` 强制添加。
>
> `<SubModule>`  ： 子模块。在`git submodule add` 后面指定的相对路径，作为子模块名称。
>
> 
>
> **示例**
>
> ```
> git submodule add -b master -f https://github.com/KumaDocCenter/Ajax.git  public3
> ```
>
> `-b`  ： 指定分支
>
> `-f` ： 强制



> **初始化子模块**
>
> > **git submodule init**
> >
> > 初始化在.git/config中记录的子模块索引
>
> 
>
> 初始化所有子模块
>
> ```
> git submodule init
> ```
>
> 初始化单个子模块
>
> ```
> git submodule init <SubModule>
> ```
>
> `<SubModule>`  ： 子模块。在`git submodule add` 后面指定的相对路径，作为子模块名称。



> **更新子模块**
>
> > **git submodule update**
> >
> > 克隆缺失的子模块并更新子模块的工作树(检出默认分支master) 
>
> 
>
> 更新所有子模块
>
> ```
> git submodule update
> ```
>
> 更新单个子模块
>
> ```
> git submodule update  <SubModule>
> ```
>
> `<SubModule>`  ： 子模块。在`git submodule add` 后面指定的相对路径，作为子模块名称。



> **删除子模块**
>
> > **git  submodule deinit**
> >
> > 取消注册给定的子模块，子模块记录从.git/config中删除
> >
> > -f 指定，则子模块的工作树将被删除
>
> ```
> git  submodule deinit -f  <SubModule>
> ```
>
> `<SubModule>`  ： 子模块。在`git submodule add` 后面指定的相对路径，作为子模块名称。
>
> `git submodule deinit`  只清除.git/config中的子模块记录和删除`<SubModule>/*` ，但不删除 `<SubModule>`  本身。
>
> 
>
> **git rm**
>
> > 从工作树和索引中删除文件
>
> ```
> git rm -rf <SubModule>
> ```
>
> `git rm -rf ` : 删除子模块`<SubModule>`，并清除`.gitmodules`中子模块的记录。
>
> 
>
> **两者结合使用**
>
> ```
> git  submodule deinit -f  <SubModule>
> git rm -rf <SubModule>
> ```
>
> 
>
> 若要删除`.git\modules\<SubModule>` ，需要额外操作
>
> Windows
>
> ```
> rd /s/q  .git\modules\<SubModule>
> ```
>
> Linux
>
> ```
> rm -rf  .git/modules/<SubModule>
> ```
>
> 
>
> **从索引中移除**
>
> > 和 `git rm` 类似效果，但不能同时使用。
> >
> > 删除子模块时，一般不推荐使用此选项
>
> 如果直接add子模块，会提示`'sub_folder already exists in the index'`，所以需要 将子模块信息从索引中移除
>
> ```
> git rm --cached  <SubModule>
> ```
>
> `--cached` : 只从索引中移除。
>
> 确认 
>
> ```
> git ls-files --stage  <SubModule>
> ```
>
> 如果没有 `<SubModule>` 字样，则成功。
>
> 





## 参考阅读

[^1]: [git subtree教程](https://segmentfault.com/a/1190000012002151)
[^2]: [git子模块使用之git submodule与 git subtree比较](https://blog.csdn.net/liusf1993/article/details/72765131)
[^3]: [git submodule 完整用法整理](https://blog.csdn.net/wkyseo/article/details/81589477)
[^4]: [git submodule使用以及注意事项](https://blog.csdn.net/xuanwolanxue/article/details/80609986)