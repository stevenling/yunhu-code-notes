# Vue3 实现路由跳转

### 一、安装 vue-router

> npm install vue-router@4

### 二、新建 vue 页面

在 `src` 目录下新建 `view` 目录用来存放 `vue` 的页面。

然后在 `view`目录下新建两个 `vue` 页面，分别是 `login.vue` 和 `register.vue`。

#### 2.1 login.vue

```js
<template>
  <div>
    当前是登录页面
  </div>
</template>

<script>
export default {
  name: "login"
}
</script>

<style scoped>
</style>
```

#### 2.2 register.vue

```js
<template>
  <div>
    当前是注册页面
  </div>
</template>

<script>
export default {
  name: "register"
}
</script>

<style scoped>
</style>
```

#### 三、新建路由文件

在 `src` 目录下新建 `router` 目录用来存放路由配置的页面。

#### 3.1 新建 index.js

在 router 目录下新建 index.js 配置路由。

```
import {createRouter, createWebHistory} from 'vue-router'
import routes from './routes'    // 导入 router 目录下的 router.js

const router = createRouter({
	history: createWebHistory(process.env.BASE_URL),
	routes
})
export default router;
```

目前 `routes` 里面还没有路由路径等内容，因此我们要再新建一个 `routes.js`文件。

#### 3.2 新建 routes.js

还是在 `router` 目录下新建 `routes.js`

```js
import Register from '@/view/register.vue'
import Login from '@/view/login.vue'

const routes = [
    {
        name: 'login',
        path: '/login',
        component: Login
    },
    {
        name: 'register',
        path: '/register',
        component: Register
    }
];
export default routes
```

导入刚刚新建的 `vue` 页面，然后和 `path` 绑定在一块。

### 四、在 App.vue 中配置路由的跳转

```js
<template>
<div id = "app">
	<p>
	  <el-button>
            <router-link to="/login">登录</router-link>
	  </el-button>
          
	  <el-button>
            <router-link to="/register">注册</router-link>
	  </el-button>
	</p>
	<router-view/>
	</div>
</template>

<script>
// App.vue 的名称叫 app
    export default {
        name: 'app'
    }
</script>
```

注意要使用 `router-link` 标签来进行路由的跳转。

`el-button`是这边 `Element UI`框架的控件，如果没安装，可以不使用。

### 五、在 main.js 中 use 路由

```js
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'
import router from './router/index' // 加载 router 底下的 index.js 路由配置文件

const app = createApp(App)
app.use(router)		
app.use(ElementPlus) // 没安装 Element UI 可以不用
app.mount('#app')
```

### 六、src 目录结构

```
src
│  App.vue
│  main.js
│      
├─router
│      index.js
│      routes.js
│      
└─view
        login.vue
        register.vue
```

### 七、结果

#### 7.1 默认页面

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f5b326124a9945429c69afab9297bf7f\~tplv-k3u1fbpfcp-zoom-1.image)

#### 7.2 点击登录

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/41be03cbcafe4ebf8a77965a8911731e\~tplv-k3u1fbpfcp-zoom-1.image)

#### 7.3 点击注册

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d7f51a9943ac4403bf461dd7eda24ed6\~tplv-k3u1fbpfcp-zoom-1.image)
