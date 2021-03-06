

### 第一步 创建项目分支

+ 1、fork项目到自己的仓库


```shell
fork项目(以xx/xx_admin为例)
搜索zq_admin,找到xx/xx_admin，点击进入
配置ssh，请自行百度完成
点击fork，把xx_admin fork到自己的仓库
进入自己的仓库，复制仓库地址，git clone ‘你的仓库地址’，这样就把你的仓库拉取到了本地
环境的配置和搭建，可以参考README进行


```

+ 2、克隆仓库到本地
+ 3、使用upstream关联fork的原项目
+ 4、基于upstream/rc创建开发分支


```shell
git clone {自己的仓库}
git remote add upstream fork的原仓库
git remote -v #查看远程关联的库


git fetch upstream # 从upstream拉取最新的代码
git  checkout -b feature-demo  upstream/rc # 基于upstream/rc创建本地开发分支，接下来就可以进行功能开发了。
```

### 第二步 项目代码开发

请根据具体的业务需求进行代码编写。

### 第三步 git代码提交


+ 1、把增加和修改的代码文件加到git仓库
+ 2、commit提交
+ 3、代码推到自己的远端仓库


操作命令:


```shell
git add {增加|修改的文件}
git commit -m '注释{尽量把这次做的事情描述清楚}'
git push origin {开发分支名}
```


### 和upstream做rebase操作


+ 1、基于git代码提交分支做
+ 2、先fetch upstream到最新代码
+ 3、执行rebase命令
+ 4、完成后推送到自己的远端仓库


操作命令:


```shell
git fetch upstream
git rebase upstream/rc
# 看看有没有冲突
git status


# 如果分支已经之前推到远端了，那么rebase完后  push要加 -f 参数
git push origin {开发分支名} [-f]
```


#### rebase有冲突解决流程


+ 1、正常解决办法
	+ 先找到冲突文件，把冲突的地方解决
	+ 解决时要根据业务来保留代码
	+ 修改完后把冲突文件加到git里
	+ 执行git rebase --continue
	+ 如果还有冲突，继续以上步骤，直到没有冲突


操作命令:


```shell
git rebase upstream/rc
# 查看有没有冲突
git status
# 有冲突，解决完冲突后
git add {解决的文件}
git rebase --continue
git status
# 没有冲突了
git push origin {开发分支} [-f]
```


+ 2、非常规解决办法(代码commit点太多，冲突按原来1的方式解决需要太多时间，可以考虑用这种方法)
	+ 找到自己项目分支的起点a
    + 代码回退到a点，git reset a，这样项目的所提交就没有进入到仓库里
	+ 再把项目的这些变动加提交到git里， 合成一次提交
	+ 把这个commit记下来
	+ 把当前分支reset到要rebase的分支(git reset --hard upstream/rc)
	+ 再通过cherry-pick方式把那个commit以补丁方式打进来
	+ 再rebase


操作命令:


```shell
# 找到自己项目分支的起点 axxxx
git reset axxxx
git add .
git commit -m '合成一个点的代码'
# 得到 bxxxx
git reset --hard upstream/rc
git cherry-pick bxxxx
# 如果有冲突， 正常解决冲突
git rebase upstream/rc
git push origin {开发分支} [-f]
```

