查看远程仓库名称和地址
git remote -v

添加一个新的远程仓库名字和地址
git remote add [remote repo name] [remote repo url]

把项目推送到远程仓库
git push -u [remote repo name] [branch name]

获取最近完整commit id
git rev-parse HEAD

获取最近短的commit id
git rev-parse --short HEAD

配置SSH key后测试github是否能连接
ssh -T git@github.com
or
ssh -T -v git@github.com // 可查看详细信息

查看本地库有没有已经关联的远程库
git remote -v

删除本地已经关联的远程库
it remote remove origin

添加关联的远程仓库
git remote add origin 远程库的地址（可以使用https或者ssh的方式新建）

将本地库中的改动推送到远程库中
git push -u origin master

pull远程分支
git pull origin master --allow-unrelated-histories
