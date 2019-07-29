# npm全局安装没有权限解决方法
大家在用非自己电脑的时候，全局安装某些依赖或方法，会出现无法调用或权限问题，有三种解决方案
## 更改目录的权限  
```
sudo chown -R 你的用户 /usr/lib/node_modules 
```
每次安装可能都需要进行调整，看具体的情况。


## 官方解决方案  
[链接 https://docs.npmjs.com/getting-started/fixing-npm-permissions](https://docs.npmjs.com/getting-started/fixing-npm-permissions)

官方给出的解决方案就是重新安装nodejs或者自定义全局安装node module的目录，但是对于如果已经开发很长时间的电脑再重新指定的话，成本过高。

## 第三种方案  
就是将没有权限的目录赋予权限：
```
sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}
```

最后一种方案几乎是一劳永逸，需要了解Linux核心的权限和操作命令

