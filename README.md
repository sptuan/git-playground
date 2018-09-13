不会用git的机械工程师不是好厨子（指炼丹与炒菜）。

<!--more-->


<h2>1 前言</h2>
<h3>1.1 为什么使用git？</h3>
<del>稍等，我把代码拷给你，你带U盘了吗？</del>

Git是先进的分布式版本控制系统。

<img class="alignnone size-full wp-image-1532" src="https://steinslab.io/wp-content/uploads/2018/09/Snipaste_2018-09-06_16-37-03.png" alt="" width="464" height="320" />

上图的文件组织方式你可能会很熟悉，这是单人进行写作时产生的遗留文件。时隔多日，我无法再回想起每次更新版本时做出的具体改动，但又不得不保留这些单文件。要我找出某一版本特有的内容，无异于上青天。

&nbsp;

我们再把规模扩大，涉及到版本合并、分支控制。如果是10人规模的小型团队进行软件开发呢？再拓展到Linux内核的开发呢？

&nbsp;

Git不只适用于代码版本控制，它适用于任何需要版本控制的场景。近年来Gitbook成为了博客或团队文档的选择。据笔者的道听途说，某传统企业的文档支持系统由原本的企业OA系统docx文档，迁移至自建Gitbook，效果良好。

&nbsp;

<del>先不说Git，代码管理是必备技能，这是过于真实的现实原因。</del>

&nbsp;
<h3>1.2 什么是git？</h3>
git是用于Linux内核开发的版本控制工具。与CVS、Subversion一类的集中式版本控制工具不同，它采用了分布式版本库的作法，不需要服务器端软件，就可以运作版本控制，使得源代码的发布和交流极其方便。git的速度很快，这对于诸如Linux内核这样的大项目来说自然很重要。git最为出色的是它的合并追踪（merge tracing）能力。[1]

&nbsp;

git的许可协议是GNU通用公共许可证 第二版，GNU宽通用公共许可证 2.1版。

&nbsp;
<h3>1.3 git简史</h3>
git是由<a href="https://zh.wikipedia.org/wiki/%E6%9E%97%E7%BA%B3%E6%96%AF%C2%B7%E6%89%98%E7%93%A6%E5%85%B9">林纳斯·托瓦兹(Linus Torvalds)</a>发起。在linux内核经过手工合并的阶段后，Linus于2002年选择了一个商业的版本控制系统BitKeeper，由BitMover公司授权使用。BitKeeper是商业软件，这也一直在社区中引起质疑，因为这不清真这和自由软件的主旨相违背。直到2005年，BitKeeper的著作权者对<a href="https://zh.wikipedia.org/wiki/%E5%AE%89%E5%BE%B7%E9%AD%AF%C2%B7%E5%9E%82%E9%B3%A9">安德鲁·垂鸠(Andrew Tridgell)</a>对BitKeep的逆向工程不满，收回BitKepper的使用许可。

协商未果，Linus决定自行开发版本控制系统。十天时间后，完成了第一个git版本[2]。

此时，一位技术力爆表的暴躁老哥呼啸而过（图文无关）。[3]

<img class="alignnone size-full wp-image-1527" src="https://steinslab.io/wp-content/uploads/2018/09/1625423z22eev22d392oe7.png" alt="" width="525" height="350" />

&nbsp;
<h3>1.4 GitHub</h3>
&nbsp;

太长不看：GitHub是通过Git进行版本控制的软件源代码托管服务。

&nbsp;

GitHub里面的项目可以通过标准的Git命令进行访问和操作。同时，所有的Git命令都可以用到GitHub项目上面。

网站提供了一系列社交网络具有的功能，例如赞(star)、关注(follow)、评论。用户可以通过复刻(fork)他人项目的形式参与开发，并可通过协作示意图来查看有多少开发者参与了开发并追踪最新的复刻版本。此外网站还有Wiki（通过一个名为 gollum 的软件实现）等功能。

GitHub同时允许注册用户和非注册用户在网页中浏览项目，也可以以ZIP格式打包下载。但是用户必须注册一个账号然后才能进行讨论、创建并编辑项目、参与他人的项目和代码审查。[4]

Github分为付费账户和免费账户。对于免费账户，能创建公开的代码仓库；付费账户在此基础上可以创建私有的代码仓库。之前我也有一篇文章记录了申请Student Developer pack的过程，赠送1年期的高级账户权限。

