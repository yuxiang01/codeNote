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

> [!note] 进程间通信 Interprocess communication 简称 IPC
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

### 示例
> [!note] 渲染进程 `http` 请求拿到数据之后，就将这个数据发送给主进程；主进程拿到数据之后给渲染进程回一个话

![[Pasted image 20250323192005.png]]
![[Pasted image 20250323191855.png]]

## 四、渲染进程间通信
![[Pasted image 20250323232704.png]]
1. 定义一个数组，存储所有的窗口对象, 注册 `registerChannelEvent` 、`transTextEvent`, 再定义记录消息通道的数组
2. 渲染进程 2 绑定事件，并将输入的内容发送主进程
3. 渲染进程 1 监听 `action` 事件，并将监听的 `action` 事件注册到主进程中
4. 主进程 `transText` 对所有记录消息通道（消息队列）推送消息 `发布、订阅`

`win02.js`
```js
const { ipcRenderer } = require("electron");

const btn = document.getElementById("btn");
const $input = document.getElementById("content");

btn.addEventListener("click", function () {
  // 将输入的内容发送主进程
  ipcRenderer.send("transTextEvent", "action", $input.value);
});
```

`main.js`
```js
const { app, BrowserWindow, ipcMain } = require("electron");
const path = require("path");
const url = require("url");

// 定义一个数组存储窗口对象
const winRef = [];
// 用于记录消息通道，也就是记录窗口进程要注册的事件
const messageChannelRecord = {}

/**
 * registerChannel 渲染进程注册事件方法
 * @param {*} channel 窗口中要注册的事件
 * @param {*} webContentsId 窗口对应的id
 */
function registerChannel(channel, webContentsId) {
  if (messageChannelRecord[channel] !== undefined) {
    // channel已注册过了，接着判断当前窗口是否已经注册过事件
    let alreadyRegister = messageChannelRecord[channel].includes(webContentsId)
    if (!alreadyRegister) {
      // 没有注册过，则将当前窗口id添加到channel事件中
      messageChannelRecord[channel].push(webContentsId)
    }
  } else {
    // 注册channel事件 { channel: [webContentsId] } 如：{ action: [1, 2], }
    messageChannelRecord[channel] = [webContentsId]
  }
}

/**
 * 获取当前channel事件注册的webContentsId
 * @param {*} channel 
 * @returns {Array} webContentsId[]
 */
function getWebContentsId(channel) {
  return messageChannelRecord[channel] || []
}

/**
 * 传输数据
 * @param {*} webContentsId 注册 channel 事件的窗口id 
 * @param {*} channel 对应的事件
 * @param {*} data 传递的数据
 */
function transText(webContentsId, channel, data) {
  for (let i = 0; i < webContentsId.length; i++) {
   for (let j = 0; j < winRef.length; j++) {
     if (winRef[j].webContents.id === webContentsId[i]) {
      winRef[j].webContents.send(channel, data)
      break
     }
   }
  }
}

// 注册事件
ipcMain.on('registerChannelEvent', (event, channel) => {
  try {
    registerChannel(channel, event.sender.id)
  } catch (error) {
    console.error(error);
  }
})

ipcMain.on("transTextEvent", (event, channel, data) => {
  try {
    transText(getWebContentsId(channel), channel, data)
  } catch (error) {
    console.error(error);
  }
});

// 创建窗口
const createWindow = (url) => {
  const win = new BrowserWindow({
    width: 600,
    height: 400,
    webPreferences: {
      nodeIntegration: true, // 允许在渲染进程（在窗口）里面使用 node.js
      contextIsolation: false, // 关闭上下文隔离
      webviewTag: true, // 允许使用 <webview> 标签
    },
  });

  win.loadFile(url);
  return win;
};

app.whenReady().then(() => {
  const url1 = path.join(__dirname, "win01/win01.html")
  const url2 =  path.join(__dirname, "win02/win02.html")
  

  winRef.push(createWindow(url1));
  winRef.push(createWindow(url2));
});
```

`win01.js`
```js
const { ipcRenderer } = require('electron');

const $span = document.querySelector('#content');

ipcRenderer.on('action', (event, data) => {
  // 这里接收主线程传递过来的数据
  $span.innerHTML = data;
})

// 接下来要将监听的 action 事件注册到主进程中
ipcRenderer.send('registerChannelEvent', 'action')
```

