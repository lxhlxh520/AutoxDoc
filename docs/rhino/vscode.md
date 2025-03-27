# VS CODE 集成

本文档介绍在电脑端使用 [vscode](https://code.visualstudio.com/)进行开发使用 autox v6 api 脚本的方法

使用第二代 nodejs 引擎开发请阅读[这篇文档](../nodejs/vscode.md)

## ts 项目模板

你可以在 [此页面](https://github.com/aiselp/AutoX/releases/tag/v7.1.2) 中下载 v6-project-demo.rar 得到一个基本 ts 项目模板，
解压后使用 vscode 打开，随后运行以下命令

```
npm install
npm i -D autox-v6-api
```

编译项目运行`npm run build`

## 项目结构

解压后项目中有如下文件

- **src** 所有的源代码将在此目录下编写，
  这里一般使用 ts 和 tsx 文件编写代码，你可以使用 rhino 不支持的部分 es6 特性，如`class`，`const`,
  打包时将由`babel`转换为兼容的代码
- **dist** 编译打包生成的代码位置，随着代码量增加，这里面会有越来越多的文件，
  你需要将整个目录复制到手机，再运行里面的主文件(默认为`main.js`)
- **start-ui.js** 该文件在打包后原封不动的复制到 dist 目录下，用于启动 ui 项目

- **eslint.config.mjs** eslint 的配置文件，模板中默认启用了 eslint 代码检查，若出现编辑器报错可在此处调整

## ui 编写

无需额外配置你可参考模板的示例代码直接使用 jsx 编写 ui，输出的代码需要在 autox 版本 v7.1.3 以上才可运行

## 编写 webui 并配合 webview 来显示界面

目前自带的 ui 框架也许对许多人来说不够美观，功能不齐全，
在这里提供了一套使用[vite](https://cn.vitejs.dev/)+[vue](https://cn.vuejs.org/)+[Framework7](https://framework7.io/)+[TypeScript](https://www.typescriptlang.org/) （技术难度较高）编写 webui 界面的方案，并且支持同时编写 autox 代码，并能够通过[JsBridge](./advanced/webViewAndHtml.md#jsbridge)与之通信，

模板下载: [v6-project-webui-demo.zip](https://github.com/aiselp/AutoX/releases/tag/v7.1.2)

使用该模板需要配合 Autox 7.1.3 以上版本才可使用，下载模板后使用 vscode 打开，运行以下命令

```
npm install
npm i -D autox-v6-api
```

安装完成后现在可以运行`npm run dev`启动 vite 调试服务器

如果一切正常，那么现在可以在 src 目录下编写 webui 的代码

在 src-autox 目录下编写 autox 环境中运行的代码，
运行`npm run build-autox`将编译项目，生成的代码在`dist-autox`目录

### 连接到手机并调试

首先将确保你的手机与电脑在同一局域网下，
在手机中打开 autox app ，在侧边栏打开 usb 调试，

随后打开 vite 调试服务器，新建一个终端运行

```
npm run dev-autox
```

第一次运行会要求输入手机的 ip 地址，之后只要回车确认即可，

等待构建完成后将会在手机打开界面，
现在尝试修改 src 目录下的 web 代码界面也会同步更新，
但是修改 src-autox 目录下的文件需要重新运行`npm run dev-autox`才能得到更新

### 保存项目到手机

运行

```
npm run save-autox
```

将会保存项目到手机的脚本主目录下，要注意的是如果有同名的项目会将文件覆盖，
注意备份

## 已有项目仅导入申明文件

首先在你的项目目录运行以下代码安装依赖包

```
npm i -D autox-v6-api
```

随后打开`tsconfig.json`文件，没有可以创建一个，在`include`字段中添加`"node_modules/autox-v6-api/**/*.d.ts"`
即可，如果是新创建的，还需要把你的源代码目录加上，
可参考模板项目的`tsconfig.json`