&nbsp;
<h3>1.5 关于本文</h3>
本文是在博主使用实际中总结的笔记。着重点多在于git的使用上，忽略了git背后的原理和实现。而且直接将Github作为实验Playground，是一篇实用向的笔记。其中参考了许多精品的教程资料[5]。其中，官方文档和书籍形式的教程是非常好的资源，详见https://git-scm.com/book/zh/v2。

&nbsp;
<h2>2 开始使用</h2>
<h3>2.1 连接至Github</h3>
git是是一个分布式的版本控制系统，是可以使用远程仓库的。在这篇笔记的实验中，我们直接连接至Github作为远程仓库。

值得注意的是，Github的开放仓库对<b>所有人都是可见的</b>。几天前华住集团住宿信息被脱裤，或因程序员将数据库连接方式暴露在公开仓库中[6]。

&nbsp;
<h4>1 创建SSH Key</h4>
经常通过SSH管理VPS的同学可能对SSH Key非常熟悉。SSH秘钥可以让用户更加方便安全地登录到SSH服务器，无需输入密码。[7]

首先生成自己的SSH秘钥对

<code>ssh-keygen -t rsa -C "youremail@example.com"</code>

生成完毕后能看到主目录下的<code>.ssh</code>文件夹包含<code>id_rsa</code>和<code>id_rsa.pub</code>。<code>id_rsa</code>是“私钥”，绝对不能泄露。<code>id_rsa.pub</code>是公钥，是可以在公网放心泄漏的。

&nbsp;
<h4>2 在Github上添加SSH Keys</h4>
<img class="alignnone size-full wp-image-1534" src="https://steinslab.io/wp-content/uploads/2018/09/github1-1.png" alt="" width="1258" height="715" />
<h4>3 git初始化</h4>
<code>$ git config --global user.name "Your Name"</code>
<code>$ git config --global user.email "email@example.com"</code>

&nbsp;

&nbsp;
<h3>2.2 从GitHub新建库并克隆到本地</h3>
首先创建新的repository。

&nbsp;

我起了个很俗的名字，就叫<code>git-playground</code>吧。

其中可以选择初始化README文件，然后选择许可证。许可证很重要，如果有志将项目发展下去，可以自己看下各开源许可的条款。

&nbsp;

然后将库克隆到本地。
<pre class="lang:default decode:true ">pi@raspberrypi:~/git_playground $ git clone git@github.com:sptuan/git-playground.git

Cloning into 'git-playground'...

The authenticity of host 'github.com (192.30.255.113)' can't be established.

RSA key fingerprint is XXXXXXXXXXXXX.

Are you sure you want to continue connecting (yes/no)? yes

Warning: Permanently added 'github.com,192.30.255.113' (RSA) to the list of known hosts.

remote: Counting objects: 4, done.

remote: Compressing objects: 100% (3/3), done.

Receiving objects: 100% (4/4), done.

remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0

Checking connectivity... done.</pre>
&nbsp;

可以看到目录下拉下了刚才创建的库。
<pre class="lang:default decode:true ">pi@raspberrypi:~/git_playground/git-playground $ ls -al

total 20

drwxr-xr-x 3 pi pi 4096 Jul 17 08:19 .

drwxr-xr-x 3 pi pi 4096 Jul 17 08:18 ..

drwxr-xr-x 8 pi pi 4096 Jul 17 08:19 .git

-rw-r--r-- 1 pi pi 1063 Jul 17 08:19 LICENSE

-rw-r--r-- 1 pi pi   46 Jul 17 08:19 README.md

pi@raspberrypi:~/git_playground/git-playground $

</pre>
&nbsp;
<h2>3 版本控制</h2>
<h3>3.1 提交更改</h3>
现在，我更改了工作目录下的<code>README.md</code>文件。

运行<code>git status</code>查看仓库状态：
<pre class="lang:default decode:true ">pi@raspberrypi:~/git_playground/git-playground $ git status

On branch master

Your branch is up-to-date with 'origin/master'.

Changes not staged for commit:

(use "git add &lt;file&gt;..." to update what will be committed)

(use "git checkout -- &lt;file&gt;..." to discard changes in working directory)



modified:   README.md



no changes added to commit (use "git add" and/or "git commit -a")</pre>
&nbsp;

&nbsp;

可以用 <code>git add</code> 将文件添加至仓库。

初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态。[8]

&nbsp;

<!--more-->