## 五、使用消息端口进行通信
![[Pasted image 20250515001953.png]]
## 六、窗口基础知识

几乎所有包含图形界面的操作系统都是以窗口为基础构建各自的用户界面的。系统内小到一个计算器，大到一个复杂的业务系统，都是基于窗口而创建的。如果开发人员要开发一个有良好用户体验的 *GUI* 应用，势必会在窗口的控制上下足功夫。

Electron 中的窗口由 BrowserWindow 对象来创建，可以配置的属性多达几十个，这里我们将介绍一些比较常用的属性，以及一些比较常见的需求。

主要包含以下内容：
- 窗口相关配置
- 组合窗口
- 窗口的层级


### 窗口相关配置

这一块儿基本上都是传递给 BrowserWindow 的配置项。

**基础属性**

- maxWidth：设置窗口的最大宽度
- minWidth：设置窗口的最小宽度
- maxHeight：设置窗口的最大高度
- minHeight：设置窗口的最小高度
- resizeable：是否可以改变大小，当设置 resizeable 为 false 之后，代表不可缩放，前面所设置的 maxWidth ... 这些就没有意义了
- moveable：是否可以移动



**窗口位置**

默认窗口出现在屏幕的位置是在正中间，但是我们可以通过 x、y 属性来控制窗口出现在屏幕的位置

- x：控制窗口在屏幕的横向坐标
- y：控制窗口在屏幕的纵向坐标



**标题栏文本和图标**

关于窗口的标题栏，实际上是可以在多个地方设置的。

既然可以在多个地方进行设置，那么这里自然会涉及到一个优先级的问题。优先级从高到低依次：

- HTML 文档的 title
- BrowserWindow 里面的 title 属性
- package. json 里面的 name
- Electron 默认值：Electron

除了标题栏文本，我们还可以设置对应的图标：

- icon：设置标题栏的图标，一般来讲是 ico 格式

```js
// 创建窗口方法
const createWindow = () => {
  const win = new BrowserWindow({
    // ...
    icon: path.join(__dirname, "logo.ico")
  });

  win.loadFile("window/index.html");
};
```



**标题栏、菜单栏和边框**

默认我们所创建的窗口，是有标题栏、菜单栏以及边框的，不过这个也是能够控制的。通过 frame 配置项来决定是否要显示。

- frame：true/false 默认值是 true


### 组合窗口

桌面应用有些时候是有多个窗口的，多个窗口彼此之间是相互独立，也就是说，假设我关闭了一个窗口，对另外一个窗口是没有影响的。

但是在有一些场景中，多个窗口之间存在一定程度的联动，例如两个窗口存在父窗口和子窗口之间的关系，父窗口关闭之后，子窗口也一并被关闭掉了。

在 Electron 中，类似这样的需求可以非常简单的被实现，Electron 提供了父子窗口的概念，通过 parent 来指定一个窗口的父窗口。

当窗口之间形成了父子关系之后，两个窗口在行为上就会有一定的联系：

- 子窗口可以相对于父窗口的位置来定位
- 父窗口在移动的时候，子窗口也跟着移动
- 父窗口关闭了，子窗口也应该一并被关闭掉
- .....



### 窗口的层级

当我们创建多个窗口的时候，默认情况下最后面创建的窗口，就在越上层。但是如果两个窗口是独立的话，那么当用户点击对应的窗口的时候，被点击的窗口会处于最上层。

但是在某些场景下，我们就是需要置顶某一些窗口，有两种方式可以办到：

- alwaysOnTop：true/false
  - 该配置属性虽然也能够置顶窗口，但是没有办法进行更新细粒度的设置
- window.setAlwaysOnTop (flag, level, relativeLevel)：该方法可以进行一个更细粒度的控制
  - flag：一个布尔值，用于设置窗口是否始终位于顶部。如果为 true，窗口将始终保持在最前面；如果为 false，则取消这一设置
  - level（可选）：一个字符串，指定窗口相对于其他窗口的层次。常用的值包括 'normal', 'floating', 'torn-off-menu', 'modal-panel', 'main-menu', 'status', 'pop-up-menu', 'screen-saver' 等。这个参数在不同的操作系统上可能会有不同的行为。
  - relativeLevel（可选）：一个整数，用于在设置了 level 的情况下进一步微调窗口层次。

