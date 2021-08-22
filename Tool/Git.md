# [Git基础概念以及简单的操作](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-Git-%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F)

> 毋庸置疑的git是一个**版本控制**工具，并不是类似于CVS，Subversion，Perforce等基于差异的实现方式，git是基于快照实现版本控制。
>
> - 近乎所有操作都是在本地进行
> - 完整性
> - Git一般只添加数据
> - 三种状态：**modfied**，**committed**，**staged**
>
> > 三种状态对应的是三个阶段：工作区，暂存区，git目录
> >
> > - 工作区是对项目某个版本提取出来的内容，提供给coder去coding
> > - 暂存区是一个文件，保存下次将要提交的文件列表信息
> > - git仓库目录是保存元数据和对象数据库的地方



## Git Basics

### 初始化

```shell
git init
```

创建一个`.git`目录，包含初始化git仓库的所有必要文件

### 克隆

```shell
git clone urlOfGitResp [userDefineName]
```

这个命令是clone一个已经存在的resp，可以是开源的也可以是你自己创建的。同样会包含一个文件夹`.git`，里面存在的是所有的数据，然后从中读取最新的文件的拷贝

### 查看状态

```shell
git status [-s]
```

查看文件的状态，是否存在新的已修改，或者新增的文件，可选参数`-s`是一个简化输出的命令

### 加入文件

```shell
git add fileName
```

开始跟踪一个文件，或者是暂存已经修改的文件

> ```console
> $ git add CONTRIBUTING.md
> $ git status
> On branch master
> Your branch is up-to-date with 'origin/master'.
> Changes to be committed:
> (use "git reset HEAD <file>..." to unstage)
> 
>  new file:   README
>  modified:   CONTRIBUTING.md
> ```

### 忽略文件

```shell
.gitignore # 描述被忽略的文件时用到的
```

