# 服务器常用 git 命令备忘

## 撤销本地已经通过 commit 提交，但是还没有 push 的修改

git reset --hard commit_id

或者

git reset --hard origin/master

注：

上面的 commit_id 替换为所需的 id

执行上述命令后，再执行 git status 就应该看到类似"Your branch is up to date with 'origin/xxxx'."这样的提示了。

参考这篇文章《Git 丢弃本地修改》

## 本地某个文件的权限没有问题，但在执行类似“git checkout .”命令时被提示“无权限”

这种情况下，即便使用 sudo git checkout . 命令也无济于事，还会将目标文件的权限修改为 root:root 。此时可以考虑使用命令：

git update-index --chmod=+x <file> 

参考：《Windows 克隆远程仓库文件权限被修改的问题》

## 查看当前 git 的 remote url

git remote -v

## 修改当前 git 的 remote url（用例：原本用 HTTPS 拉的库，现在想改为 SSH 了）

git remote set-url origin git@bitbucket.org:food_hwy/foodhwy_website.git

## 同时向多个 remote 分支推送

git remote set-url --add origin git@bitbucket.org:food_hwy/foodhwy_website.git

## 恢复误删除的分支

情况1: 知道删除分支时的 hash 值，那么 git branch <branch_name> <hash_val> 即可。

情况2: 不知道 hash 值。那么，

2.1 先用 git reflog 显示整个本地仓储的commit，包括所有branch的commit，甚至包括已经撤销的commit。

2.2 使用命令 git branch <branch_name> HEAD@{4} 恢复即可，注意：不要照搬“HEAD@{4}”部分。

## 批量删除本地分支

假设本地有太多的分支需要删除，执行命令：

git branch | grep -v "*" | xargs git branch -D

注意：这个命令有点粗暴，它会删除“当前分支”之外的“所有分支”。不过，只要 remote 分支还在，这并没有什么伤害；需要哪个分支时再 checkout 回来即可。

##  批量删除 remote 分支

简单的做法是，先将需要删除的 remote 分支的名字保存为一个 txt 文件（每行一个名字），假设是 your_file.txt 。然后，执行命令：

cat your_file.txt | xargs -I {} git push origin :{}

上面的命令是能够完成任务的，但如果你觉得 xargs -I {} git push origin :{} 部分不太好理解，也可以参考并替换为下面2种写法：

# No.1
git push -d <remote_name> <branchname>

# No.2
git branch -d <branchname>

# Note: In most cases, <remote_name> will be "origin".

## 关于在 Bitbucket 上添加自己的 SSH key

自己的 SSH key 必须要添加到自己“头像”下的“Personal Setting → SSH Keys”中，这样才具备 write/read 权限；如果只是讲 SSH key 增加的项目中，只会有 read 权限。

## 通过 tag 来 checkout 分支

假设我们要将 v20220406 这个 tag 的节点 checkout，并将其命名为“v20220406“。命令如下：

git checkout tags/v20220406 -b v20220406

## 利用 git 来“二分法”查 bug

参考： http://www.ruanyifeng.com/blog/2018/12/git-bisect.html 

## 将文件/目录从 git 中移除（不再跟踪它的变化）

假设不小心将名为“vendor”的目录加入到 git 中了，可以通过下列命令将其移除：

$ git rm -r --cached ./vendor

## 从 server 上 copy 文件到本地

例如，用户 max 从 host 172.31.39.28 上 copy 日志文件到本地，并将文件改名为 admin1-20210713-laravel.log。

scp max@172.31.39.28:/var/www/admin.foodhwy.com/app/storage/logs/20210713-laravel.log ./admin1-20210713-laravel.log

## 命令行下通过 git log 命令查找单个文件的修改历史（配合“管道符”使用）

$ git log -p -- app/routes_test.php | grep init_product_coupon

## git tag 倒序排列

git tag --sort=-v:refname

## Ubuntu 下切换 PHP 版本

sudo update-alternatives --config php


* https://www.atlassian.com/git/tutorials/inspecting-a-repository/git-blame
* 《An Introduction to Git Merge and Git Rebase: What They Do and When to Use Them》https://medium.com/free-code-camp/an-introduction-to-git-merge-and-rebase-what-they-are-and-how-to-use-them-131b863785f
* 《Git pull from a PHP script, not so simple.》通过 PHP 执行 git pull（钩子）https://jondavidjohn.com/git-pull-from-a-php-script-not-so-simple/