#### 制作桌面时钟挂件
- 步骤
	1. 创建 `electron` 基础模板
	2. 先编写渲染进程，相关页面逻辑
		1. 基础页面骨架
		2. css 样式
		3. js 逻辑（定时器读秒，时分秒旋转，`clock` 时钟可拖拽）
	3. 窗口参数设置（`窗口透明`、`窗口不可缩放`、`隐藏窗口边框`、`设置窗口置顶`）

##### 1. 创建 `electron` 基础模板
![[Pasted image 20250515223005.png]]
`main.js`
```js
const { app, BrowserWindow } = require("electron")
const path = require("path")

const createWindow = () => {
  const win = new BrowserWindow({
    width: 325,
    height: 330,
    webPreferences: {
      nodeIntegration: true,
      contextIsolation: false
    }
  })
  win.loadFile('window/index.html')
}

// whenReady 是一个生命周期事件，当 Electron 完成了初始化并准备创建浏览器窗口时调用
app.whenReady().then(() => {
  createWindow()
})
```

##### 2. 编写渲染进程，相关页面逻辑
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'" />
  <title>09. 制作桌面时钟挂件</title>
  <link rel="stylesheet" href="./index.css">
</head>

<body>
  <div id="clock" class="clock">
    <div class="hour"></div>
    <div class="min"></div>
    <div class="sec"></div>
  </div>
  <script src="./index.js"></script>
</body>

</html>
```
```css
* {
  margin: 0;
  padding: 0;
}

/* body 设置为弹性盒子，可以让时钟容器居中 */
body {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100vw;
  height: 100vh;
  -webkit-user-select: none;
  /* Safari */
  -moz-user-select: none;
  /* Firefox */
  -ms-user-select: none;
  /* IE10+ */
  user-select: none;
  /* 标准语法 */
  background-color: transparent;
}

/* 时钟容器 */
.clock {
  width: 260px;
  height: 260px;
  display: flex;
  justify-content: center;
  align-items: center;
  /* 这里设置为弹性盒，是为了让所有的盒子居中显示
      如果去除这里的弹性盒，所有时分秒盒子定位将定在该容器的左上角 */
  background: url("../window/assets/clock.png") no-repeat center/cover;
  background-color: #091921;
  border-radius: 50%;
  border: 3px solid #091921;
  box-shadow: 0 -15px 15px rgba(255, 255, 255, 0.05),
    inset 0 -15px 15px rgba(255, 255, 255, 0.05),
    0 15px 15px rgba(0, 0, 0, 0.05), inset 0 15px 15px rgba(0, 0, 0, 0.05);
  cursor: pointer;
}

/* 制作时钟中间的小圆点 */
.clock::before {
  content: "";
  width: 15px;
  height: 15px;
  position: absolute;
  background: #fff;
  border-radius: 50%;
  z-index: 999;
}

/* 时针、分针、秒针容器盒子的公共样式 */
.hour,
.min,
.sec {
  position: absolute;
  display: flex;
  justify-content: center;
  /* 设置为弹性盒子，是为了让伪元素制作的时针、分针、秒针居中显示 */
}

/* 时针的容器 */
.hour {
  width: 160px;
  height: 160px;
  /* outline: 1px solid red; */
}

/* 时针，利用伪元素来进行制作 */
.hour::before {
  content: "";
  position: absolute;
  width: 8px;
  height: 80px;
  background: #ff105e;
  z-index: 10;
  /* 时针的 z-index 应该设置为最低 */
  border-radius: 6px 6px 0 0;
}

/* 分针的容器 */
.min {
  width: 190px;
  height: 190px;
  /* outline: 1px solid blue; */
}

/* 分针，利用伪元素来进行制作 */
.min::before {
  content: "";
  position: absolute;
  width: 4px;
  height: 90px;
  background: #fff;
  z-index: 11;
  /* 分针的 z-index 应该比时针高，但是比秒针低 */
  border-radius: 6px 6px 0 0;
}

/* 秒针的容器 */
.sec {
  width: 230px;
  height: 230px;
  /* outline: 1px solid yellow; */
}

