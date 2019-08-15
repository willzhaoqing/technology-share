# Front-End workflow 前端的工作流
前端工作需要完善的工作流，防止协作中的错误，增加产出的成功率
1. 明确需求和功能  
因为语言沟通确实存在“错位”问题，为了避免，需要“心妮”或产品助理帮助确认。  
另外，为了双方沟通更明确，最好是一个任务一个TaskID，在openproject或jira上。需求文档或相关资料也请PMO上传。

2. 分支工作流  
拿到任务之后，需要新建分(下面有更详细说明)支，分支命名规则：开发人员-任务，例如`git create -b paul-HLA master`
如果是其他人，比如Blake新建了platform分支的开发任务`git create -b paul-platform platform`

3. 任务本地开发完成后，需要自测  
根据文档、设计稿，自己进行对照；

4. 其它部门review  
自测结束，请UI先review，然后是QA部门，最后产品验收

5. 完成开发，提交PR： from ‘paul-platform’ to ‘platform’ ，或 ‘paul-HLA to ‘master’ 方便admin成员进行CodeReview

## 沟通规则

+ feature:   
功能
+ issue description:   
任务说明
+ links:   
jira或slack 链接
+ problem:  
在什么环境/情况下遇到了什么问题。  
API版本  
后台地址  
请求头、请求体  
返回数据（message，body）  
+ recurrent:  
如何重现该问题。
+ result:  
你希望得到什么结果/帮助。

## PR规则
提交PR时，需要写清本次开发的内容  
```
## Description
-------
英文内容 Summary of changes made


## Links
-----
JIRA ticket and/or Slack thread if applicable


## Images
-----
图片内容 Section for screenshots pertaining to the PR


## What to test
------------


## Notes
------------


```

## 分支操作&shell
公司目前的分支系统和规则如下：

`master`: all development branches off of master (if you’re creating a new feature/fixing something, you branch from latest master. while you’re developing, you keep your branch as close to master as possible. when you’re done, create a PR, and have it QA-ed and then once approved, it merges back to master)

`staging`: before something is released to production, master is merged into staging. that’s where QA can test all the new things in one place. they have to test each feature PR, but testing the entire product before a release comes off of this branch

`production`: once everything is approved, staging is merged into production. all deployments happen from production

如无特殊说明，从`master`上拉取分支，开发`dev`完成可以进行提测，将代码放到`staging`分支，通知QA或UI进行Review，通过Review和PR后，将`staging`合并到`production`准备上线发版；和国内常用的`test`=>`release`一样。

如果多任务多人协作，那么以“类型-人名-任务名”做为规范，例如：`dev/paul/feature01`,`staging/paul/feature01`,`staging/paul/feature02`。这样提交`PR`，进行code-review。

* clone 到本地`git clone`
* 切换分支或重置某文件`git checkout `
* 版本回退，可查询git log里的 HEAD^ `git reset`
* 解决冲突和提交
```
git pull && git merge branch && git push origin branch
```
* 如果多人协作的情况下，有多个PR，一旦发生不能自动merge的情况，需要RD自行merge 父级分支，比如你从master上拉取，那么就
```
git pull && git checkout your-branch && git merge master
```