<code>git stauts</code>可以看到仓库文件状态。若要比较文件，可以用<code>git diff</code>
<pre class="lang:default decode:true ">diff --git a/README.md b/README.md

index 21d66a4..3abc867 100644

--- a/README.md

+++ b/README.md

@@ -1,2 +1,4 @@

# git-playground

Playground. Nothing special.

+## Title

+Now I am trying to edit this file.

</pre>
&nbsp;

提交到仓库：

<code>git add README.md</code>

<code>git commit -m ""</code>

<code>git push</code>

&nbsp;
<pre class="height-set:true lang:default decode:true ">pi@raspberrypi:~/git_playground/git-playground $ git add README.md

pi@raspberrypi:~/git_playground/git-playground $ git status

On branch master

Your branch is up-to-date with 'origin/master'.

Changes to be committed:

(use "git reset HEAD &lt;file&gt;..." to unstage)



modified:   README.md



git commit -m "commit-114514"

pi@raspberrypi:~/git_playground/git-playground $ git commit -m "commit-114514"

[master d899a30] commit-114514

1 file changed, 2 insertions(+)

pi@raspberrypi:~/git_playground/git-playground $ ^C

pi@raspberrypi:~/git_playground/git-playground $ git status

On branch master

Your branch is ahead of 'origin/master' by 1 commit.

(use "git push" to publish your local commits)

nothing to commit, working directory clean

pi@raspberrypi:~/git_playground/git-playground $ git push

warning: push.default is unset; its implicit value has changed in

Git 2.0 from 'matching' to 'simple'. To squelch this message

and maintain the traditional behavior, use:



git config --global push.default matching



To squelch this message and adopt the new behavior now, use:



git config --global push.default simple



When push.default is set to 'matching', git will push local branches

to the remote branches that already exist with the same name.



Since Git 2.0, Git defaults to the more conservative 'simple'

behavior, which only pushes the current branch to the corresponding

remote branch that 'git pull' uses to update the current branch.



See 'git help config' and search for 'push.default' for further information.

(the 'simple' mode was introduced in Git 1.7.11. Use the similar mode

'current' instead of 'simple' if you sometimes use older versions of Git)



Counting objects: 3, done.

Delta compression using up to 4 threads.

Compressing objects: 100% (3/3), done.

Writing objects: 100% (3/3), 354 bytes | 0 bytes/s, done.

Total 3 (delta 0), reused 0 (delta 0)

To git@github.com:sptuan/git-playground.git

92599e0..d899a30  master -&gt; master

pi@raspberrypi:~/git_playground/git-playground $

</pre>
&nbsp;
<h3>3.2 版本记录</h3>
我们先来对README.md进行多次修改和提交。

然后使用<code>git log</code>查看近三次提交。可以加上<code> --pertty=oneline</code>。

具体参数可以参见<a href="https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2">https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2</a>

&nbsp;
<pre class="height-set:true lang:default decode:true ">pi@raspberrypi:~/git_playground/git-playground $ git log

commit 83c0ea033ee080de96d2667d79c4cf77e1c4ea3d

Author: SPtuan &lt;sptuan@steinslab.xyz&gt;

Date:   Tue Jul 17 10:01:48 2018 +0800



+1919810



commit 01ad95d0a73a1f61a93de43d20d5c3899e9e5e55

Author: SPtuan &lt;sptuan@steinslab.xyz&gt;

Date:   Tue Jul 17 10:01:21 2018 +0800



+114514



commit d899a307a13ec67413ed1959a0c4fdb584fbd66f

Author: SPtuan &lt;sptuan@steinslab.xyz&gt;

Date:   Tue Jul 17 09:45:34 2018 +0800



commit-114514



commit 92599e059341b18593bd67ad501bcae5b5f207f5

Author: SPtuan &lt;sptuan@users.noreply.github.com&gt;

Date:   Thu Sep 6 21:05:25 2018 +0800



Initial commit

pi@raspberrypi:~/git_playground/git-playground $ git log --pertty=oneline

fatal: unrecognized argument: --pertty=oneline

pi@raspberrypi:~/git_playground/git-playground $ git log --pretty=oneline

83c0ea033ee080de96d2667d79c4cf77e1c4ea3d +1919810

01ad95d0a73a1f61a93de43d20d5c3899e9e5e55 +114514

d899a307a13ec67413ed1959a0c4fdb584fbd66f commit-114514

