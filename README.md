# 在Windows上安装Git
msysgit是Windows版的Git，从https://git-for-windows.github.io下载，然后按默认选项安装即可。安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！
安装完成后，还需要最后一步设置，在命令行输入：
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

# 创建版本库
版本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。
首先，选择一个合适的地方，创建一个空目录：
$ mkdir learngit 建立
$ cd learngit    打开
$ pwd            显示所在文件夹
第二步，通过git init命令把这个目录变成Git可以管理的仓库：
$ git init
##把文件添加到版本库
首先这里再明确一下，所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。不幸的是，Microsoft的Word格式是二进制格式，因此，版本控制系统是没法跟踪Word文件的改动的。
##使用Windows的童鞋要特别注意：
千万不要使用Windows自带的记事本编辑任何文本文件。原因是Microsoft开发记事本的团队使用了一个非常弱智的行为来保存UTF-8编码的文件，他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个“?”，明明正确的程序一编译就报语法错误，等等，都是由记事本的弱智行为带来的。建议你下载Notepad++代替记事本，不但功能强大，而且免费！记得把Notepad++的默认编码设置为UTF-8 without BOM即可
把一个文件放到Git仓库只需要两步。
第一步，用命令git add告诉Git，把文件添加到仓库：
$ git add readme.txt
第二步，用命令git commit告诉Git，把文件提交到仓库：
$ git commit -m "wrote a readme file"


Git提供了一个命令git reflog用来记录你的每一次命令：
$ git reflog

我们用git log再看看现在版本库的状态：
$ git log
$ git log --pretty=oneline

$ git add readme.txt
$ git commit -m 'append GPL'

在Git中，用HEAD表示当前版本，也就是最新的提交3628164...882e1e0（注意我的提交ID和你的肯定不一样），上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
$ git reset --hard HEAD^
指定回到未来的某个版本：
$ git reset --hard 3628164


看看readme.txt的内容是不是版本add distributed：
$ cat readme.txt

git status命令可以让我们时刻掌握仓库当前的状态

工作区（Working Directory）
就是你在电脑里能看到的目录
版本库（Repository）
工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
把文件往Git版本库里添加的时候，是分两步执行的：
第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。
你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

用git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别：
$ git diff HEAD -- readme.txt 
每次修改，如果不add到暂存区，那就不会加入到commit中。

git checkout -- file可以丢弃工作区的修改：
$ git checkout -- readme.txt
命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
总之，就是让这个文件回到最近一次git commit或git add时的状态。

命令git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区：
$ git reset HEAD readme.txt
Unstaged changes after reset:
M       readme.txt
git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。

假设你不但改错了东西，还从暂存区提交到了版本库，怎么办呢？还记得版本回退一节吗？可以回退到上一个版本。

小结
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。
场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

删除
你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：
$ rm test.txt
一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：
另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
$ git checkout -- test.txt
git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。


Remotes
这个世界上有个叫GitHub的神奇的网站，从名字就可以看出，这个网站就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库。
http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374385852170d9c7adf13c30429b9660d0eb689dd43a000

添加远程库
要关联一个远程库，使用命令git remote add origin https://github.com/DevilNut/learngit.git；
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

从远程库克隆
要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone https://github.com/DevilNut/learngit.git命令克隆。


分支
创建dev分支，然后切换到dev分支：
$ git checkout -b dev
git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
$ git branch dev 建立
$ git checkout dev 切换
然后，用git branch命令查看当前分支，会列出所有分支，当前分支前面会标一个*号。：
$ git branch
* dev
  master

然后，我们就可以在dev分支上正常提交，修改。
现在，dev分支的工作完成，我们就可以切换回master分支：
$ git checkout master

现在，我们把dev分支的工作成果合并到master分支上,合并指定分支到当前分支。合并后，再查看readme.txt的内容，就可以看到，和dev分支的最新提交是完全一样的。：
$ git merge dev
合并完成后，就可以放心地删除dev分支了：
$ git branch -d dev