/* 秒针，利用伪元素来进行制作 */
.sec::before {
  content: "";
  position: absolute;
  width: 2px;
  height: 150px;
  background: #fff;
  z-index: 12;
  /* 秒针的 z-index 应该最高 */
  border-radius: 6px 6px 0 0;
}
```
```js
const deg = 6; // 角度值
// 获取 DOM 元素
const clock = document.querySelector("#clock");
const hour = document.querySelector(".hour");
const min = document.querySelector(".min");
const sec = document.querySelector(".sec");

setTime()

setInterval(() => setTime(), 1000)

function setTime() {
  // 获取当前时间
  let day = new Date();
  // 获取时、分、秒
  let hh = day.getHours() * 30; // 一圈360° 12小时 = 360°
  let mm = day.getMinutes() * deg; // 一圈360° 60分钟 = 360°，所以每分钟旋转6°
  let ss = day.getSeconds() * deg; // 一圈360° 60秒 = 360°，所以每秒旋转6°
  // 设置时、分、秒的旋转角度
  hour.style.transform = `rotateZ(${hh + mm / 12}deg)`;
  min.style.transform = `rotateZ(${mm}deg)`;
  sec.style.transform = `rotateZ(${ss}deg)`;
}

let offset = null; // 记录用户拖拽的偏移值
// 解决时钟挂件拖拽移动的问题
document.addEventListener("mousedown", (e) => {
  // 如果用户点击的是 .clock 元素或者 .clock 元素的子元素
  if (e.target.matches(".clock, .clock *")) {
    window.isDragging = true; // 设置一个开关，表示用户正在拖拽
    offset = {
      x: e.screenX - window.screenX,
      y: e.screenY - window.screenY,
    };
  }
});

document.addEventListener("mousemove", (e) => {
  if (window.isDragging) {
    const { screenX, screenY } = e; // 从最新的鼠标位置获取 x 和 y
    window.moveTo(screenX - offset.x, screenY - offset.y);
  }
});

document.addEventListener("mouseup", () => {
  window.isDragging = false; // 用户停止拖拽
  offset = null; // 重置偏移值
});

```
##### 3. 窗口参数设置（`窗口不可缩放`、`设置窗口置顶`）
```js
const { app, BrowserWindow } = require("electron")
const path = require("path")

const createWindow = () => {
  const win = new BrowserWindow({
    width: 325,
    height: 330,
    // transparent: true, // 设置窗口透明（win11设置成透明之后会有一条白色标题，无法去除）
    resizable: false, // 设置窗口不可缩放
    frame: false, // 隐藏窗口边框
    icon: path.join(__dirname, "window/assets/clock.ico"),
    webPreferences: {
      nodeIntegration: true,
      contextIsolation: false
    }
  })
  win.loadFile('window/index.html')

  win.setAlwaysOnTop(true, "pop-up-menu"); // 设置窗口置顶
  // win.setIgnoreMouseEvents(true); // 设置鼠标事件可以穿透
}

