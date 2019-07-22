## 更快显示和骨架屏

由于最近几年 Angular/React/Vue 的相继推出和流行，前端渲染开始占据主导。这种模式的应用也叫单页应用（SPA, Single Page Application）。但是打完的包往往体积很大，如何优化？  

#### 前端渲染

模式是服务器（多为静态服务器）返回一个固定的 HTML。通常这个 HTML 包含一个空的容器节点，没有其他内容。之后内部包含的 JS 包含路由管理，页面渲染，页面切换，绑定事件等等逻辑，所以称之为前端渲染。  

因为前端要管理的事情很多，所以 JS 通常很大很复杂，执行起来也要花较多的时间。在 JS 渲染出实际内容之前，往往就是大白屏。很多同学都知道可以通过服务端渲染来进行优化。  

#### 服务端渲染

在这波前端渲染流行之前，早期的传统网站采用的模式叫做后端渲染，即服务器直接返回网站的 HTML 页面，已经包含首页的全部（或绝大部分） DOM 元素。其中包含的 JS 的作用大多是绑定事件，定义用户交互后的行为等。少量会额外添加/修改一些 DOM，但无碍大局。  

此外，前端渲染的模式存在 SEO 不友好的问题，因为它返回的 HTML 是一个空的容器。如果搜索引擎没有执行 JS 的能力（称为 Deep Render），那它就不知道你的站点究竟是什么内容，自然也就无法把站点排到搜索结果中去。这对于绝大部分站点来说是不可接受的，于是前端框架又相继推出了服务端渲染（简称 SSR, Server Side Rendering）模式。这个模式和传统网站很接近，在于返回的 HTML 也是包含所有的 DOM，而非前端渲染。而前端 JS 除了绑定事件之外，还会多做一个事情叫做“激活”（hydration），这里就不再赘述了。  

#### 前端解决方案
中国是仅次于美国的App和web应用大国，国内用户对于速度和体验的要求极为变态，真的，大家科学上网去看看东南亚国家，欧洲的web应用，一堆的alert……  

那么怎么在已经没得优化的基础上继续让应用更”快“呢？预期和欺骗  

是的，你没看错，预期是让用户知道页面在加载，并且在内容实际显示之前能够预测内容，那么他们会认为系统更快、更有耐心，这在很大程度上与管理期望和保持用户知情有关。  
对于Web应用程序，这个概念可能包括显示文本，图像或其他内容元素的“模型” - 称为骨架屏💀。可以在网上可以看到，Facebook，Google，Slack等公司使用：  
> SVG component to create placeholder loading, like Facebook cards loading.   

最简单的方案：骨架屏，依赖式加载

### 具体实现
先加载一个没东西的基础页面，然后持续加载js和css以及后台数据，直到完毕再进行正常显示。可以像下面这样，先加载一个画好框架骨头的图片
```
    <html>
    <head>
    <style>
    /*骨架屏的css*/
    </style>
    <link rel="stylesheet" href="index.css">
    <link rel="preload" href="index.css" as="style" onload="this.rel='stylesheet'">
    <!--
    通过preload提前加载引入真实需要的CSS，通过js的onload进行执行，变成正式的css而不是文件；
    js文件也是一样的原理，也可以在代码最下面持续加载；
    -->
    </head>
    <body>
      <div id="app">
        <div class="skeleton-wrapper">
          <img src="data:image/svg+xml;base64,XXXXXX">
        </div>
      </div>
      <!-- 引用 JS -->
    </body>
    </html>
```
还有现有的react解决方案： react-content-loader[地址](https://github.com/danilowoz/react-content-loader)  
```
npm i react-content-loader --save
```
安装之后有很多现成的解决方案和自定义方案，分别举例
+ 现成

```
// import the component
    import { BulletList } from 'react-content-loader'

    const MyBulletListLoader = () => <BulletList />
```

+ 自定义

```
    const MyLoader = () => (
      <ContentLoader height={140} speed={1} primaryColor={'#333'} secondaryColor={'#999'}>
        {/* Pure SVG */}
        <rect x="0" y="0" rx="5" ry="5" width="70" height="70" />
        <rect x="80" y="17" rx="4" ry="4" width="300" height="13" />
        <rect x="80" y="40" rx="3" ry="3" width="250" height="10" />
      </ContentLoader>
    )
```

还有饿了么的webpack插件方案  
[地址](https://github.com/ElemeFE/page-skeleton-webpack-plugin)  