92599e059341b18593bd67ad501bcae5b5f207f5 Initial commit

pi@raspberrypi:~/git_playground/git-playground $</pre>
&nbsp;

&nbsp;
<h3>3.3 版本撤销/回退</h3>
若提交后发现有若干改动没有添加，可以使用<code>--amend</code>选项尝试重新提交

<code>git commit --amend</code>

最终这次提交会代替上次提交结果

&nbsp;

版本回退：

<code>git reset --hard HEAD^</code>

其中，<code>git reset</code>用于回退版本。虽然在调用时加上 <code>--hard</code> 选项可以令 <code>git reset</code> 成为一个危险的命令（可能导致工作目录中所有当前进度丢失！）

<code>HEAD</code>可以理解为一个版本的指针。进行回退时，指针直接指向回退的版本。

<code>^</code>表示上一个版本。

可以用<code>git reflog</code>查看回退版本指针情况。

对于后悔药，为<code>git reset --hard commit_id</code>

&nbsp;

这里摘抄一段穆雪峰老师的评论：

假设一开始你的本地和远程都是：

&nbsp;

a -&gt; b -&gt; c

&nbsp;

你想把HEAD回退到b，那么在本地就变成了：

&nbsp;

a -&gt; b

&nbsp;

这个时候，如果没有远程库，你就接着怎么操作都行，比如：

&nbsp;

a -&gt; b -&gt; d

&nbsp;

但是在有远程库的情况下，你push会失败，因为远程库是 a-&gt;b-&gt;c，你的是 a-&gt;b-&gt;d

&nbsp;

两种方案：

&nbsp;

push的时候用--force，强制把远程库变成a -&gt; b -&gt; d，大部分公司严禁这么干，会被别人揍一顿

&nbsp;

做一个反向操作，把自己本地变成a -&gt; b -&gt; c -&gt; d，注意b和d文件快照内容一莫一样，但是commit id肯定不同，再push上去远程也会变成 a -&gt; b -&gt; c -&gt; d

&nbsp;

简单地说就是你无法容易地抹去远程库的提交信息，所以本地提交怎么都行，push前想好了

&nbsp;

使用 git revert &lt;commit_id&gt;操作实现以退为进，

git revert 不同于 git reset  它不会擦除"回退"之后的 commit_id ,而是正常的当做一次"commit"，产生一次新的操作记录，所以可以push，不会让你再pull

&nbsp;

&nbsp;

&nbsp;

实验记录
<pre class="height-set:true lang:default decode:true ">pi@raspberrypi:~/git_playground/git-playground $ git reset --hard HEAD^

HEAD is now at 01ad95d +114514

pi@raspberrypi:~/git_playground/git-playground $ git reflog

01ad95d HEAD@{0}: reset: moving to HEAD^

83c0ea0 HEAD@{1}: commit: +1919810

01ad95d HEAD@{2}: commit: +114514

d899a30 HEAD@{3}: commit: commit-114514

92599e0 HEAD@{4}: clone: from git@github.com:sptuan/git-playground.git

pi@raspberrypi:~/git_playground/git-playground $ git reset --hard 83c0ea0

HEAD is now at 83c0ea0 +1919810

pi@raspberrypi:~/git_playground/git-playground $ git reflog

83c0ea0 HEAD@{0}: reset: moving to 83c0ea0

01ad95d HEAD@{1}: reset: moving to HEAD^

83c0ea0 HEAD@{2}: commit: +1919810

01ad95d HEAD@{3}: commit: +114514

d899a30 HEAD@{4}: commit: commit-114514

92599e0 HEAD@{5}: clone: from git@github.com:sptuan/git-playground.git

pi@raspberrypi:~/git_playground/git-playground $</pre>
&nbsp;

&nbsp;
<h3>3.4 工作区与暂存区</h3>
实际我们进行正常更新文件的目录就可以看做我的工作区。而在工作区下有一个隐藏目录.git，是Git的版本库。
<pre class="height-set:true lang:default decode:true ">pi@raspberrypi:~/git_playground/git-playground $ ls -la

total 20

drwxr-xr-x 3 pi pi 4096 Jul 18 08:00 .

drwxr-xr-x 3 pi pi 4096 Jul 17 08:18 ..

drwxr-xr-x 8 pi pi 4096 Jul 18 08:00 .git

-rw-r--r-- 1 pi pi 1063 Jul 17 08:19 LICENSE