// whenReady 是一个生命周期事件，当 Electron 完成了初始化并准备创建浏览器窗口时调用
app.whenReady().then(() => {
  createWindow()
})
```
## 七、多窗口管理
主要是将所有的窗口存储到 `map` 里，之后要对哪一个窗口进行引用直接从 `map` 中取出对应的窗口引用即可。
```js
// 该 map 结构存储所有的窗口引用
const winMap = new Map();
// ...
// 往 map 里面放入对应的窗口引用
winMap.set(config.name, win);
```

很多时候，我们的窗口不仅是多个，还需要对这多个窗口进行一个分组。这个时候简单的更改一下 map 的结构即可。
首先在窗口配置方面，新增一个 group 属性，表明该窗口是哪一个分组的。

```js
const win1Config = {
  name: "win1",
  width: 600,
  height: 400,
  show: true,
  group: "grounp1" // 新增一个 grounp 的属性
  file: "window/index.html",
};
```

第二步，在创建了窗口之后，从 map 里面获取对应分组的数组，这里又分为两种情况：

- 该分组名下的数组存在：直接将该窗口引入放入到该分组的数组里面
- 该分组还不存在：创建新的数组，并且将该窗口引用放入到新分组里面

核心代码如下：

```js
if (config.group) {
  // 根据你的分组名，先找到对应的窗口数组
  let groupArr = winMap.get(config.group);
  if (groupArr) {
    // 如果数组存在，直接 push 进去
    groupArr.push(win);
  } else {
    // 新创建一个数组，作为该分组的第一个窗口
    groupArr = [win];
  }
  // 接下来更新 map
  winMap.set(config.group, groupArr);
}
```

之后，在窗口进行关闭操作时，还需要将关闭的窗口实例从 map 结构中移除掉。

```js
// 接下来还需要监听窗口的关闭事件，以便在窗口关闭时将其从 map 结构中移除
win.on("close", () => {
  groupArr = winMap.get(config.group);
  // 因为当前的窗口已经关闭，所以我们需要将其从数组中移除
  groupArr = groupArr.filter((item) => item !== win);
  // 接下来更新 map
  winMap.set(config.group, groupArr);
  // 如果该分组下已经没有窗口了，我们需要将其从 map 结构中移除
  if (groupArr.length === 0) {
    winMap.delete(config.group);
  }
});
```


## 八、应用常见设置

- 快捷键
- 托盘图标
- 剪切板
- 系统通知



### 1. 快捷键

在 Electron 中，页面级别的快捷键直接使用 DOM 技术就可以实现。

例如，我们在渲染进程对应的 JS 中书写如下的代码：

```js
// 设置一个页面级别的快捷键
window.onkeydown = function (e) {
  if ((e.ctrlKey || e.metaKey) && e.key === "q") {
    // 用户按的键是 ctrl + q
    // 我们可以执行对应的快捷键操作
    console.log("您按下了 ctrl + q 键");
  }
};
```


有些时候我们还有注册全局快捷键的需求。所谓全局快捷键，指的是操作系统级别的快捷键，也就是说，即便当前的应用并非处于焦点状态，这些快捷键也能够触发相应的动作。

在 Electron 中想要注册一个全局的快捷键，可以通过 `globalShortcut` 模块来实现。

例如：
在主进程中引入 `require("./shortcut");`
```js
const { globalShortcut, app, dialog } = require("electron");

app.on("ready", () => {
  // 需要注意，在注册全局快捷键的时候，需要在 app 模块的 ready 事件触发之后
  // 使用 globalShortcut.register 方法注册之后会有一个返回值
  // 这个返回值是一个布尔值，如果为 true 则表示注册成功，否则表示注册失败
  const ret = globalShortcut.register("ctrl+e", () => {
    dialog.showMessageBox({
      message: "全局快捷键 ctrl+e 被触发了",
      buttons: ["好的"],
    });
  });

  if (!ret) {
    console.log("注册失败");
  }

  console.log(
    globalShortcut.isRegistered("ctrl+e")
      ? "全局快捷键注册成功"
      : "全局快捷键注册失败"
  );
});

// 当我们注册了全局快捷键之后，当应用程序退出的时候，也需要注销这个快捷键
app.on("will-quit", function () {
  globalShortcut.unregister("ctrl+e");
  globalShortcut.unregisterAll();
});
```

几个核心的点：

- 需要在应用 ready 之后才能注册全局快捷键
- 使用 globalShortcut. register 来进行注册
- 通过 globalShortcut. isRegistered 可以检查某个全局快捷键是否已经被注册
- 当应用退出的时候，需要注销所注册的全局快捷键，使用 globalShortcut. unregister 进行注销



### 2. 托盘图标

有些时候，我们需要将应用的图标显示在托盘上面，当应用最小化的时候，能够通过点击图标来让应用显示出来。

例如 Mac 下面：

<img src="https://xiejie-typora.oss-cn-chengdu.aliyuncs.com/2023-12-19-082635.jpg" alt="16073255924875" style="zoom:80%;" />

在 Electron 里面为我们提供了 Tray 这个模块来配置托盘图标。

例如：
`主进程`
```js
let win = null
let tray = null; 

app.whenReady().then(() => {
  // 创建窗口1
  win = createWindow(win1Config)
  createTray();
})


