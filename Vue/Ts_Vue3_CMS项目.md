>[!tip]- 基于 Typescript + Vue3 后台管理项目
> - 听了写了不代表真的会, 只有及时复盘、总结、吸收, 才是会
# 一、项目搭建、环境配置
## 1.1 构建项目
```shell
npm init vite@latest
## 或者
npm init vue@latest
```
![[Pasted image 20230320105227.png]]
## 1.2 初始化 css 样式
- 安装 `npm i normalize.css`
- 创建 `src/assets/css/reset.css` 重置自带样式
	```css
	/* margin/padding重置 */
	body, h1, h2, h3, h4, ul, li {
	  padding: 0;
	  margin: 0;
	}
	
	html {
	  line-height: 1.2;
	}
	
	ul, li {
	  list-style: none;
	}
	
	a {
	  text-decoration: none;
	  color: #333;
	}
	
	a, button, input {
	  -webkit-tap-highlight-color: rgba(255, 0, 0, 0);
	}
	
	img {
	  vertical-align: top;
	}
	
	/* 对斜体元素重置 */
	i, em {
	  font-style: normal;
	}
	```
- 创建 `src/assets/css/common.css` 通用样式
```css
.flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

.flex-beside {
  display: flex;
  justify-content: space-between;
}
```
- 创建 `src/assets/css/index.css` 统一样式入口
```css
@import './reset.css';
@import './common.css';
```
- 导入 `main.ts`
```ts
import 'normalize.css'
import '@/assets/css/index.css'
```
## 1.3 配置路由
- 安装 `npm i vue-router`
- 创建 `src/router/index.ts` 路由文件
	```ts
	import { createRouter, createWebHashHistory } from 'vue-router'
	import type { RouteRecordRaw } from 'vue-router'
	
	const routes: Array<RouteRecordRaw> = [
	  {
	    path: '/',
	    name: '',
	    component: () => import('@/layout/index.vue')
	  }
	]
	
	export const router = createRouter({
	  history: createWebHashHistory(),
	  routes
	})
	```
- 导入 `main.ts`
	```ts
	import { router } from './router'
	createApp(App).use(router).mount('#app')
	```
## 1.4 配置别名 `@`
- 安装 node 环境
	```shell
	npm install @types/node -D
	```
- `vite.config.ts`
	```ts
	import { resolve } from 'path'
	
	resolve: {
		alias: { '@': resolve(__dirname, './src') }
	}
	```
- `tsconfig.json`
	```json
	{
	  "compilerOptions": {
	    "baseUrl": "./",
	    "paths": {
	      "@/*": ["src/*"]
	    }
	  }  
	}
	```
## 1.5 代码规范
- 集成 editorconfig 配置
	- EditorConfig 有助于为不同 IDE 编辑器上处理同一项目的多个开发人员维护一致的编码风格
	- 新建 `.editorconfig` 文件
		```yaml
		# http://editorconfig.org
		
		root = true
		
		[*] # 表示所有文件适用
		charset = utf-8 # 设置文件字符集为 utf-8
		indent_style = space # 缩进风格（tab | space）
		indent_size = 2 # 缩进大小
		end_of_line = lf # 控制换行类型(lf | cr | crlf)
		trim_trailing_whitespace = true # 去除行尾的任意空白字符
		insert_final_newline = true # 始终在文件末尾插入一个新行
		
		[*.md] # 表示仅 md 文件适用以下规则
		max_line_length = off
		trim_trailing_whitespace = false
		```
	- VSCode 需要安装一个插件：EditorConfig for VS Code
- 使用 prettier 工具
	- Prettier 是一款强大的代码格式化工具
	- 配置 Prettierrc 文件： 
		* useTabs：使用 tab 缩进还是空格缩进，选择 false；
		* tabWidth：tab 是空格的情况下，是几个空格，选择 2 个；
		* printWidth：当行字符的长度，推荐 80，也有人喜欢 100 或者 120；
		* singleQuote：使用单引号还是双引号，选择 true，使用单引号；
		* trailingComma：在多行输入的尾逗号是否添加，设置为 `none`，比如对象类型的最后一个属性后面是否加一个 `，` 
		* semi：语句末尾是否要加分号，默认值 true，选择 false 表示不加 `；` 
	- 新建 `.prettierrc` 文件
		```json
		{
		  "useTabs": false,
		  "tabWidth": 2,
		  "printWidth": 80,
		  "singleQuote": true,
		  "trailingComma": "none",
		  "semi": false
		}
		```
	- 创建 `.prettierignore` 忽略文件 
		 ```
		/dist/*
		.local
		.output.js
		/node_modules/**
		
		**/*.svg
		**/*.sh
		
		/public/*
		```
	- VSCode 需要安装 prettier 的插件