-rw-r--r-- 1 pi pi  107 Jul 18 08:00 README.md</pre>
&nbsp;

&nbsp;

git中有个叫stage的暂存区，最开始时，还包含git自动创建的分支master，和指向master的指针<code>HEAD</code>。

<img class="alignnone size-full wp-image-1521" src="https://steinslab.io/wp-content/uploads/2018/09/0.jpg" alt="" width="458" height="234" />

<code>git add</code> 是将文件修改添加到暂存区，<code>git commit</code>是将暂存区所有内容提交到当前分支。

&nbsp;

值得指出的是，git追踪的文件的修改。如果做出以下操作：

<code>第一次修改</code> -&gt; <code>git add</code> -&gt; <code>第二次修改</code> -&gt; <code>git commit</code>

commit后提交的是第一次修改的结果，因为<code>git add</code>将第一次修改放入了暂存区域。

&nbsp;

若要撤销更改，可以使用

<code>git checkout -- [file]</code>

使工作区的该文件撤回到最近一次 <code>git commit</code>或者<code>git add</code>的状态。

&nbsp;

若要删除文件，可先rm掉文件，再使用<code>git remove</code>。若要恢复误删文件，可使用<code>git checkout -- [file]</code>

&nbsp;
<h2>3 分支</h2>
分支是非常重要的功能！可以把分支理解成一个一个的开发线。在各自有自己功能的分支上或多人协作分支开发，最后合并，安全便捷。

举个不恰当的例子，就像下图一样，只不过各个分支可以根据项目需要在最后合并。

<img class="alignnone size-full wp-image-1528" src="https://steinslab.io/wp-content/uploads/2018/09/bfae17b6gy1frrns5eieuj21hc0u00vb.jpg" alt="" width="1920" height="1080" />

图： 底特律:便乘人 剧情分支图（不恰当的例子）

&nbsp;
<h3>3.1 创建与合并分支</h3>
实际上HEAD指针指向的是master。在我们之前的常规提交，master分支不断向前，HEAD指针也随着向前进。

<img class="alignnone size-full wp-image-1522" src="https://steinslab.io/wp-content/uploads/2018/09/1.png" alt="" width="301" height="151" />

现在我们创建新的分支，用于开发，起名为dev。

<code>git branch dev</code>

<code>git checkout dev</code>

&nbsp;

&nbsp;

&nbsp;

在dev分支上不断commit，<code>HEAD</code>指向<code>dev</code>。

<img class="alignnone size-full wp-image-1523" src="https://steinslab.io/wp-content/uploads/2018/09/2.png" alt="" width="494" height="233" />

对于合并，最简单的的是将master指向dev。

<img class="alignnone size-full wp-image-1524" src="https://steinslab.io/wp-content/uploads/2018/09/3.png" alt="" width="423" height="222" />

Git鼓励大量使用分支：

&nbsp;

查看分支：<code>git branch</code>

&nbsp;

创建分支：<code>git branch</code> &lt;name&gt;

&nbsp;

切换分支：<code>git checkout</code> &lt;name&gt;

&nbsp;

创建+切换分支：<code>git checkout -b</code> &lt;name&gt;

&nbsp;

合并某分支到当前分支：<code>git merge</code> &lt;name&gt;

&nbsp;

删除分支：<code>git branch -d</code> &lt;name&gt;

&nbsp;

将本地新创建的分支推送到github仓库。

<code>git push origin dev</code>

&nbsp;
<h3>3.2 冲突解决</h3>
很容易想到，如果我们从<code>master</code>分支分出<code>dev</code>，那么<code>dev</code>和<code>master</code>都进行了新的commit，这样就需要我们解决冲突问题。

<img class="alignnone size-full wp-image-1525" src="https://steinslab.io/wp-content/uploads/2018/09/4.png" alt="" width="425" height="272" />
<pre class="lang:default decode:true ">pi@raspberrypi:~/git_playground/git-playground $ git merge dev

Auto-merging README.md

CONFLICT (content): Merge conflict in README.md

Automatic merge failed; fix conflicts and then commit the result.</pre>
&nbsp;

&nbsp;

查看冲突：
<pre class="lang:default decode:true ">pi@raspberrypi:~/git_playground/git-playground $ git status

On branch master

Your branch is ahead of 'origin/master' by 1 commit.

(use "git push" to publish your local commits)

You have unmerged paths.

