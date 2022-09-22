# BUG分支

每一个bug都可以通过一个新的临时分支来修复, 修复完毕后, 合并分支后, 就可以把临时分支删除.

但接到一个代号404的bug任务时, 就可以创建一个分支`issue-404`来修复它, 但是如果当前正在`dev`分支进行的工作短时间内不能提交,然后去改bug.

***那怎么办?***

其实git贴心的给出了一个`stash`功能, 可以把当前工作现场`储藏`起来,等以后的恢复现场后继续工作.

`git stash` 保存现场后, 可以`git status`查看, 可以看到现在就是"干净"的.

于是我们就可以放心的去修复bug了.

首先,我们需要确定是哪个分支出了bug, 假定是`master`分支上修复, 就从`master`上创建临时分支.

修复完成后, 切回`master`, 完成合并, 最后1删除`issue-404`分支.

然后我们就可以继续回到`dev`上干活了.

现在在`dev`上`git status`, 是干净的.

我们要用`git stash list`来查看.

我们就可以看到工作现场的标记了, 现在就要去恢复它. 有两种方法:

- 先`git stash apply`恢复, 但那个记录还没有被删除, 我们就需要再用`git stash drop`来删除.
- 另外一种方法就是`git stash pop`, 恢复的同时把stash的内容也删了.

如果工作现场的list多, 我们可以在`git stash list`查看后, 用:

`git stash apply stash@{<number>}` 这个nmber就是查看时的目标现场的编号.

如果`dev`分支早期也是从`master`那里来的, 那么`dev`上也有同样的bug, 我们可以在`dev`上修复同样的bug, 再重复一边.

但还有更好的办法, 那就是把刚才还没删除的修改bug完成后的那个分支的提交复制到`dev`分支上.

git提供了一个`cherry-pick`命令, 让我们能复制一个特定的提交到当前分支

`git cherry-pick <特定的提交的id>`

这样git就自动为`dev`分支提交了一个commit, 这个commit其实跟源提交改动是一样的,但他们的id却不同.

然后我们就继续`git stash list`, `git stash pop`恢复现场继续在暂时没有bug的`dev`分支上工作.