Git鼓励大量使用分支：
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>
创建+切换分支：git checkout -b <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>


Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突，我们试试看：
$ git merge feature1
git status也可以告诉我们冲突的文件：
$ git status
我们可以直接查看readme.txt的内容：cat readme.txt.Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
用git log --graph命令可以看到分支合并图。
用带参数的git log也可以看到分支的合并情况：
$ git log --graph --pretty=oneline --abbrev-commit

显示某个文件的内容
vi README.md
:q 退出
F1 help
:qa! 退出vim
:wq 写入并退出

q退出

准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward：
$ git merge --no-ff -m "merge with no-ff" dev

分支策略
在实际开发中，我们应该按照几个基本原则进行分支管理：
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。
所以，团队合作的分支看起来就像这样：
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
ff和no ff的结果都是一样的,都会合并,其最大区别在于,ff会"默默"地合并,其仅仅是将指针移动到分支的最新处(当然删除分支并不会对master产生任何影响),而no ff则是重新建立了一个commit,然后将合并当做一个comiit来处理,自然就产生了合并的信息.
所以,所谓的分支信息丢失,在这里是指:合并的时候,master的log里面没有任何的合并记录!
所以,可想而知,如果你的master很重要,某一次合并后出现问题,而你又是使用ff,这里没有任何记录可以让你回退版本的!(回退只有靠commit id).

Bug分支
每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。
工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？
Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：$ git stash
接着回到dev分支干活了！
刚才的工作现场存到哪去了？用git stash list命令看看：
$ git stash list
工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：
一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
另一种方式是用git stash pop，恢复的同时把stash内容也删了：
你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：
$ git stash apply stash@{0}

Feature分支
每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。
feature-vulcan分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用命令git branch -D feature-vulcan。

多人协作
多人协作的工作模式通常是这样：
首先，可以试图用git push origin branch-name推送自己的修改；
如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
如果合并有冲突，则解决冲突，并在本地提交；
没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！
如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。
这就是多人协作的工作模式，一旦熟悉了，就非常简单。
要查看远程库的信息，用git remote -v：

推送分支
推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：
$ git push origin master

抓取分支
克隆：$ git clone git@github.com:michaelliao/learngit.git
你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支：
$ git checkout -b dev origin/dev
原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：
$ git branch --set-upstream dev origin/dev

小结
查看远程库信息，使用git remote -v；
本地新建的分支如果不推送到远程，对其他人就是不可见的；
从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

标签管理
tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。
创建标签
在Git中打标签非常简单，首先，切换到需要打标签的分支上：$ git checkout master
然后，敲命令git tag <name>就可以打一个新标签：$ git tag v1.0
可以用命令git tag查看所有标签：$ git tag
如果忘了打标签，方法是找到历史提交的commit id，然后打上就可以了：
$ git log --pretty=oneline --abbrev-commit
比方说要对add merge这次提交打标签，它对应的commit id是6224937，敲入命令：
$ git tag v0.9 6224937
标签不是按时间顺序列出，而是按字母排序的。可以用git show <tagname>查看标签信息：
$ git show v0.9
还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：
$ git tag -a v0.1 -m "version 0.1 released" 3628164

操作标签
小结
命令git push origin <tagname>可以推送一个本地标签；
命令git push origin --tags可以推送全部未推送过的本地标签；
命令git tag -d <tagname>可以删除一个本地标签；
命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

使用GitHub
在GitHub上，可以任意Fork开源仓库；
自己拥有Fork后的仓库的读写权限；
可以推送pull request给官方仓库来贡献代码。

自定义Git
让Git显示颜色，会让命令输出看起来更醒目：
$ git config --global color.ui true

忽略特殊文件
忽略文件的原则是：
忽略操作系统自动生成的文件，比如缩略图等；
忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。
忽略某些文件时，需要编写.gitignore；
直接用touch .gitignore就可以在当前目录创建 .gitignore 文件，然后编辑就可以了。
.gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！用git check-ignore命令检查：
$ git check-ignore -v App.class

配置别名
$ git config --global alias.st status
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
配置文件
配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。
配置文件放哪了？每个仓库的Git配置文件都放在.git/config文件中：