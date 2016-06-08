title: Git Magic

date: 2015-09-13 22:51:55

tags: [Git]

---

这是我最近给公司同事分享的PPT，特记录于此。
其实PPT总体还是过于简洁，Git知识点繁多，但只要理解了思想，多实践是会发现其中的精彩的。

<!--more-->

# Git Magic PPT

> “The stupid content tracker.” 
>  –Linus Torvalds

### 来自各方的赞美

* 版本控制瑞士军刀。 
* 足够先进的技术与魔法无二 
* 可靠、多才多艺、用途多样、异常灵活、 
* 不易掌握、难以精通  

## Why git is magic?

### 身世显赫

* 出自linux之父Linus伟大作品
* Linux 内核代码管理工具:BitKeeper
* 2005年：开始自己开发Git

### Git 诞生大事记

* 2005年4月3号 —> 开始开发
* 2005年4月6日 —> 项目发布
* 2005年4月7日 —> Git作为自身的版本控制工具
* 2005年4月18日—> 多分支合并
* 2005年4月29日—> Git 的性能达到了 Linus 的预期
* 2005年6月16日—> 开始用于维护Linux 源代码

### 开发目标

* 速度 
* 简单的设计
* 对分支强力支持
* 完全分布式
* 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）

## Git思想

### 1、直接记录快照

* 每个Commit是一个快照，而非差异比较
* 变更文件直接copy， 未变更文件指向上一个版本
* 需切换到某个commit时，直接load
* 空间换时间（而非保存差异，做merge）

### 2、几乎所有操作本地执行

分布式
速度之神赐给了 Git 超凡的能量

### 3、保证数据完整性

* 所有数据在存储前都用 SHA-1 散列计算校验和
* 40 个十六进制字符（0-9 和 a-f）组成字符串
* 基于 Git 中文件的内容或目录结构计算出来
* 通常 8 到 10 个字符已经足够避免 SHA-1 的歧义
* Linux 目前有超过 45 万个提交，包含 360 万个对象，也只需要前 11 个字符就能保证唯一性。

### 4、一般直添加数据

* 仓库内容几乎只增加不减少
* 难以丢失数据，大部分操作是可逆的

### 5、三种状态

* Working Directory：工作目录—>已修改（modified）
* Stage（Index）：暂存“目录”，用git add命令添加的文件就到了这里，即将被commit的文件—>已暂存（staged）
* Repository：项目“目录”，用git commit提交的文件就到了这里—>已提交（committed）

### 6、给力的分支

* 大多数版本控制系统创建分支时，将所有的项目文件都复制一遍
* 而Git是如何创建分支的呢？为什么会这么快
* Git 的分支实质上仅是包含所指对象校验和（长度为 40 的 SHA-1 值字符串）的文件，所以它的创建和销毁都异常高效。
* 创建一个新分支就像是往一个文件中写入 41 个字节（40 个字符和 1 个换行符），如此的简单能不快吗？
 
## Git Magic Usage

### 常用命令

* git init、git clone、git status、git add、git commit、
* git push、git pull、git diff、git log、git checkout、
* git reset、git fetch、git rebase、git revert、
* git cherry-pick、git rm、git rerere、git mv、
* git config、git remote、git tag、git show、git grep……

## 从小例子开始简单讲起

### 本地项目初始化

* git init  : 创建一个名为 .git 的文件夹
* git remote add origin xxxxx.git
* vi .gitignore
* git add ./xxxx
* git commit -m “first commit”
* 此时默认仓库名为origin ，默认分支为master
* origin 和master  这两个只是默认的名称，没有什么特殊，可以修改为任意名字

### 从网络上clone项目

* git clone xxx.git TestProject
* git clone -b develop  xxx.git
* git add fileName
* git commit -m “first commit”
* 此时默认仓库名为origin ，默认分支为master

### git clone/remote

* git clone xxx.git TestProject// 自定义文件夹名称
* git clone -b develop  xxx.git // 指定拉取分支
* git remote add <shortname> <url> // 链接仓库

### .gitignore

* 每个项目开始先配置好
* 支持标准的 glob 模式匹配
* 以#开头和空行默认不生效
* ！（不过滤，必须上传）
* https://github.com/github/gitignore 

### git add/commit

* git add  text.txt   反操作：git reset HEAD test.txt
* git add .
* git add --all
* git commit -a -m
* git commit  --amend    // 修改上次提交

### git branch

* Head是一个指针，指向当前所在的本地分支
* 它总是指向该分支上的最后一次提交

### git fetch/pull

* pull = fetch + merge
* 官方建议fetch+merge
* git pull  - - rebase   =  git fetch + git rebase

### merge/rebase

* 作用都是：整合来自不同分支的修改

### merge

* git merge master
* merge=(C3 & C4) & C2进行三方合并，合并的结果是生成一个新的快照（并提交）
* Fast-forward合并，其实可以理解为HEAD指针指向的快速移动。

### rebase

* git rebase master
* 取出master上c2后面的历次commit(提交)
* 将上述提交按照时间对C4（experiment分支）应用

### merge和rebase的两种观点

* 记录实际发生过什么：它是针对历史的文档，本身就有价值，不能乱改。（像历史本身）
* 项目过程中发生的故事：持这一观点的人会使用 rebase 及 filter-branch 等工具来编写故事，怎么方便后来的读者就怎么写。（像正史本身）
* 总的原则：只对尚未推送或分享给别人的本地修改执行rebase操作清理历史，从不对已推送至别处的提交执行rebase操作

### checkout

* 分支相关操作：git checkout 分支名/commit hash切换到相应的分支或commit，加上-b参数则会创建分支并切换过去
* 恢复文件相关操作：git checkout file

### git reset

* git reset --soft HEAD~2
* --soft     移动 HEAD
* --mixed  移动 HEAD 、更新索引  (默认)
* --hard    移动 HEAD 、更新索引、更改目录
* git reset file.txt  —>移动HEAD、更新索引
* 缺点是它会重写历史

### revert

* git revert HEAD
* HEAD继续前进
* 逆向的commit“中和”之前的commit

### checkout vs reset

* git checkout [branch] 与git reset --hard [branch]非常相似
* checkout 对工作目录是安全的、reset 会移动 HEAD 分支的指向
* checkout 只会移动 HEAD 自身来指向另一个分支。
 
### 比较有用的一些命令
 
* git rm —cached //移除remote 仓库不需要的文件
* git mv
* git shortlog --since=18.hours --no-merges > pbcopy // 打印简短的log
* android studio local history
* git stash // 暂时缓存当前的修改
* git stash list
* git stash apply id
* git stash pop
* git stash drop  id
* git cherry-pick <commit id>
* 核武器级选项：filter-branch // 更改所有历史
* git filter-branch --tree-filter 'rm -f passwords.txt' HEAD 

### 推荐资源

* Pro Git book : http://git-scm.com/book/zh/v2
* Learn Git : http://pcottle.github.io/learnGitBranching/
* 图解Git : http://my.oschina.net/xdev/blog/114383
* Git Magic `:http://www-cs-students.stanford.edu/~blynn/gitmagic/intl/zh_cn/book.html

### 形象记忆

* cherry-pick - 妈妈，我也要
* merge - 求合体
* rebase - 我是直男，不喜欢弯的
* commit - 我会好几种姿势呢
* checkout - 上得了厅堂，下得了厨房
* diff - 来，叔叔给你检查身体
* reset - 有了我你随便咋折腾都行
* rebase -变基

### Thanks











