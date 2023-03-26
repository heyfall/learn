## Git操作命令：
`$ mkdir learngit`            //创建空目录

`$ cd learngit`              //进入learngit目录

`$ pwd`                       //显示当前所在目录

`$ ls/dir`                    //ls和dir命令可以用来查看当前目录下的文件

`$ rm <file>`                 //删除文件
				            
`$ git init`                  //将当前所在目录变成git可以管理的仓库

`$ git add readme.txt`        //将readme.txt添加到暂存区

`$ git commit -m "wrote a readme file"` //git commit是将文件提交到仓库，-m后面输入的是本次提交的说明

`$ git status`                //查看当前仓库状态

`$ git diff readme.txt`       //查看修改内容

`$ git log`                   //查看提交日志，按q可退出

`$ git reset -- commit_id`    //将版本库的回退到commit_id代表的版本，HEAD为当前版本，HEAD~是上一个版本，HEAD~~是上上个版本

`$ git reflog`                //查看命令历史，以便确定回到哪个版本

`$ git checkout -- file`      //丢弃工作区的修改，其实是将版本库的内容代替工作区的内容，无论是修改还是删除

`$ git reset HEAD <file>`    //撤销暂存区的修改，重新放回工作区

`$ git rm <file>`             //从版本库中删除文件

`$ git remote -v`             //查看版本库信息

`$ git remote rm origin`      //删除版本库，这里的删除其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库

`$ ssh-keygen -t rsa -C 'youremail@example.com'`   //创建ssh key

`$ ssh-keygen -t rsa -C 'youremail@example.com' -f ~/.ssh/github_id_rsa`  //在ssh目录下创建名为github_id_rsa的公钥

`$ git remote add origin git@github.com:heyfall/pengyanlin.git`  //将本地仓库推送到github
`$ git remote set-url origin git@github.com:heyfall/learn.git`   //更新远程仓库的url

`$ touch ~/.ssh/config`       //在ssh目录下新建config文件

`$ ssh -T git@github.com`     //测试是否能够连接github

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

如果使用 `$ ssh -T git@github.com` 显示不能连接github，则可以把hostname换为ssh.github.com，在最后一行添加Port 443

