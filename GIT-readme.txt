$ pwd 追踪文件位置

$ mkdir +foldername+ 创建空文件夹

$ git init 安装空git到目录位置

$ git add +filename.figitletype+ 将文件加到git仓库

$ git commit -m "file description" 将文件提交到仓库，*-m “file description”可省略*

$ git status 显示文件夹里的文件有无修改、提交

$ git diff 显示文件被修改的地方

提交修改和提交文件的步骤是一样的

$ git log 显示版本目录
$ git log --pretty=oneline

$ git reset --hard HEAD^ HEAD表示当前版本,HEAD^表示上一个版本，以此类推;HEAD~100表示往上数第100版本
$ git reset --hard +commit id+ 恢复到制定id的版本状态/*id可以不全写*/

$ git reflog 显示你的每一条命令历史

$ git checkout - b +branch name+ 创建并切换分支
$ git branch +branch name+ 创建分支
$ git checkout +branch name+切换分支

$ git branch 查看所有分支，加*的代表当前分支

$ git merge +branch name+合并指定分支到当前分支

$ git branch -d +branch name+ 删除指定分支