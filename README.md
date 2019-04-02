# technology-communication
 Zilly-FE-China technology share, communication


## React项目架构
> The idea about "HLA" or "HLB" project in all React-Project 

1. By type  
"依照类型(by type)"的集中组织方式。用actions, constants, reducers, containers, components等目录区分，文档名可以依功能或应用命名区分，这大概是最常见的一种。范例如下:  

directory structure is as follows:  
```
├── config
├── datas
├── actions
│   ├── user.js
│   ├── hla.js
│   └── hlb.js
├── components
│   ├── header.js
│   ├── footer.js
│   ├── Sidebar.js
│   ├── Command.js
│   ├── UserProfile.js
│   ├── User*.js
├── containers
│   ├── App.js
│   └── User.js   
├── reducers   
│   ├── App.js
│   └── User.js
├── router.js
├── index.js
└── serviceWorker.js
```

2. By feature  
"依照功能(by feature)"的集中组织方式。先以功能或应用领域不同的目录区分，目录里有各自的reducer、action等等文档，可以用文档命名再作类型区分。范例如下:  

directory structure is as follows:  

```
├── config
├── datas
├── app
│   ├── header.js
│   └── footer.js
├── product
│   ├── ProductActions.js
│   ├── ProductList.js
│   ├── ProductItem.js
│   ├── ProductReducer.js
│   └── index.js
├── user
│   ├── UserActions.js
│   ├── UserList.js
│   ├── UserItem.js
│   ├── UserReducer.js
│   └── index.js
├── router.js
├── index.js
└── serviceWorker.js
```

3. By project
"鸭子-模组化Redux"的代码组识方法，它是把reducers, constants, action types与actions打包成模组来用。鸭子可以减少很多目录与文档结构，范例如下:  

directory structure is as follows:  

```
├── config
├── datas
├── modules
│   ├── home
│   │   ├── duck.js                       // reducers, action creators, constants in one file and export function
│   │   ├── container.js                       // import duck 
│   │   ├── router.js                       // route for this module, it can write at src/router
│   │   └── index.js                       // import container, components, router and export
│   ├── user                                // Lauri dev
│   │   ├── duck.js
│   │   ├── container.js
│   │   ├── router.js
│   │   └── index.js
│   ├── HLA                                 // paul dev
│   │   ├── duck.js
│   │   ├── container.js
│   │   ├── router.js
│   │   └── index.js
├── router                          // add by project need, this part under global control
│   ├── home.js
│   ├── aboutus.js
│   ├── user.js
│   └── otherLayout.js
├── plugins
│   ├── getData.js
│   └── Layout.js
├── index.js
└── serviceWorker.js
```

### Thinking

第三种模式，不用再每次开发前都需要去拆分代码，大大降低代码维护的成本。除了全局掌控者，所有人管理好自己的模块就好。也可以保密。
需求阶段：独立团队、独立需求，互不干扰
开发过程：通过Ducks模式，可以物理上隔离代码，不管是hla或hlb，只需要关注自己的代码模块，调用的全局plugins可以在package.json中写好，安装编写好的脚手架即可
组件抽离：components上升到plugins，即可；都是局部代码，泄密性很小
交付阶段：build好的版本可通过nginx路由配置，如果源代码，一起build也可以；两种都能很好的融入现有的项目结构  

I think maybe we can use mode 3, in addition to the master, everyone should manage their own modules. And it can keep secret.  
+ Requirements phase:   
independent team, independent requirements, non-interference.  
+ Development process:   
through Ducks mode, you can physically isolate the code, whether it is hla or HLB, just need to focus on their own code module, call the global plugins can be written in package.  
+ Extraction of components:   
increase to plugins with components; It's all local code, with minimal leaks.  
+ Delivery phase:   
build version can be configured through nginx routing, if the source code, build together can also be; Both fit well into the existing project structure.  

### cli

It need one days to demo.
