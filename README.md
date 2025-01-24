## 使用 Git 上传本地项目到 Github （个人记录版）

参考文档：[手把手教你用git上传项目到GitHub（图文并茂，这一篇就够了），相信你一定能成功！！ - 知乎](https://zhuanlan.zhihu.com/p/193140870?utm_campaign=shareopn&utm_medium=social&utm_psn=1866167660087214081&utm_source=wechat_session)

- 主要解决了一些主观性的问题，如果恰好能帮到大家是最好了
- 细节如有不完善的地方可以参考对应列出的文档
- 如果前期配置完好，可以直接跳转最后[省流版](#final)



### 一、下载安装 Git

> [!NOTE]
>
> [Git下载及安装保姆级教程-CSDN博客](https://blog.csdn.net/weixin_41293671/article/details/144255269)

> [!tip]
>
> - 下载路径确保没有中文
> - 编辑器可以选择为vscode或者notepad++
>
> [配置git默认编辑器为vscode或者notepad++  - CSDN博客](https://blog.csdn.net/ShuSheng0007/article/details/115449596)



### 二、Git 初始配置

> [!note]
>
> ==**Add new SSH Key 生成的密钥是私钥还是公钥？每次创建仓库都是相同的公钥吗？**==
>
> 在 GitHub 或其他版本控制平台上添加 SSH 密钥时，是在 **添加公钥**，而不是私钥。
>
> 当你通过 `ssh-keygen` 创建 SSH 密钥对时，它会生成两个文件：( 默认路径 `C:\Users\[用户名]\.ssh` )
>
> 1. **私钥**（通常是 `id_rsa`）：这个密钥应该保密，仅存储在你的本地电脑上，不应该与任何人共享。
> 2. **公钥**（通常是 `id_rsa.pub`）：这个密钥是可以共享的，通常用来将其添加到你希望连接的远程服务器或服务中（如 GitHub、GitLab、Bitbucket 等），**Add new SSH Key 正是通过将 id_rsa.pub 中的公钥信息复制到 Github 上来添加公钥**
>
> <img src="./img/屏幕截图 2025-01-25 004752.png" alt="屏幕截图 2025-01-25 004752" style="zoom:45%;" />
>
> **关于每次创建仓库的问题：**
>
> - 不需要为每个仓库都创建新的 SSH 密钥。你可以使用一个密钥对多个仓库或多个服务（如 GitHub）进行身份验证。
> - 当然，可以在个人主页 - settings - SSH and GPG keys - Add new SSH Key 里面创建多个SSH公钥，创建结果如下所示。

<img src="./img/屏幕截图 2025-01-25 002900.png" alt="屏幕截图 2025-01-25 002900" style="zoom: 50%;" />

> [!tip]
>
> 这是我的配置文件，文件位置在 `C:\Users\[用户名]\.gitconfig`，可供参考
>
> （ 我选用了 VS code 作为编辑器，设置了默认本地分支为 main ）
>
> ```ini
> [user]
> 	name = DING4526
> 	email = Ding13522634526@163.com
> [core]
> 	autocrlf = true
> 	editor = code --wait --new-window 
> 	ignorecase = true
> [init]
> 	defaultBranch = main
> [color]
> 	ui = auto
> 
> [diff]
>  	tool = vscode-diff
> [difftool]
>  	prompt = false
> [difftool "vscode-diff"]
>  	cmd = code --wait --diff $LOCAL $REMOTE
> [merge]
> 	tool = vscode-merge
> [mergetool "vscode-merge"]
>  	cmd = code --wait $MERGED
> ```



### 三、配置本地项目到 Github 仓库

#### 1. 创建一个新的 repository

- 自定义仓库命名、仓库描述

- 可以选择添加README（后续默认选择了这一项）

#### 2. Git 索引到本地项目文件夹

- 方法一（推荐）：从文件资源管理器打开项目文件夹，右键 –> Open Git Bash here

- 方法二：打开 Git Bash 之后，cd 到目标文件夹：

	```bash
	cd d:
	# 先索引到相应盘符
	```

	```bash
	cd 03_Courses/2_1_02_Python\(H\)/Experiment_9   
	# 换成自己的目标文件夹路径，“/”连接文件夹，“\”表示对特殊字符的转义
	```



#### 3. 创建本地git仓库，与github仓库链接

###### 1）初始化本地仓库

 ```bsah
git init
# 实际上是创建了一个隐藏文件夹.git
 ```
<img src="./img/屏幕截图 2025-01-24 163839-1737712772983-2.png" alt="屏幕截图 2025-01-24 163839" />

###### 2）将本地仓库链接到github远程仓库

 ```bash
git remote add origin https://github.com/DING4526/python.H_exp_09.git
# https://github.com/DING4526/python.H_exp_09.git 为对应仓库的https索引网址
 ```
<img src="./img/屏幕截图 2025-01-24 163847.png" alt="屏幕截图 2025-01-24 163847" />

###### 3）将初始化的远程仓库同步到本地仓库

==实际上由于初始化的时候选择了自动添加README，本地仓库是落后于远程仓库的，如果直接add,commit,push，会出现冲突，因此**先做一次拉取非常重要**==

 ```bash
git pull origin main
 ```
<img src="./img/屏幕截图 2025-01-24 163854.png" alt="屏幕截图 2025-01-24 163854" />


> [!caution]
>
> - git 默认分支是 master ，但是目前 github 的默认分支是 main，如果想要直接推送到远程仓库的 main 分支上，需要事先更改创建本地 .git 仓库的默认分支为 main
>
>
> ```bash
> git config --global init.defaultBranch main
> ```
> - 通过如下语句确认是否配置成功
>
>
> ```bash
> git config --global --get init.defaultBranch
> # 或者也可以直接看路径最后的括号，括号里面就是当前分支
> ```
>
> - 实际上，可以通过如下语句查看所有的全局配置
>
>
> ```bash
> git config --list --show  # 简洁版
> git config --list --show-origin 
> ```

至此，前期准备工作全部完成，可以进行 add commit push 的经典流程了



#### 4. 推送本地项目到远程仓库

###### 1） 使用 add 语句将项目中所有的修改添加到暂存区

```bash
git add .
```
<img src="./img/屏幕截图 2025-01-24 163904.png" alt="屏幕截图 2025-01-24 163904" />

> [!NOTE]
>
> 如果你只想添加特定的文件而不是所有修改，可以指定文件名来添加某些文件：
>
> ```bash
> git add file1.txt file2.py
> ```
>
> 你还可以使用 **路径** 来选择性地添加文件：
>
> ```bash
> git add src/  # 只添加 src 文件夹下的更改
> git add *.py  # 只添加所有 Python 文件
> ```
>
> 如果你删除了文件并希望将这个删除操作也提交，你也需要将删除操作添加到暂存区：
>
> ```bash
> git add -u
> # -u 参数会自动包括已删除的文件。
> ```
>
> 在使用 `git add` 之前，你可以使用 `git status` 来查看哪些文件已经修改，哪些是新添加的，哪些是删除的。这样，你可以有选择地添加文件，而不需要全部添加。
>
> ```bash
> git status
> ```



###### 2）使用 commit 语句将暂存区的修改保存为一个本地提交，更新本地分支

```bash
git commit -m "final"
# -m 后的字符串标注了本次提交的描述信息，可自定义，会在Github对应文件后面显示
```

<img src="./img/屏幕截图 2025-01-24 163914.png" alt="屏幕截图 2025-01-24 163914" />

> [!NOTE]
>
> ```bash
> git branch
> # 可以通过这个语句查看当前所处的本地分支
> ```



###### 3）使用 push 语句将本地分支的提交推送到 Github 的远程分支

```bash
git push -u origin main
# main为对应推送的分支
```

<img src="./img/屏幕截图 2025-01-24 163921.png" alt="屏幕截图 2025-01-24 163921" />

> [!NOTE]
>
> `-u` 或 `--set-upstream` 会使得本地分支与远程分支建立关联，使得后续的 `git pull` 和 `git push` 操作无需显式地指定远程和分支。Git 会自动将 `origin/main` 作为本地 `main` 分支的上游分支。
>
> **没有 `-u` 选项**：
>
> ```bash
> git pull origin main
> git push origin main
> ```
>
> **使用了 `-u` 选项**：
>
> ```bash
> git pull   # 自动从 origin/main 拉取
> git push   # 自动推送到 origin/main
> ```



### 四、总结（省流版）<a name="final"></a>

==如果确保各种配置都没有问题，可以直接进行下面的步骤==

- 初始化仓库

```bash
# 先在 Github 上创建远程仓库
# 在项目文件夹打开 Git Bash 
git init
git remote add origin [远程仓库https网址]
git pull origin main
git add .
git commit -m "[描述信息]"
git push -u origin main
```

- 本地项目有了新的更改

```bash
# 在项目文件夹打开 Git Bash 
git add .
git commit -m "[描述信息]"
git push
```

- 需要从 Github 上远程项目仓库获取最新的 main 分支代码

```bash
# 在项目文件夹打开 Git Bash 
git pull
```

