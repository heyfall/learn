### 常见linux bash命令

`$ mkdir learngit`            //创建空目录

`$ cd learngit`               //进入learngit文件夹

`$ cd C:`                     //进入C盘

`$ pwd`                       //显示当前所在目录

`$ dir`或`$ ls`               //ls和dir命令可以用来查看当前目录下的文件

`$ rm <file>`                 //删除文件

`$ cat <file>`                //查看文件内容

### 安装git

`$ git config --global user.name "yourname"`       //设置用户名

`$ git config --global user.email "email@example.com"`   //设置邮箱

`$ ssh-keygen -t rsa -C 'youremail@example.com'`   //创建ssh key

`$ ssh-keygen -t rsa -C 'youremail@example.com' -f ~/.ssh/github_id_rsa`  //在ssh目录下创建名为github_id_rsa的公钥

如果需要创建多个ssh公钥，则需要使用``$ ssh-keygen -t rsa -C 'youremail@example.com' -f ~/.ssh/github_id_rsa``来创建一个名为github_id_rsa的公钥，
因为直接使用``$ ssh-keygen -t rsa -C 'youremail@example.com'``命令来创建ssh，会直接覆盖之前生成的；
然后再使用`$ touch ~/.ssh/config`命令创建config文件，粘贴复制以下内容：
```
# gitee
Host gitee.com
HostName gitee.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/gitee_id_rsa
# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
```

如果使用 `$ ssh -T git@github.com` 显示不能连接github，则可以把hostname换为ssh.github.com，在最后一行添加Port 443。

如果还是不能连接，首先到ipaddress 输入 github.com 查找到其IP地址，将查到的IP地址和网址映射放到你的本地 hosts 文件中即可，例子：140.82.112.4 github.com
windows下host默认地址： C:\Windows\System32\drivers\etc

### 创建版本库

`$ git init`                  //将当前所在目录变成git可以管理的仓库

`$ git add <file>`        //将readme.txt添加到暂存区

`$ git commit -m "wrote a readme file"` //git commit是将文件提交到仓库，-m后面输入的是本次提交的说明

### 状态查看

`$ git status`                //查看当前仓库状态

`$ git diff`                  //查看工作区和暂存区的差异

`$ git diff --cached`         //查看暂存区和仓库的差异

`$ git diff --HEAD`           //查看工作区和仓库的差异

`$ git diff <file>`          //查看工作区和暂存区readme文件的差异

`$ git log`                   //查看提交日志，按q可退出

`$ git reflog`                //查看命令历史，以便确定回到哪个版本

### 修改和删除

`$ git reset -- commit_id`    //将版本库的回退到commit_id代表的版本，HEAD为当前版本，HEAD^是上一个版本，HEAD^^是上上个版本

`$ git checkout -- file`      //丢弃工作区的修改，其实是将暂存区的内容代替工作区的内容，无论是修改还是删除，如果暂存区无内容，则是用仓库的内容代替

`$ git reset HEAD <file>`    //撤销暂存区的修改，重新放回工作区

`$ git restore <file>`       //与`$ git checkout -- file`命令一样，撤销工作区的修改，用暂存区的内容替代工作区的内容

`$ git restore --staged <file>`  //与`$ git reset HEAD <file>`命令一样，撤销暂存区的修改（实质是从仓库恢复暂存区），重新放回工作区

`$ git restore --source=HEAD --staged --worktree <file>` //从仓库同时恢复工作区和暂存区

`$ git rm <file>`             //删除文件，并将删文件的修改提交到暂存区

### 远程仓库

`$ git remote -v`             //查看版本库信息

`$ git remote add <name> git@github.com:heyfall/learn.git`  //将本地仓库推送到github，name可为origin，origin是默认习惯名

`$ git remote rm <name>`      //删除版本库，这里的删除其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库

`$ git push -u origin master`     //将本地的master分支内容推送到远程的master分支，并将本地的master分支和远程的master分支关联，下次推送就不同加-u了

`$ git remote set-url origin git@github.com:heyfall/learn.git`   //更新远程仓库的url

这里的remote url是ssh格式，ssh和https格式的差别是ssh格式每次clone push pull都不用验证用户名和密码，https则需要，但是https格式的可以克隆任何组织的仓库

`$ ssh -T git@github.com`     //测试是否能够连接github

`$ git clone <ssh url / https orl>`  //克隆一个远程库到本地

### 分支管理

Git创建分支实际上是增加一个区别于master的指针，比如dev，改变HEAD的指向，将其从指向master改为指向dev，工作区的内容没有任何变化；合并分支最简单的方法就是把master指向dev的当前提交；删除分支实际上就是将dev指针删掉。

`$ git branch dev`             //创建一个名为dev的分支

`$ git checkout dev`           //切换到dev分支

`$ git checkout -b dev`        //创建dev并切换到dev分支

`$ git switch dev`             //切换到dev分支

`$ git switch -c dev`          //创建dev并切换到dev分支

`$ git branch`                 //查看当前分支

`$ git merge dev`              //将dev分支合并到当前分支

`$ git branch -d dev`          //删除dev分支

`$ git branch -D dev`          //强行删除一个未被合并的分支

`$ git push origin -d <branch_name>`   //删除一个远程分支

需要注意的是，如果在A分支下有未完成的工作，切换到B分支的时候，会把在A分支下的工作带过去，如果此时提交，在A分支下的修改会算作B分支的工作；所以可以在跳转分支前git status一下查看是否有没有add和commit的工作，或者使用git stash进行现场保留。

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成；解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

`git merge --no-ff dev`         //使用普通模式合并分支，合并后能看出来做过合并，不加这个参数则是fast forward合并，看不出来曾经做过合并

这里的--no--ff 模式其实就是相当于master指针new了一个跟dev指针一样的空间并且放了相同的内容然后指向这个空间。而原来的快速模式，就是简单将master指针指向dev指针指向的内容而已，并没有自己创造空间。

`$ git stash`                             //保存工作现场

`$ git stash list`                        //查看保存的工作现场

`$ git stash apply stash@{0}`             //恢复保存的工ne作现场，但是恢复后，stash内容并不删除

`$ git stash drop`                        //删除保存的工作现场

`$ git stash pop`                         //恢复并删除保存的工作现场

`$ git cherry-pick <commit_id>`           //复制一个特定的提交到当前分支，比如在master分支修复的bug，想要合并到当前dev分支，可以用这个命令将bug修改的提交复制到当前分支

`$ git pull`                              //拉取最新代码；如果提示no tracking information，说明本地分支和远程分支的链接关系没有创建

`$ git branch --set-upstream-to <branch-name> origin/<branch-name>`   //创建本地分支和远程分支的链接关系