function createTray() {
  // 构建托盘图标的路径
  const iconPath = path.join(__dirname, "assets/tray.jpg");
  tray = new Tray(iconPath);

  // 我们的图标需要有一定的功能
  tray.on("click", function () {
    win.isVisible() ? win.hide() : win.show();
  });

  // 还可以设置托盘图标对应的菜单
  const contextMenu = Menu.buildFromTemplate([
    {
      label: "显示/隐藏",
      click: () => {
        win.isVisible() ? win.hide() : win.show();
      },
    },
    {
      label: "退出",
      click: () => {
        app.quit();
      },
    },
  ]);
  tray.setContextMenu(contextMenu);
}
```

在上面的代码中，我们就创建了一个托盘图标。几个比较核心的点：

- **图片要选择合适，特别是大小，一般在 20 x 20 左右**
- 创建 tray 实例对象，之后托盘图标就有了
- 之后就可以为托盘图标设置对应的功能
  - 点击功能
  - 菜单目录



### 2. 剪切板

这个在 Electron 里面也是提供了相应的模块，有一个 clipboard 的模块专门用于实现剪切板的相关功能。

大致的使用方式如下：

```js
const { clipboard } = require('electron');
clipboard.writeText("你好"); // 向剪切板写入文本
clipboard.writeHTML("<b>你好HTML</b>"); // 向剪切板写入 HTML
```


### 3. 系统通知

这也是一个非常常见的需求，有些时候，我们需要给用户发送系统通知。

在 Electron 中，可以让系统发送相应的应用通知，不过这个和 Electron 没有太大的关系，这是通过 HTML 5 里面的 notification API 来实现的。

例如：

```js
const notifyBtn = document.getElementById("notifyBtn");
notifyBtn.addEventListener("click", function () {
  const option = {
    title: "您有一条新的消息，请及时查看",
    body: "这是一条测试消息，技术支持来源于 HTML5 的 notificationAPI",
  };
  const myNotify = new Notification(option.title, option);
  myNotify.onclick = function () {
    console.log("用户点击了通知");
  };
});
```

核心的点有：

- 使用 HTML 5 所提供的 Notification 来创建系统通知
- new Notification 之后能够拿到一个返回值，针对该返回值可以绑定一个点击事件，该点击事件会在用户点击了通知消息后触发

## 九、系统对话框与菜单

每个桌面应用都或多或少的要与系统 *API* 打交道。比如显示系统通知、在系统托盘区显示一个图标、通过“打开文件对话框”打开系统内一个指定的文件、通过“保存文件对话框”把数据保存到系统磁盘上面等。

早期的 *Electron* 对这方面支持不足，但随着使用者越来越多，用户需求也越来越多且各不相同，*Electron* 在这方面的支持力度也越来越强。

这节课我们来看两个方面：

- 系统对话框
- 菜单



### 1. 系统对话框

在 Electron 中，可以使用一个 dialog 的模块来实现打开系统对话框的功能。

ipcRenderer. invoke 和 ipcMain. handle 可以算作是一组方法，这一组方法主要就是处理异步调用。

举一个例子，如下：

```js
// 主进程
const { ipcMain } = require("electron");

ipcMain.handle('get-data', async (event, ...args) => {
  // 这里就可以执行一些异步的操作，比如读取文件、查询数据库等
  // args 是参数列表，是从渲染进程那边传递过来的
  const data = ...; // 从一些异步操作中拿到数据
  return data;
})
```

```js
// 渲染进程
const { ipcRenderer } = require("electron");

async function fetchData(){
  try{
    const data = await ipcRenderer.invoke('get-data', /* 后面可以传递额外的参数 */);
    // 后面就可以在拿到这个 data 之后做其他的操作
  } catch(e){
    console.error(e);
  }
}

fetchData();
```



接下来我们在 handle 的异步回调函数中，用到了 BrowserWindow. getFocusedWindow 方法，该方法用于获取当前聚焦的窗口，或者换一句话说，就是获取用户当前正在交互的 Electron 窗口的引用。

如果当前没有窗口获取焦点，那么会返回 null。

**使用场景**

这个方法在需要对当前用户正与之交互的窗口执行操作时非常有用。比如：

- 在当前获得焦点的窗口中打开一个对话框。
- 调整或查询当前活跃窗口的大小、位置等属性。
- 对当前用户正在使用的窗口应用特定的逻辑或视觉效果。



### 2. 菜单



#### 自定义菜单

在使用 Electron 开发桌面应用的时候，Electron 为我们提供了默认的菜单，但是这个菜单仅仅是用于演示而已。

我们可以自定义我们应用的菜单。

在 Electron 中，想要自定义菜单，可以使用 Menu 这个模块。代码如下：

```js
// 做我们的自定义菜单
const { Menu } = require("electron");

