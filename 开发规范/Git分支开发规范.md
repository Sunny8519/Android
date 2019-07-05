## Git分支开发规范

以Git Flow的方式开发。

"功能驱动式开发"：先有需求再有功能分支(feature branch)，完成开发后，功能分支合并到主分支，然后被删除。

### 开发方式

- master：主分支，保留稳定版本的代码，上线版本的包都是从该分支打包出来

- develop：开发分支，所有的功能分支都是基于该分支

- feature：基于develop分支拉出来的功能分支，一般按照小功能从develop拉分支开发，开发完成后合并回develop分支，待功能上线后删除，下一个小功能还是基于develop分支出

- release：bug修复以及打包测试分支，出该分支的时机是一个迭代的功能都做完了，并且这些功能都已合并入develop分支后，从develop分支出release分支，表明了一个迭代的功能做完了，后续bug需要在release分支上修改，测试也是基于release分支。个人觉得release分支可实时与develop分支同步，也就是说在release分支上修改的bug可同步到develop分支上，防止团队中其他人员在基于develop分支做下个迭代功能时碰到release上已经修复但是develop上还没修复的bug。当release分支经过测试验证完后，稳定了，已经具备上线条件了，可以把release分支合并入master分支，然后master分支出tag，打包上线。

- hotfix：修复线上问题的分支，基于上次发布的commit拉取出的分支，只用来修复线上bug，修复完后合并入master和develop分支，合并入master后应打tag再打包发布。

### 命名规范

- 主分支名：master

- 主开发分支：develop

- 发布分支名称：release/version，"release"为小写，"version"为版本号，如：release/1.0.0

- 新功能分支名称：feature/version/developer_name/feature_name，"version"为迭代的版本号，与发布分支上的版本号应该保持一致，"developer_name"表示开发者的名字，"feature_name"表示所做功能的简要描述，不建议太长

- master的bug修复分支名称：hotfix/*，*为bug简称或者bug号，如：hotfix/item_update_bug

- 标签(tag)名称：v*.RELEASE，*表示版本号，"RELEASE"为大写，如v1.1.1.RELEASE

### 代码提交规范

1.每次commit都应该尽可能的包含完善的功能，并且每次commit的代码量不宜太大，比如需要做一个大的功能，在开发过程中应该把大的功能拆分成小功能，每个小功能提交一个commit，这样方便代码审查。

2.commit信息规范
```
[xxx]xxxx
```
功能开发的commit：
比如做通讯录功能，应这样提交：`[通讯录]添加通讯录字段name`，`[]`中应是功能的简称，后面改动的说明。

bug修复的commit：
比如修复了`文本不能复制的问题`，可以这样提交：`[fix-issue]文本不能复制`

### 参考

https://docs.gitlab.com/ee/workflow/gitlab_flow.html
[Git工作流程](http://www.ruanyifeng.com/blog/2015/12/git-workflow.html)
[Git工作流指南：Gitflow工作流](https://www.cnblogs.com/jiuyi/p/7690615.html)