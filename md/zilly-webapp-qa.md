# webapp问题整理：

1. API不清楚，API缺少文档，不清楚每个API的作用，需要一份完整的API文档。
2. 路由权限逻辑不清楚，路由嵌套规则不清楚。有太多的重定向路由了，哪些路由需要权限？权限是如何控制的？
3. 公用API和公用状态都有哪些？是否可以提取出来然后拆分后做注入？
4. 哪一些路由是需要权限的？无权限时是如何做重定向的？有哪些页面有重定向？如何做错误处理的？
5.  如下代码 ：
```  const homeRoute = !isTenant ? '/account/home' : '/account/payments';``` 
这段代码是根据用户角色权限来做重定向的吗？`processNewUserAndRedirect`这个方法看起来像检测用户信息的？在某些情况下会重定向到`/session/role-switcher`， 但是`/session/role-switcher`这个路径对应的组件我没有在页面上找到。

	```{onBasePath && isEmpty(currentUser) && !isBranchLink && <Redirect to="/session/auth/sign-in" />}``` 
	
	这段代码看起来好像是做权限验证的，但是不明白isBranchLink的作用，开起来像是根据查询字符串来验证是否登陆？登陆的业务逻辑不清楚。。。
	
6. 太多类似的代码了，如果要拆分应用，那我必须要清楚所有重定向的地方，需要把类似```this.history.push ``` 或者 ```<Redirect to="/">``` 类似的代码替换成```window.location.href``` 或者 ```<a href="//:">```
7. 全局状态都存放在哪里？是否有做统一管理？各个页面中的reducer是否有相互依赖？每个页面是独立的状态还是有共享状态？
8. 我们需要清楚页面基本的跳转逻辑以及权限控制，有太多的方法没有注释，需要逐行查找才能明白，这样很浪费时间，最好能够直接与当时的项目参与者沟通。

9. 项目内的UI组件d似乎没有文档，我找到了 <strong>stories</strong>文件夹，但是我只看到了两个组件的说明文档，能否提供更详细的使用文档？