> - 所有空行或者以 `#` 开头的行都会被 Git 忽略。
> - 可以使用标准的 glob 模式匹配，它会递归地应用在整个工作区中。
> - 匹配模式可以以（`/`）开头防止递归。
> - 匹配模式可以以（`/`）结尾指定目录。
> - 要忽略指定模式以外的文件或目录，可以在模式前加上叹号（`!`）取反。
>
> eg: *GitHub 有一个十分详细的针对数十种项目及语言的 `.gitignore` 文件列表， 你可以在[这里](https://github.com/github/gitignore) 找到它。*
>
> ```shell
> # 忽略所有的 .a 文件
> *.a
> # 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
> !lib.a
> # 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
> /TODO
> # 忽略任何目录下名为 build 的文件夹
> build/
> # 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
> doc/*.txt
> # 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
> doc/**/*.pdf
> ```
>

### 查看diff

```shell
git diff [fileName] [--staged]
```

要查看尚未暂存的文件更新了哪些部分，不加参数直接输入 `git diff`  加了参数后此命令比较的是工作目录中当前文件和暂存区域快照之间的差异。 也就是修改之后还没有暂存起来的变化内容。若要查看已暂存的将要添加到下次提交里的内容，可以用 `git diff --staged`*--staged` 和 `--cached*命令。 这条命令将比对已暂存文件与最后一次提交的文件差异。请注意`git diff` 本身只显示尚未暂存的改动，而不是自上次提交以来所做的所有改动。 所以有时候你一下子暂存了所有更新过的文件，运行 `git diff` 后却什么也没有，就是这个原因。

### 提交

```shell
git commit [-m "your update describe"] [-a]
# git commit -a -m 'added new benchmarks'
```

`commit` 是在`git add`所有的你需要提交的修改以后的操作，提交时记录的是放在暂存区域的快照。

- [-m] 增加描述性的信息
- [-a] 可以直接当作`git add . + git commit -m ""`但是并不会对新创建的文件进行操作

### 移除文件

```shell
rm PROJECTS.md
git rm PROJECTS.md
```

要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。 可以用 `git rm` 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。如果只是简单地从工作目录中手工删除文件，运行 `git status` 时就会在 “Changes not staged for commit” 部分。

### 移动文件

````shell
git mv file_from file_to
# mv README.md README
# git rm README.md
# git add README
````

不像其它的 VCS 系统，Git 并不显式跟踪文件移动操作。 如果在 Git 中重命名了某个文件，仓库中存储的元数据并不会体现出这是一次改名操作。 不过 Git 非常聪明，它会推断出究竟发生了什么。

### 查看日志

```shell
git log	[--stat] [-p]/[--patch] [-n]  [--since]/[--until] [--author]
# git log --since=2.weeks
# 
```

不传入任何参数的默认情况下，`git log` 会按时间先后顺序列出所有的提交，最近的更新排在最上面。 正如你所看到的，这个命令会列出每个提交的 **SHA-1 校验和**、**作者的名字**和**电子邮件地址**、**提交时间**以及**提交说明**。

`git log` 有许多选项可以帮助你搜寻你所要找的提交， 下面我们会介绍几个最常用的选项。

其中一个比较有用的选项是 `-p` 或 `--patch` ，它会显示每次提交所引入的差异（按 **补丁** 的格式输出）。`--stat` 输出简略信息。你也可以限制显示的日志条目数量，例如使用 `-2` 选项来只显示最近的两次提交：

> ```shell
> $ git log -p -1
> commit ca82a6dff817ec66f44342007202690a93763949
> Author: Scott Chacon <schacon@gee-mail.com>
> Date:   Mon Mar 17 21:52:11 2008 -0700
> 
>     changed the version number
> 
> diff --git a/Rakefile b/Rakefile # 这里是差异所在
> index a874b73..8f94139 100644
> --- a/Rakefile
> +++ b/Rakefile
> @@ -5,7 +5,7 @@ require 'rake/gempackagetask'
>  spec = Gem::Specification.new do |s|
>      s.platform  =   Gem::Platform::RUBY
>      s.name      =   "simplegit"
> -    s.version   =   "0.1.0"
> +    s.version   =   "0.1.1"    # 差异以及上下文
>      s.author    =   "Scott Chacon"
>      s.email     =   "schacon@gee-mail.com"
>      s.summary   =   "A simple gem for using Git in Ruby code."
> ```

另一个非常有用的选项是 `--pretty`。 这个选项可以使用不同于默认格式的方式展示提交历史。 这个选项有一些内建的子选项供你使用。 比如 `oneline` 会将每个提交放在一行显示，在浏览大量的提交时非常有用。 另外还有 `short`，`full` 和 `fuller` 选项，它们展示信息的格式基本一致，但是详尽程度不一：

> ```console
> $ git log --pretty=oneline
> ca82a6dff817ec66f44342007202690a93763949 changed the version number
> 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test
> a11bef06a3f659402fe7563abf99ad00de2209e6 first commit
> ```

最有意思的是 `format` ，可以定制记录的[显示格式](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2)。 这样的输出对后期提取分析格外有用——因为你知道输出的格式不会随着 Git 的更新而发生改变：

> ```console
> $ git log --pretty=format:"%h - %an, %ar : %s"
> ca82a6d - Scott Chacon, 6 years ago : changed the version number
> 085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
> a11bef0 - Scott Chacon, 6 years ago : first commit
> ```

*作者与提交者的区别在于行为差异，修改文件的是作者，最后做出合并的是提交者*

###  撤销操作

```shell
git commit --amend	
# git commit -m 'initial commit'
# git add forgotten_file
# git commit --amend
```

有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 `--amend` 选项的提交命令来重新提交。这个命令会将暂存区中的文件提交。 如果自上次提交以来你还未做任何修改（例如，在上次提交后马上执行了此命令）， 那么快照会保持不变，而你所修改的只是提交信息。

### 取消暂存的文件

接下来的两个小节演示如何操作暂存区和工作目录中已修改的文件。 这些命令在修改文件状态的同时，也会提示如何撤消操作。 例如，你已经修改了两个文件并且想要将它们作为两次独立的修改提交， 但是却意外地输入 `git add *` 暂存了它们两个。如何只取消暂存两个中的一个呢？ `git status` 命令提示了你：

```console
$ git add *
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
    modified:   CONTRIBUTING.md
```

在 “Changes to be committed” 文字正下方，提示使用 `git reset HEAD <file>...` 来取消暂存。 所以，我们可以这样来取消暂存 `CONTRIBUTING.md` 文件：

```console
$ git reset HEAD CONTRIBUTING.md
Unstaged changes after reset:
M	CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

### 撤消对文件的修改

如果你并不想保留对 `CONTRIBUTING.md` 文件的修改怎么办？ 你该如何方便地撤消修改——将它还原成上次提交时的样子（或者刚克隆完的样子，或者刚把它放入工作目录时的样子）？ 幸运的是，`git status` 也告诉了你应该如何做。 在最后一个例子中，未暂存区域是这样：

```console
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   CONTRIBUTING.md
```

它非常清楚地告诉了你如何撤消之前所做的修改。 让我们来按照提示执行：

```console
$ git checkout -- CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

    renamed:    README.md -> README
```

可以看到那些修改已经被撤消了。git中有[数据恢复](https://git-scm.com/book/zh/v2/ch00/_data_recovery)相关的部分。

### 查看远程仓库

```shell
git remote [-v]
# git remote -v 简写与URL
```

如果想查看你已经配置的远程仓库服务器，可以运行 `git remote` 命令。 它会列出你指定的每一个远程服务器的简写。 如果你已经克隆了自己的仓库，那么至少应该能看到 origin ——这是 Git 给你克隆的仓库服务器的默认名字。

### 添加远程仓库

我们在之前的章节中已经提到并展示了 `git clone` 命令是如何自行添加远程仓库的， 不过这里将告诉你如何自己来添加它。 运行 `git remote add <shortname> <url>` 添加一个新的远程 Git 仓库，同时指定一个方便使用的简写：

```console
$ git remote
origin
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin	https://github.com/schacon/ticgit (fetch)
origin	https://github.com/schacon/ticgit (push)
pb	https://github.com/paulboone/ticgit (fetch)
pb	https://github.com/paulboone/ticgit (push)
```

### 从远程仓库中抓取与拉取

就如刚才所见，从远程仓库中获得数据，可以执行：

```shell
git fetch <remote>
git fetch pb # 拉去远程中有，但是我本地并没有的
```

如果你使用 `clone` 命令克隆了一个仓库，命令会自动将其添加为远程仓库并默认以 “origin” 为简写。 所以，`git fetch origin` 会抓取克隆（或上一次抓取）后新推送的所有工作。 必须注意 `git fetch` 命令只会将数据下载到你的本地仓库——它并不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作。

### 推送到远程仓库

当你想分享你的项目时，必须将其推送到上游。 这个命令很简单：`git push <remote> <branch>`。 当你想要将 `master` 分支推送到 `origin` 服务器时（再次说明，克隆时通常会自动帮你设置好那两个名字）， 那么运行这个命令就可以将你所做的备份到服务器：

```shell
git push origin master
```

### 远程仓库的重命名与移除

你可以运行 `git remote rename` 来修改一个远程仓库的简写名。 例如，想要将 `pb` 重命名为 `paul`，可以用 `git remote rename` 这样做：

```console
$ git remote rename pb paul
$ git remote
origin
paul
```

### 标签

在 Git 中列出已有的标签非常简单，只需要输入 `git tag` （可带上可选的 `-l` 选项 `--list`）：

```shell
git tag [ -l "v1.8.5*"]
v1.8.5
v1.8.5-rc0
v1.8.5-rc1
v1.8.5-rc2
v1.8.5-rc3
v1.8.5.1
v1.8.5.2
v1.8.5.3
v1.8.5.4
v1.8.5.5
git show v1.8.5 # 查看对应的提交信息
```

#### 创建标签

标签分为两种：轻量级标签以及附注标签

```shell
git tag v1.4-lw # 不需要提供额外的信息，本质是一个而将提交和校验存储到一个文件
```

```shell
git tag -a v1.4 -m "my version 1.4" # 带有描述性的信息
```

#### 后期打标签

你也可以对过去的提交打标签。 假设提交历史是这样的：

```shell
git log --pretty=oneline # 显示为一行
15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
a6b4c97498bd301d84096da251c98a07c7723e65 beginning write support
9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile
```

现在，假设在 v1.2 时你忘记给项目打标签，也就是在 “updated rakefile” 提交。 你可以在之后补上标签。 要在那个提交上打标签，你需要在命令的末尾指定提交的校验和（或部分校验和）：

```shell
git tag -a v1.2 9fceb02
```

#### 共享标签

默认情况下，`git push` 命令并不会传送标签到远程仓库服务器上。 在创建完标签后你必须显式地推送标签到共享服务器上。 这个过程就像共享远程分支一样——你可以运行 `git push origin <tagname>`。

```shell
git push origin tagName
```

#### 删除标签

```shell
删除标签
要删除掉你本地仓库上的标签，可以使用命令 git tag -d <tagname>。 例如，可以使用以下命令删除一个轻量标签：

git tag -d v1.4-lw
# Deleted tag 'v1.4-lw' (was e7d5add)
```

#### 检出标签

```shell
git checkout -b version2 v2.0.0
```

*没懂*

### 分支创建

Git 是怎么创建新分支的呢？ 很简单，它只是为你创建了一个可以移动的新的指针。 比如，创建一个 testing 分支， 你需要使用 `git branch` 命令（并不会自动切换），创建好的分支的实质是一个指针。**HEAD 指向当前所在的分支**。

[这里有一个工作流程的实例](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6)