(fix conflicts and run "git commit")



Changes to be committed:



new file:   G.I.



Unmerged paths:

(use "git add &lt;file&gt;..." to mark resolution)



both modified:   README.md

</pre>
&nbsp;
<pre class="lang:default decode:true "># git-playground

Playground. Nothing special.



&lt;&lt;&lt;&lt;&lt;&lt;&lt; HEAD

## RA2 master

- IFV 1

=======

## RA2

- IFV

- G.I.

&gt;&gt;&gt;&gt;&gt;&gt;&gt; dev</pre>
&nbsp;

&nbsp;

看到了2个分支的冲突之处。

修改后使用<code>git add</code>该文件，可以提交。

之后的版本关系为

<img class="alignnone size-full wp-image-1526" src="https://steinslab.io/wp-content/uploads/2018/09/5.png" alt="" width="551" height="272" />

使用<code>git log --graph</code>
<pre class="lang:default decode:true ">pi@raspberrypi:~/git_playground/git-playground $ git log --graph

*   commit 29d6136d1276224f3944d1467659609bcb229080

|\  Merge: dc4e3df 76cc090

| | Author: SPtuan &lt;sptuan@steinslab.xyz&gt;

| | Date:   Fri Sep 7 23:47:20 2018 +0800

| |

| |     conflict fixed

| |

| * commit 76cc090c3fd57349cdf6c8e0bc31aadf3b186108

| | Author: SPtuan &lt;sptuan@steinslab.xyz&gt;

| | Date:   Fri Sep 7 23:40:52 2018 +0800

| |

| |     Add G.I.

| |

| * commit 365220bd7df24007964c5d661bdd6f825923b4d4

| | Author: SPtuan &lt;sptuan@steinslab.xyz&gt;

| | Date:   Fri Sep 7 23:29:16 2018 +0800

| |

| |     Add G.I.

| |

* | commit dc4e3df1a9f5d28beb7cecd97d2b01a05b48189d

|/  Author: SPtuan &lt;sptuan@steinslab.xyz&gt;

|   Date:   Fri Sep 7 23:42:04 2018 +0800

|

|       Edit README

|

* commit 0b7cc5500872a215c36eda4b2925e1a0ab030c19

| Author: SPtuan &lt;sptuan@steinslab.xyz&gt;

</pre>
&nbsp;
<h2>4 本文小结</h2>
本文是在博主使用实际中总结的笔记。着重点多在于git的使用上，忽略了git背后的原理和实现。

本文作为一个Start Up，暂时还不完整。

&nbsp;

在下一期中，将补全以下内容
<ul>
 	<li>实际环境中的使用技巧</li>
 	<li>团队开发、多人协作注意点</li>
 	<li>可视化</li>
 	<li>在Visual Studio使用Git</li>
</ul>
&nbsp;

&nbsp;
<h2>参考资料</h2>
[1] git - Wikipedia <a href="https://zh.wikipedia.org/wiki/Git">https://zh.wikipedia.org/wiki/Git</a>

[2] Git十岁了！Git之父Linus Torvalds说古，大谈Git开发秘辛 <a href="https://www.ithome.com.tw/news/95088">https://www.ithome.com.tw/news/95088</a>

[3] <a href="https://linux.cn/article-640-1.html">https://linux.cn/article-640-1.html</a>

[4] GitHub - Wikipedia <a href="https://zh.wikipedia.org/wiki/GitHub">https://zh.wikipedia.org/wiki/GitHub</a>

[5] 廖雪峰的git教程 <a href="https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000">https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000</a>

[6] 华住旗下酒店5亿信息疑被泄，专家：或因华住程序员失误所致 - 澎湃新闻 <a href="https://baijiahao.baidu.com/s?id=1610052563948067365&amp;wfr=spider&amp;for=pc">https://baijiahao.baidu.com/s?id=1610052563948067365&amp;wfr=spider&amp;for=pc</a>

[7] SSH keys - ArchLinux Wiki <a href="https://wiki.archlinux.org/index.php/SSH_keys_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)">https://wiki.archlinux.org/index.php/SSH_keys_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)</a>

[8] Git-基础-记录每次更新到仓库 - git-scm <a href="https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%AE%B0%E5%BD%95%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E5%88%B0%E4%BB%93%E5%BA%93">https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%AE%B0%E5%BD%95%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E5%88%B0%E4%BB%93%E5%BA%93</a>