- 使用 ESLint 检测
	- VSCode 需要安装 ESLint 插件
	- 解决 eslint 和 prettier 冲突的问题 
		- 添加 prettier 插件：
			```json
			extends: [
				"plugin:vue/vue3-essential",
				"eslint:recommended",
				"@vue/typescript/recommended",
				"@vue/prettier",
				"@vue/prettier/@typescript-eslint",
				'plugin:prettier/recommended'
			]
			```
		- VSCode 中 eslint 的配置
			```json
		 "eslint.lintTask.enable": true,
		 "eslint.alwaysShowStatus": true,
		 "eslint.validate": [
		   "javascript",
		   "javascriptreact",
		   "typescript",
		   "typescriptreact"
		 ],
		 "editor.codeActionsOnSave": {
		   "source.fixAll.eslint": true
		 },
			```
			
## 1.6 区分 dev 和 pro 环境
- Vite 的环境变量：
	```ts
	import.meta.env.MODE: {string} // 应用运行的模式。 
	import.meta.env.PROD: {boolean} // 应用是否运行在生产环境。 
	import.meta.env.DEV: {boolean} // 应用是否运行在开发环境 (永远与import.meta.env.PROD相反)。 
	import.meta.env.SSR: {boolean} // 应用是否运行在 server 上。
	```
- Vite 使用 dotenv 从你的环境目录中的下列文件加载额外的环境变量：
		- 只有以 VITE_ 为前缀的变量才会暴露给经过 vite 处理的代码
			![[Pasted image 20230320152648.png]]
## 1.7 整合 Element-plus
1. 安装 `npm install element-plus --save`
2. 安装插件, 自动导入
	- 安装插件 `npm install -D unplugin-vue-components unplugin-auto-import`
	- `vite.config.ts` 配置插件
		```ts
		import AutoImport from 'unplugin-auto-import/vite'
		import Components from 'unplugin-vue-components/vite'
		import { ElementPlusResolver } from 'unplugin-vue-components/resolvers'
		
		export default defineConfig({
		  plugins: [
			vue(),
		    AutoImport({ resolvers: [ElementPlusResolver()] }),
		    Components({ resolvers: [ElementPlusResolver()] })
		  ]
		})
		```
