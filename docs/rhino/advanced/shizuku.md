# shizuku

v7.1.1 新增

要使用该模块请确保已安装[shizuku App](https://github.com/RikkaApps/Shizuku/releases)并激活，
并且授予了此应用权限，对于 root 用户也可以选择安装[Sui](https://github.com/RikkaApps/Sui) (未进行测试)

使用 shizuku 可以以更高的权限执行 shell 命令，以及调用一些系统 api

## shizuku(cmd)

- cmd \{ string } shell 命令
- return \{ [ShellResult](./shell#shellresult) } 运行结果

使用 shizuku 运行一条 shell 命令，如果 shizuku 不可用将抛出错误

```js
shizuku("input tap 500 500");
```

## shizuku.isAlive()

- return \{ boolean }

返回 shizuku 是否可用

## shizuku.runRhinoScript(script)

v7.1.2 新增

- script \{ string } 要运行的脚本内容
- return \{ any } 运行脚本设置的结果

使用 shizuku 所在的进程运行一段脚本，这里运行的脚本具有与 shizuku 相同的权限，

此处运行的脚本是一个全新的脚本环境，<font color="#ef5952">所有常规脚本可使用的 api 这里都无法使用</font>,
可用的对象及 api 将在下方说明。

此方法可通过 java 与安卓类更方便的访问受限功能

```js
let data = shizuku.runRhinoScript(`
    let root = new java.io.File("/");
    console.log(root.listFiles());
    let data = root.listFiles().map((v) => v.path);

    // 设置返回结果，必须是能通过JSON序列化的值
    resultReceiver.setData(data);
`);
console.info(data);
```

## shizuku.runRhinoScriptFile(path)

v7.1.2 新增

- path \{ string } 要运行的脚本路径，只能是绝对路径
- return \{ any } 运行脚本设置的结果

运行来自文件的脚本

## RhinoScriptGlobalScope

这里列出的是上方函数所运行的脚本环境中的全局属性，除了以下属性，
你还可以使用来自 Rhino 的 java 交互 api，如`Packages`,`importClass()`,`importPackage()`,`JavaAdapter`等

- context \{ `Context` } shizuku 所提供的 context,它并不是一个常规 app 中的完整的`Context`，
  你无法使用它来创建 ui，访问资源等等

- global \{ object } 全局对象的引用

- resultReceiver \{ object } 结果接收器，有一个`setData`方法,用于设置此脚本的返回值，
  只能设置能通过 JSON 序列化的值，如`resultReceiver.setData(65)`

- console \{ object } 用于向日志界面打印内容