// 定义我们的自定义菜单
const menuArr = [
  {
    label: "",
  },
  {
    label: "菜单1",
    submenu: [
      {
        label: "菜单1-1",
      },
      {
        label: "菜单1-2",
        click() {
          // 该菜单项目被点击后要执行的逻辑
          console.log("你点击了菜单1-2");
        },
      },
    ],
  },
  {
    label: "菜单2",
    submenu: [
      {
        label: "菜单2-1",
      },
      {
        label: "菜单2-2",
        click() {
          // 该菜单项目被点击后要执行的逻辑
          console.log("你点击了菜单2-2");
        },
      },
    ],
  },
  {
    label: "菜单3",
    submenu: [
      {
        label: "菜单3-1",
      },
      {
        label: "菜单3-2",
        click() {
          // 该菜单项目被点击后要执行的逻辑
          console.log("你点击了菜单3-2");
        },
      },
    ],
  },
];

// 创建菜单
const menu = Menu.buildFromTemplate(menuArr);
// 设置菜单，让我们的自定义菜单生效
Menu.setApplicationMenu(menu);
```

核心的步骤就是：

1. 自定义菜单数组
2. 创建菜单：`Menu.buildFromTemplate` 方法
3. 设置菜单：`Menu.setApplicationMenu`

现在我们遇到了一个问题：无法打开开发者工具了，这给我们调试代码带来了很大的不便，我们需要解决这个问题。之所以无法打开，是因为 Electron 默认菜单中，包含了一些基本的功能，其中就有打开开发者工具的快捷方式，但是一旦我们自定义了菜单，这些默认项目就不存在了，默认的功能也就没了。

要解决这个问题，我们只需要在菜单模板中添加一个专门用于打开开发者工具的项目，以及设置快捷键：

```js
{
  label: "开发者工具",
  submenu: [
    {
      label: "切换开发者工具",
      accelerator:
        process.platform === "darwin" ? "Alt+Command+I" : "Ctrl+Shift+I",
      click(_, focusedWindow) {
        if (focusedWindow) focusedWindow.toggleDevTools();
      },
    },
  ],
}
```

在设置菜单的时候，我们是可以为菜单设置一个 role 属性。

```js
{ label: '菜单3-1', role: "paste" }
```

role 是菜单项中一个特殊的属性，用于指定一些常见的操作和行为。例如常见的复制、粘贴、剪切等。

当你设置了 role 属性之后，Electron 会自动实现对应的功能，你就不需要在编写额外的代码。

使用 role 的好处：

- 简化开发
- 一致性
- 自动的状态管理：例如当剪贴板为空的时候，粘贴的操作会自动处于禁用状态

#### 右键菜单

这个也是一个非常常见的需求，我们需要在页面上点击鼠标右键的时候，显示右键菜单。

这个功能非常简单，只需要在对应渲染进程对应的窗口上编写 HTML、CSS 和 JS 相应的逻辑即可。

核心的 JS 代码逻辑如下：

```js
const { ipcRenderer } = require("electron");

const btn = document.getElementById("btn");

btn.addEventListener("click", async function () {
  // 我们需要弹出一个对话框
  const result = await ipcRenderer.invoke("show-open-dialog");
  console.log(result);
});

const menu = document.getElementById("menu");
// 点击右键时对应的事件
window.oncontextmenu = function (e) {
  e.preventDefault();
  menu.style.left = e.clientX + "px";
  menu.style.top = e.clientY + "px";
  menu.style.display = "block";
};

// 用户点击右键菜单上面的某一项的时候
// 注意下面的查询 DOM 的方式只会获取到第一个匹配的元素
// 因此右键菜单上面的功能只会绑定到第一个菜单项上面
document.querySelector(".menu").onclick = function () {
  console.log("这是右键菜单上面的某一个功能");
};

// 当用户点击窗口的其他地方的时候，右键菜单应该消失
window.onclick = function () {
  menu.style.display = "none";
};
```

主要就是针对 3 个方面绑定事件：

- 右键点击的时候
- 右键点击后，出现的菜单上面的项目需要绑定对应的事件
- 点击窗口其他位置的时候，右键菜单要消失
