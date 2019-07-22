# 前端部门
前端工作分为两部分：  
## tellus(主要任务)
1. website -- bootstrap 为主
2. webapp -- react 为主

[本地资利代码地址：http://10.0.190.210/zilly-fe](http://10.0.190.210/zilly-fe/)  

[网络资利代码地址：https://github.com/zillyinc](https://github.com/zillyinc/)  

## capital（投资网站）
[本地大华代码地址：http://10.0.190.210/mp-web](http://10.0.190.210/mp-web/)   

[网络大华代码地址：http://39.97.180.12:8929/mp-web](http://39.97.180.12:8929/mp-web/)   

### 相关地址
[项目进度管理：https://zillyinc.atlassian.net](https://zillyinc.atlassian.net/browse/WEB)

### 产品文档
每个项目PM或UI 会发到slack里

## 工作规范
[tellus-FE-workflow 工作流](md/workflow.md)  

[tellus-website 样式规范](md/web-standard.md)     持续优化  

[tellus-react-components](md/react-components.md)   

## 职责分配
Paul：  
1. website & webapp
2. 组件、工程化
3. 架构设计
4. 管理

Ninja:  
1. website: zilly-web
2. webapp: zilly-ui && zilly-webapp
3. 投资网站

Webb： 
1. webapp: zilly-ui
2. website: hoa-react-next, resource-guide-redis

Jerry：
1. website: platform
2. webapp: none

Rick： 
1. website: platform
2. webapp: zinder?

Ada： 
1. website: platform
2. webapp: zinder?

------------------

## 工作问题和解决方案
记录工作过程中所遇到的问题，以及思考和最终的解决方案  
[前端工作](md/work.md)   

### 技术体系建立
> 技术架构
搭建好的gitlab可进行内部博客、架构方案与技术分享  
后续做为保密方式，拿到源码后，本地进行开发，我做为统一出口再PR给整体团队。  
HLA各级操作是否MPA下的一个SPA？是。
react开发模式采取Duckx方式  
提取公用部分和页面级别组件
其它技术的特点和实质解决方案？

> 产品-设计-技术-测试
工作流的规范化和文档化？  
工作方式较为松散，效率不高  
从前端开始进行文档的自动化搭建
openproject  

> 内部培训和技术栈建立

[技术分享](md/README.md) -- 前端、框架的整体思路分析，要求团队成员思考和学习  
[Linux常见命令](md/linux-command.md)  
[面试问题](md/interview.md)

### 公用组件、库
T答应给的HK-server还没到位，可考虑在内网先搭建一套  
npm工作流，依赖[nrm](md/nrm.md)和[verdaccio](md/sinopia.md)私有库
常用UI组
业务容器

### 