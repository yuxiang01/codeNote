## 一、架构原理
### Chromium 架构
![[Pasted image 20240105174808.png]]
### Electron 架构
![[Pasted image 20240105174504.png]]
## 二、主进程
- 主进程
	- Electron 运行 `package.json` 的 `main` 脚本的进程被称为主进程
	- 每个应用只有一个主进程
	- 管理原生 GUI, 典型的窗口（BrowserWindow、Tray、Dock、Menu)
	- 创建渲染进程
	- 控制应用生命周期（app)
- 渲染进程
	- 展示 Web 页面的进程称为渲染进程
	- 通过 Node. js、Electron 提供的 API 可以跟系统底层打交道
	- 一个 Electron 应用可以有多个渲染进程
- `Node.js` 和 `Chromiums` 整合
	- 难点：`Node.js` 事件循环基于 `libuv`, 但 `Chromium` 基于 `messagebump`
		- `Chromium` 集成到 `Node.js`: 用 `libuv` 实现 `messagebump`
		- `Node.js` 集成到 Chromium
### 主线程生命周期
```js
app.on('will-finish-launching', () => {
  console.log('will-finish-launching');
})
app.on('ready', () => {
  console.log('ready');
})
app.on('will-quit', () => {
  console.log('will-quit');
})
app.on('window-all-closed', () => {
  console.log('window-all-closed')
  app.quit();
})
app.on('before-quit', () => {
  console.log('before-quit')
})
app.on('quit', () => {
  console.log('quit');
})
```
```shell
will-finish-launching # 当应用程序完成基础的启动的时候被触发
ready                 # 当 Electron 完成初始化时，发出一次
window-all-closed     # 当所有的窗口都被关闭时触发
before-quit           # 在程序关闭窗口前发信号
will-quit             # 当所有窗口被关闭后触发，同时应用程序将退出
quit                  # 在应用程序退出时发出
```

## 三、主渲染进程通信
> 主进程【ipcMain】-渲染进程【ipcRenderer】

主进程
```js
// 窗口需要设置为全局变量，避免垃圾回收
let win
app.on('ready', () => {
  console.log('ready');
  win = new BrowserWindow({
    width: 300,
    height: 320,
    webPreferences: {
      nodeIntegration: true,
      contextIsolation: false
    }
  })
  win.loadFile('./index.html')
  handleIPC()
})

function handleIPC() {
  ipcMain.handle('work-notification', async function () {
    return await new Promise((res, reject) => {
      dialog.showMessageBox({
        icon: path.join(__dirname, 'tomato.png'),
        type: 'info',
        title: '任务结束',
        message: '是否开始休息',
        buttons: ['开始休息', '继续工作']
      }).then((it) => res(it.response))
    })
  })
}
```
渲染进程
```js
async function notification() {
  let res = await ipcRenderer.invoke('work-notification')
  if (res == 0) {
    console.log('休息一下')
  } else if (res == 1) {
    startWork()
  }
}

```