Chromium 架构
![[Pasted image 20240105174808.png]]
Electron 架构
![[Pasted image 20240105174504.png]]
## 一、搭建 Electron 项目
首先创建一个新的目录，例如 client，然后使用 `npm init -y` 进行一个初始化。
先配置 Electron 镜像源
```shell
npm config list # 找出npm配置源
```

修改 `C:\Users\fishx\.npmrc`, 追加 `ELECTRON_MIRROR`

```shell
registry=https://registry.npmmirror.com
ELECTRON_MIRROR=https://npmmirror.com/mirrors/electron/
```

接下来需要安装 Electron 依赖:

```shell
npm install --save-dev electron
```

之后分别创建 `index.html`、`index.js` 文件：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Hello Electron!</h1>
    <p>Hello from Electron!!!</p>
</body>
</html>
```

```js
const { app, BrowserWindow } = require("electron");

// 创建窗口
const createWindow = () => {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
    webPreferences: {
      nodeIntegration: true,
    },
  });

  // 加载index.html文件
  win.loadFile("index.html");
};

app.whenReady().then(() => {
  createWindow();

  app.on("activate", () => {
    if (BrowserWindow.getAllWindows().length === 0) {
      createWindow();
    }
  });
});
```

快速上手案列：获取电脑配置
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- 定义内容安全策略（CSP），限制页面只能加载来自当前域的资源，并且仅允许执行来自当前域的脚本 -->
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'"></meta>
    <title>Document</title>
    <link rel="stylesheet" href="index.css">
</head>
<body>
    <div id="cpu" class="info">
        <span>CPU: </span>
        <span>Loading...</span>
    </div>
    <div id="cpu-arch" class="info">
        <span>CPU 架构: </span>
        <span>Loading...</span>
    </div>
    <div id="platform" class="info">
        <span>平台: </span>
        <span>Loading...</span>
    </div>
    <div id="freemem" class="info">
        <span>可用内存: </span>
        <span>Loading...</span>
    </div>
    <div id="totalmem" class="info">
        <span>总内存: </span>
        <span>Loading...</span>
    </div>
    <script src="./window.js"></script>
</body>
</html>
```

```js
const os = require('os');

console.log(os);

function getCpu() {
    const cpus = os.cpus();
    if (cpus.length > 0) {
        return cpus[0].model;
    } else {
        return 'unknown';
    }
}

function covert(bytes) {
    return (bytes / 1024 / 1024 / 1024).toFixed(2) + 'G';
}

document.querySelector('#cpu span:last-child').innerHTML = getCpu();
document.querySelector('#cpu-arch span:last-child').innerHTML = os.arch();
document.querySelector('#platform span:last-child').innerHTML = os.platform();
document.querySelector('#freemem span:last-child').innerHTML = covert(os.freemem())
document.querySelector('#totalmem span:last-child').innerHTML = covert(os.totalmem())
```


## 二、进程与线程
- 什么是进程？
	- 假设我们的电脑看作是一个工厂，电脑上面是可以运行各种应用程序的（浏览器、Word、音乐播放器、视频播放器）
	- 每一个应用程序都可以看作是一个独立的工作区域，这个独立的工作区域就是我们的进程。
	- 每个进程都会有独立的内存空间和系统资源，每个进程之间是独立的，这意味着假设有一个进程崩了，那么不会影响其他的进程。
- 什么又是线程？
	- 刚才我们将进程比做工厂里面一个独立的工作区域，那么每个工作区域都有员工的，一个独立的工作区域是可以有多个员工的，类似的，
	- 一个进程也可以有多个线程，线程之间进行协同工作，共享相同的数据和资源。线程是操作系统所能够调度的最小单位。

> [!note]- 如果一个应用是多进程应用，那么也会有一个“主进程”，起到一个协调和管理其他子进程的作用。
> 例如，在 `Node.js` 里面，我们可以通过 `child_process` 这个模块来创建一个子进程，那么在这种情况下，启动这些子进程的 `Node.js` 应用实例就会被看作是主进程，`child_process` 就是子进程
> 主进程负责管理这些子进程，比如分配任务，处理通信和同步数据之类的。

回到 Electron 桌面应用，当我们启动一个 Electron 桌面应用的时候，该应用对应的也是一个多进程应用，如下图所示
![[Pasted image 20250314002622.png]]

这里面 Electron 是主进程，对应的就是我们应用入口文件的 `index.js`，该主进程负责的任务有：
- 管理整个 Electron 应用程序的生命周期
- 访问文件系统以及获取操作系统的各种资源
- 处理操作系统发出的各种事件
- 创建并管理菜单栏
- 建并管理应用程序窗口

Electron Helper（Renderer）该进程就是我们窗口所对应的渲染进程

假设在任务管理器将该进程关闭掉，我们会发现窗口不再渲染任何的东西，但是应用还存在，窗口也还存在。
> [!tip] 实际上在 Electron 应用中，有一个窗口进程，由窗口进程来创建的窗口，之后才是渲染进程来渲染的页面。这也是为什么我们关闭了渲染进程，但是窗口还存在的原因。

假设我们创建了多个窗口，那么会有多个窗口进程么？

多个窗口下仍然只有一个窗口进程，由这个窗口进程负责绘制多个窗口，不同的窗口里面会有不同的渲染进程来渲染页面。

如下图所示：

![[Pasted image 20250314004236.png]]

最后再明确一个点，一个窗口只能对应一个渲染进程么？

其实也不是，哪怕我是在一个窗口里面，我也是可以有多个渲染进程的。如何做到？通过 `webview` 加载其他的页面，当你使用 `webview`

## 三、主进程和渲染进程通信

> 进程间通信 Interprocess communication 简称 IPC
> IPC 进程通信是操作系统所提供一种机制，允许应用中的不同进程之间进行一个交流

在 Electron 中需要注意：`主进程和渲染进程之间的通信`、`渲染进程之间的通信`

也提供了对应的模块 `ipcMain` 、`ipcRenderer` 来实现两类进程之间的通信

### ipcMain 模块
- `ipcMain.on(channel,listener)`
	- 监听事件，`on` 方法监听 `channel` 频道所触发的事件
	- `listener` 是一个回调函数，当监听频道有新消息抵达时，会执行该回调函数
		- `listener(event,args)`
			- `event` 是一个事件对象
			- `args` 是一个参数列表
- `ipcMain.once(channel,listener)`
	- 和上面 on 的区别在于 once 只会监听一次
- `ipcMain.removeListener(channel,listener)`: 移除 on 方法所绑定的事件监听

### ipcRenderer 模块

基本上和上面的主进程非常的相似。

- `ipcRenderer.on(channel, listener)`
	- 和上面主进程的 `on ` 方法用法一样
-  `ipcRenderer.send (channel,...args)`
	- 此方法用于向主进程对应的 channel 频道发送消息。
	- 注意 send 方法传递的内容是被序列化了的，所以并非所有数据类型都支持

这两个模块实际上是基于 `Node.js` 里面`EventEmitter`模块实现的。例如：

```js
//index. js
const event = require('./event');
//触发事件
event.emit("some_event");
```

```js
//event.js
const EventEmitter = require("events").EventEmitter;
const event = new EventEmitter();
//监听自定义事件
event.on("some_event", () => { console.log("事件已触发"); })
module.exports = event;
```