3. 解决 `el-message等` 样式导入问题
	- [安装插件](https://github.com/vbenjs/vite-plugin-style-import): `npm i -D vite-plugin-style-import`
	- 插件捆绑安装 `npm install consola -D`
	- 配置 `vite.config.ts`
		```ts
		import {
		  createStyleImportPlugin,
		  ElementPlusResolve
		} from 'vite-plugin-style-import'
		
		export default defineConfig({
		  plugins: [
		      createStyleImportPlugin({
			      resolves: [ElementPlusResolve()],
			      libs: [
			        {
			          libraryName: 'element-plus',
			          esModule: true,
			          resolveStyle: (name: string) => {
			            return `element-plus/theme-chalk/${name}.css`
			          }
			        }
			      ]
		    })
		  ]
		})
		```
4. `El-Icon` 图标
	- 安装 `npm install @element-plus/icons-vue`
	- 注册所有图标
		- `src/global/register-icon.ts`
			```ts
			import type { App } from 'vue'
			import * as ElementPlusIconsVue from '@element-plus/icons-vue'
			
			function registerIcons(app: App<Element>) {
			  for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
			    app.component(key, component)
			  }
			}
			
			export default registerIcons
			```
		- `src/main.ts` 
			```ts
			import registerIcons from './global/register-icon'
			createApp(App).use(registerIcons).mount('#app')
			```
## 1.8 封装 axios 
0. `src/service/config/index.ts` 
	- `url` 资源路径统一管理
		```ts
		export const BASE_URL = "http://111.230.245.205:8880"
		export const MAP_URL =  'https://restapi.amap.com'
		export const TIME_OUT = 5000
		```
1. `src/service/request/index.ts`
	- 类型约束 `src/service/request/type.ts` 
		```ts
		import type {
		  AxiosRequestConfig,
		  AxiosResponse,
		  InternalAxiosRequestConfig
		} from 'axios'
		
		export interface Interceptors<T = AxiosResponse> {
		  requestSuccessFn?: (
		    config: InternalAxiosRequestConfig
		  ) => InternalAxiosRequestConfig
		  requestFailureFn?: (err: any) => any
		  responseSuccessFn?: (res: T) => T
		  responseFailureFn?: (err: any) => any
		}
		
		export interface RequestConfig<T = AxiosResponse> extends AxiosRequestConfig {
		  interceptors?: Interceptors<T>
		}
		```
	- 封装 `axios` 成 `Request` 类
		```ts
		import axios from 'axios'
		import type { AxiosInstance } from 'axios'
		import { RequestConfig } from './type'
		
		class Request {
		  instance: AxiosInstance
		  constructor(config: RequestConfig) {
		    this.instance = axios.create(config)
		    this.instance.interceptors.request.use(
		      (config) => {
		        // TODO: add Token/Loading...
		        return config
		      },
		      (err) => err
		    )
		    this.instance.interceptors.request.use(
		      config.interceptors?.requestSuccessFn,
		      config.interceptors?.requestFailureFn
		    )
		    this.instance.interceptors.response.use(
		      config.interceptors?.responseSuccessFn,
		      config.interceptors?.responseFailureFn
		    )
		    this.instance.interceptors.response.use(
		      (res) => {
		        // TODO: remove Loading...
		        return res.data
		      },
		      (err) => {
		        // TODO: remove Loading...
		        return err
		      }
		    )
		  }
		  request<T = any>(config: RequestConfig<T>) {
		    if (config.interceptors?.requestSuccessFn) {
		      config = config.interceptors.requestSuccessFn(config as any)
		    }
		    return new Promise<T>((resolve, reject) => {
		      this.instance
		        .request<any, T>(config)
		        .then((res) => {
		          if (config.interceptors?.responseSuccessFn) {
		            res = config.interceptors.responseSuccessFn(res)
		          }
		          resolve(res)
		        })
		        .catch((err) => reject(err))
		    })
		  }
		  get<T = any>(config: RequestConfig<T>) {
		    return this.request<T>({ ...config, method: 'get' })
		  }
		  post<T = any>(config: RequestConfig<T>) {
		    return this.request<T>({ ...config, method: 'post' })
		  }
		}
		
		export default Request
		```
2. `src/service/index.ts`
	- `Request` 实例
		```ts
		import { BASE_URL, MAP_URL, TIME_OUT } from './config'
		import Request from './request'
		
		export const baseRequest = new Request({ 
			baseURL: BASE_URL, 
			timeout: TIME_OUT 
		})
		
		export const mapRequest = new Request({ 
			baseURL: MAP_URL, 
			timeout: TIME_OUT 
		})
		```

# 二、用户登录
## 2.1 界面搭建
- 使用到的 UI 组件: `el-tabs`、`el-form`
- 难点: `el-tab-pane` 组件中使用 `slot`
	```html
	<el-tab-pane name="account">
	  <!--具名插槽-->
	  <template #label>
	    <div class="label flex-center">
	      <el-icon>
	        <UserFilled />
	      </el-icon>
	      <span class="text">账号登录</span>
	    </div>
	  </template>
	  <AccountPanel ref="accountRef" />
	</el-tab-pane>
	```
## 2.2 表单输入校验
![[Pasted image 20230323185259.png]]
```html
<script setup lang="ts">
const form = reactive<Account>({
  name: '',
  password: ''
})

const accountRules: FormRules = {
  name: [
    { required: true, message: '请填写用户信息', trigger: 'blur' },
    { min: 5, message: '用户名长度最少为5', trigger: 'change' }
  ],
  password: [
    { required: true, message: '请填写用户信息', trigger: 'blur' },
    { min: 5, message: '密码长度最少为5', trigger: 'change' }
  ]
}
</script>

<template>
  <el-form
    :model="form"
    :rules="accountRules"
    label-width="60px"
    status-icon
    ref="formRef"
  >
    <el-form-item label="帐号" prop="name">
      <el-input v-model="form.name" />
    </el-form-item>
    <el-form-item label="密码" prop="password">
      <el-input show-password v-model="form.password" />
    </el-form-item>
  </el-form>
</template>
```
## 2.3 点击按钮、立即登录
父组件发生点击事件, 执行子组件函数
```html
<script setup lang="ts">
import { ref } from 'vue'

import AccountPanel from './AccountPanel.vue'

const activeName = ref('account')

// ref 获取组件实例 (先typeof推断组件类型, 再InstanceType获取返回类型)
const accountRef = ref<InstanceType<typeof AccountPanel>>()
// 处理登录按钮点击事件
const handleLoginEvent = () => {
  if (activeName.value === 'account') {
   // 父组件执行子组件方法
    accountRef.value?.loginAction()
  } else {
    console.log('用户使用手机号码登录')
  }
}
</script>
```

## 2.4 表单提交校验
![[Pasted image 20230323192319.png]]
```ts
// 获取组件实例, 用于调用组件内部方法(如: validate)
// (先typeof推断组件类型, 再InstanceType获取返回类型)
const formRef = ref<InstanceType<typeof ElForm>>()
const loginAction = () => {
  formRef.value?.validate((valid) => {
    if (valid) {
      loginStore.handleLoginAccount(form)
    } else {
      ElMessage({ message: '验证失败', type: 'error' })
    }
  })
}
// 子组件暴露内部成员给父组件
defineExpose({ loginAction })
```

## 2.5 本地存储 token 
 `src/utils/cache.ts`
 
`封装Cache工具类` 实现对 `localStorage`、`SessionStorage` 存储的 `存、取、删、清空` 方法
```ts
enum CacheType { Local, Session }

class CaChe {
  storage: Storage

  constructor(type: CacheType) {
    this.storage = (type === CacheType.Local) ? localStorage : sessionStorage
  }

  setCache(key: string, value: any) {
    if (value) {
      this.storage.setItem(key, JSON.stringify(value))
    }
  }

  getCache(key: string) {
    const value = this.storage.getItem(key)
    if (value) return JSON.parse(value)
  }

  removeCache(key: string) {
    this.storage.removeItem(key)
  }

  clear() {
    this.storage.clear()
  }
}

const localCache = new Cache(CacheType.Local)
const sessionCache = new Cache(CacheType.Session)

export { localCache, sessionCache }
```

## 2.6 路由守卫
```ts
router.beforeEach((to) => {
  // 只有登录成功(token存在),才能真正进入到main页面
  const token = localCache.getCache('token')
  if (to.path.startsWith('/main') && !token) {
    return '/login'
  }
})
```

# 三、首页搭建
## 3.1 页面搭建
整体的布局 Container
![[Pasted image 20230324235732.png]]

## 3.2 侧边栏-菜单
- 菜单 => 根据角色 id 获取菜单信息
- RBAC: role based access control
	- 基于角色访问控制 (权限管理)
- 根据 userMenus 动态遍历菜单项
	```html
	<el-menu unique-opened default-active="62" :collapse="isFold">
	  <!-- 遍历菜单group -->
	  <template v-for="item in userMenu" :key="item.id">
	    <el-sub-menu :index="item.id + ''">
	      <template #title>
	        <el-icon>
	          <component :is="item.icon.split('-icon-')[1]" />
	        </el-icon>
	        <span>{{ item.name }}</span>
	      </template>
	      <!-- 遍历子路由 -->
	      <template v-for="subitem in item.children" :key="subitem.id">
	        <el-menu-item
	          :index="subitem.id + ''"
	          @click="handleMenuItemClick(subitem)"
	        >	
	          {{ subitem.name }}
	        </el-menu-item>
	      </template>
	    </el-sub-menu>
	  </template>
	</el-menu>
	```

### 1. 动态组件: 解决动态图标问题
![[Pasted image 20230325000540.png]]
```html
<el-icon>
  <component :is="item.icon.split('-icon-')[1]" />
</el-icon>
```

### 2. 隐藏菜单滚动条
隐藏 `Aside` 区域 `Menu` 菜单的滚动条
```less
.layout-content {
  display: flex;
  height: 100%;
  &-left {
    border-right: 1px solid #ccc;
    overflow-x: hidden;
    overflow-y: auto;
    text-align: left;
    transition: width 0.3s linear;
    scrollbar-width: none;
    -ms-overflow-style: none;

    &::-webkit-scrollbar {
      display: none;
    }
  }
}
```

### 3. 问题: Element-plus 菜单动画卡顿
1. 取消 `<el-menu>` 自带动画、添加自定义动画折叠
	```html
	<template>
	  <el-menu :collapse-transition="false"></el-menu>
	</template>
	
	<style scoped lang="less">
	   .el-menu {
	     border-right: none;
	     transition: width 0.15s;
	     -webkit-transition: width 0.15s;
	     -moz-transition: width 0.15s;
	     -webkit-transition: width 0.15s;
	     -o-transition: width 0.15s;
	   }
	</style>
	```
2. 添加折叠菜单折叠动画
	```css
	.layout-content-left {
	  transition: width 0.3s linear;
	}
	```

### 4. 问题: flex 布局子元素宽度超出父元素
[[Ts_Vue3_CMS项目#2. 隐藏菜单滚动条|样式:flex布局(左定宽、右flex:1)]]
![[Pasted image 20230329175224.png]]
右侧内容过大、占用面过广、致使右侧 `flex: 1` 自动占宽失效

>[!summary]- [问题分析](https://juejin.cn/post/6974356682574921765#heading-0)
>- `flex：1` 并不是决定子元素宽度的因素
>	- 它只是规定了: 
>		- 如果父元素有多余空间，以怎样的比例去分配剩余空间
>		- 并不会对子元素原本就占据的空间做处理
>	- 所以，当元素原本的宽度就超过父元素宽度时，子元素内容就会超出

- 解决方案
	- 限制子元素原本宽度，右侧区域设置 `width` 属性  
		- `width: 0` 原理：强行设置. 
			- right 原本宽度为0
			- 让右侧盒模型宽度完全由 `flex: 1` 这个属性来分配

## 3.3 头部-折叠控制菜单
![[复杂组件通信-侧边栏折叠控制.excalidraw|806]]
[[Pasted image 20230325003716.png|复杂组件通信-侧边栏折叠控制.png]]

## 3.4 头部-面包屑组件
### 1. 界面搭建
```html
<template>
  <el-breadcrumb separator-icon="ArrowRight">
    <template v-for="item in breadcrumb" :key="item.name">
      <el-breadcrumb-item :to="item.path">
        {{ item.name }}
      </el-breadcrumb-item>
    </template>
  </el-breadcrumb>
</template>
```
### 2. 定义获取面包屑数据方法
遍历菜单拿到父子数据
```ts
export const mapPathTobreadCrumb = (path: string, menus: Menus[]) => {
  // 定义面包屑对象
  const breadcrumbs: { name: string; path: string }[] = []
  for (const menu of menus) {
    for (const submenu of menu.children) {
      if (submenu.url === path) {
        breadcrumbs.push({ name: menu.name, path: menu.url })
        breadcrumbs.push({ name: submenu.name, path: submenu.url })
      }
    }
  }
  return breadcrumbs
}
```

### 3. 监听路由变化、返回数组
监听 route 路由的路径的变化
```html
<script setup lang="ts">
...
const breadcrumb = computed(() => {
  return mapPathTobreadCrumb(route.path, userMenu)
})
</script>
```


## 3.5 动态路由
> 根据用户的权限信息, 动态添加路由 (而不是一次性注册所有路由)

### 1. 文件结构 
![[Pasted image 20230325162523.png]]
![[Pasted image 20230325163552.png]]

### 2. 获取所有路由对象
`路由文件` => `allRoutes[]`
* 动态获取所有的路由对象, 放到数组中
    * 路由对象都在独立的文件中
    * 从文件中将所有路由对象先读取数组中  

`文件` => `数组`
```ts
// 1.1.读取router/main所有的ts文件
const files: Record<string, any> = import.meta.glob(
  '../router/main/**/*.ts',
  { eager: true }
)
```
![[Pasted image 20230325165334.png]]
遍历 `files` 数组 => `Module` / `Object` 
```ts
for (const key in files) {
  console.log(files[key])
}
```
![[Pasted image 20230325171408.png]]
将 `router` 对象 `push` 至数组中
```ts
// 1.2 遍历文件列表,将加载的对象放到allRoutes数组中
for (const key in files) {
  const module = files[key]
  allRouter.push(module.default)
}
console.log(allRouter)
```
![[Pasted image 20230325171753.png]]

完整代码 `src/utils/map-menu.ts`
```ts
/*
  动态获取所有的路由对象, 放到数组中
    * 路由对象都在独立的文件中
    * 从文件中将所有路由对象先读取数组中
*/
const allRoutesList = () => {
  const allRouter: RouteRecordRaw[] = []

  // 1.1 读取router/main所有的ts文件
  const files: Record<string, any> = import.meta.glob(
    '../router/main/**/*.ts',
    { eager: true }
  )
  
  // 1.2 遍历文件列表,将加载的对象放到allRoutes
  for (const key in files) {
    const module = files[key]
    allRouter.push(module.default)
  }
  
  return allRouter
}
```

### 3. 将菜单映射至路由
记录默认路由/默认菜单选中
```ts
// 定义全局变量
export let firstMenu = {}
```

根据菜单动态映射路由、并注册路由
```ts
export const mapMenusToRoutes = (menus: Menus[]) => {
  // 获取所有路由数组
  const allRoutes = allRoutesList()
  // 根据路由数据、添加并注册路由
  const routes: RouteRecordRow[] = []
  for (const menu of menus) {
    for (const submenu of menu.children) {
      const route = allRoutes.find(item => item.path === submenu.url)
      // 一级路由 重定向至 首个二级路由
      if (route) {
        if (!routes.forEach((item) => item.path === menu.url)) {
          routes.push({ path: item.path, redirect: route.path})
        }
      }
      routes.push(route)

	  if (Object.keys(firstMenu).length == 0 && route) firstMenu = submenu
    }
  }
  return routes
}
```


### 4. 根据路由匹配菜单
根据菜单权限 `userMenu` 匹配 `Router`
- `Es6+` 语法: `for...in` 和 `for...of` 区别? 
	- `for...in` 仅迭代自身的属性, 是为遍历对象属性而构建的 
		- 不建议与数组一起使用, 数组可以使用 `foreach()`、`for...of`
		- 常用于调试，可以更方便的去检查对象属性
	- `for...of` 类似于迭代器
		- `for (variable of iterable) {}`
			- `variable` 当前对象
			- `iterable` 被迭代枚举其属性的对象

工具函数 `src/utils/map-menu.ts`
```ts
/**
 * 根据url去高亮/选中菜单项
 * @param path 当前路径
 * @param menus 所有的菜单
 */
export const mapPathToMenus = (path: string, menus: Menus[]) => {
  for (const menu of menus) {
    for (const submenu of menu.children) {
	  if (submenu.url === path) return menu
    }
  }
}
```
使用: `src/layout/Menu.vue`
```html
<script setup lang="ts">
...
// 默认菜单选中
const route = useRoute()
// computed计算属性监听路由变化
// 一旦路由变化,就会重新计算
const defaultActive = computed(() => {
  const pathMenu = mapPathToMenus(route.path, userMenu)
  return `${pathMenu?.id}`
})
</script>

<template>
...
  <el-menu
    :collapse-transition="false"
    unique-opened
    :default-active="defaultActive"
    :collapse="isFold"
  >
  </el-menu>
</template>
```



## 3.6 页面刷新保持路由注册状态
在 `createApp` 挂载之前, 调 `pina` 的 `action` 方法读取本地缓存
```ts
// 防止页面刷新数据丢失
loadLocalCache() {
  const token = localCache.getCache('token')
  const userInfo = localCache.getCache('userInfo')
  const menus = localCache.getCache('menus')
  if (token && userInfo && menus) {
    this.token = token
    this.userInfo = userInfo
    this.menus = menus
    // 请求所有roles/departments数据
    const mainStore = useMainStore()
    mainStore.fetchEntireDataAction()
    // 动态添加路由
    const routes = mapMenusToRoutes(menus)
    routes.forEach((route) => router.addRoute('main', route))
  }
}
```
`src/store/index.ts`
```ts
const pinia = createPinia()
const registerStore = (app: App<Element>) => {
  app.use(pinia)

  const userStore = useUserStore()
  userStore.loadLocalCache()
}

export default registerStore
```
`src/main.ts` 入口文件
```ts
import pinia from './stores'

createApp(App).use(icons).use(pinia).use(router).mount('#app')
```


## 3.7 Element-plus 国际化设置方案
`App.vue` 组件入口文件, 导入 `zHCn` 中文配置, `ConfigProvider` 组件包裹 `RouterView` 实现全局配置
```html
<script setup lang="ts">
import zhCn from 'element-plus/dist/locale/zh-cn.mjs'
</script>

<template>
  <el-config-provider :locale="zhCn">
    <RouterView></RouterView>
  </el-config-provider>
</template>
```

# 四、组件重构
## 4.1 抽离通用组件
后台页面/功能大致相同: `条件搜索(搜索区域)`、`表格内容(内容区域)`、`对话框(改、增)`
![[Pasted image 20230329203703.png]]
抽离高级组件、配置生成代码、**要求**后端接口必须和页面保持一致

先常规 => 再通用 => 高级组件抽离
`src/page-search/pageSearch.vue`
```html
<template>
  <div class="page-search">
    <el-form :model="searchForm" ref="formRef" label-width="80px" size="large">
      <el-row :gutter="20">
       ...
      </el-row>
    </el-form>
    <div class="btns">
      <el-button icon="Refresh" @click="handelResetClick">重置</el-button>
      <el-button icon="Search" type="primary" @click="handelQueryClick">
        查询
      </el-button>
    </div>
  </div>
</template>
```

```html
<template v-for="item in searchConfig.formItems" :key="item.prop">
  <el-col :span="8">
    <el-form-item :label="item.label" :prop="item.prop">
      <template v-if="item.type === 'input'">
        <el-input
          v-model="searchForm[item.prop]"
          :placeholder="item.placeholder"
        />
      </template>
      <template v-if="item.type === 'date-picker'">
        <el-date-picker
          v-model="searchForm[item.prop]"
          type="daterange"
          range-separator="—"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          size="large"
        />
      </template>
    </el-form-item>
  </el-col>
</template>
```


## 4.2 补充: 作用域slot 插槽
![[Pasted image 20230331145457.png]]

```html
<div>
  <!--预留具名插槽-->
  <slot v-bind='obj' :name='img'/>
</div>
```
使用
```html
<template #img='scope'>
  <!-- 从插槽的作用域中取数据 -->
  <el-image :src='scope.row.imgUrl'>
</template>
```

## 4.3 高阶组件抽离
高阶组件抽离, 既要实现通用组件的抽离, 又要实现单个组件的独特性 (又要允许自定义)

实现 => 预留插槽、特殊用法特殊写

举个🌰: 

公共组件 `<PageDialog>` 预留插槽、允许自定义
```html
<div class="form">
  <el-form :model="formDate" label-width="80px">
    <template v-for="item in dialogConfig.formItems" :key="item.prop">
      <el-form-item :label="item.label" :prop="item.prop">
        <!--...-->
        <template v-if="item.type === 'custom'">
          <slot :name="item.slotName"></slot>
        </template>
      </el-form-item>
    </template>
  </el-form>
</div>
```

现在实现对话框中、插入一个 `el-tree` 组件
```html
<PageDialog>
    <template #menuList>
        <el-tree
          ref="treeRef"
          :data="entireMenus"
          show-checkbox
          node-key="id"
          highlight-current
          :props="{ children: 'children', label: 'name' }"
          @check="handleElTreeCheck"
        />
    </template>
</PageDialog>
```

# 五、按钮级别权限控制