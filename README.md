# monorepo
## 什么是monorepo？

monorepo是一种软件开发策略，多个项目的代码存储在同一个`repo`中。
## 什么是multirepo？

multirepo与monorepo相反, 不同的项目存储在不同的`repo`中。
## 为什么需要monorepo？

1. 拆分不同的`repo`，虽然可以进行项目隔离。但是如果仓库之间存在依赖时，调试将会恨困难。因为我们需要关注各个包的版本号，调试时需要`npm link`。
2. 难以复用基础配置和代码，如果团队没有通过统一的`cli`创建项目，`eslint`等基础的配置难以复用，需要手动拷贝，效率低下

## npm link

## 如何实现monorepo？

通常的解决方案：yarn workspaces + lerna 
## yarn workspaces

`yarn workspaces`, 可以将多个`node_modules`整合成一个`node_modules`。只需要一次`yarn install`, 就可以安装所有的依赖。

### 如何配置yarn workspaces?

当前的目录结构

```shell

|- package
    |- packageA
        |- package.json
    |- packageB
        |- package.json
|- package.json
```

最外层的`package.json`的文件添加`workspaces`配置项。`workspaces`是数组，里面分别添加需要合并包的路径。

```js
{
  "workspaces": [
    "package/packageA",
    "package/packageB"
  ]
}
```

packageA项目的`package.json`如下

```js
{
  "name": "package-a",
  "version": "1.0.0",
  "dependencies": {
    "vue": "^2.6.12"
  }
}
```

packageB项目的`package.json`如下:

```js
{
  "name": "package-b",
  "version": "1.0.0",
  "dependencies": {
    "react": "^17.0.1"
  }
}
```

在根目录执行`yarn install`, `node_modules`的目录如下：

![image.png](https://i.loli.net/2021/03/02/QwWmTGcfZCdvzhK.png)

## lerna

A tool for managing JavaScript projects with multiple packages.

lerna是一个用于管理拥有多个项目代码的项目的工具。

使用lerna管理的项目，结构大致如下：

```js
|- package
  |- packageA // 项目A
    |- package.json
  |- packageB // 项目B
    |- package.json
  |- packageC // 项目C
    |- package.json
|- package.json
|- lerna.json // lerna的配置文件
```

lerna有两种工作模式`Fixed/Locked mode`, `Independent mode`

### Fixed/Locked mode

Fixed模式是默认模式。Fixed模式下，项目中的所有子项目共用一个版本号。版本保存在lerna.json文件的version字段中。

> Babel使用的就是该模式
### Independent mode

Independent模式允许每个包自行更新版本号，使用`lerna init --independen`命令创建Independent模式的项目。

### lerna常用命令

- `lerna init`, 创建一个lerna的项目。`lerna init --i`, 创建Independent模式。
- `lerna bootstrap`, 
- `lerna publish`, 发布所有已修改的包

### lerna.json

## 实战

## 参考

- https://en.wikipedia.org/wiki/Monorepo
- https://juejin.cn/post/6901665019927527431
- https://juejin.cn/post/6903330914550415373
- https://juejin.cn/post/6855129007185362952
- https://juejin.cn/post/6844903842597830669
- https://juejin.cn/post/6866748110644822023#heading-1
