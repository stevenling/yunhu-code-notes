# Vue3 搭配 Element-Plus

### 一、初始化 vue3 项目

在终端中进入存放项目路径的文件夹，然后使用命令：

```shell
vue create project
```

这时候 `vue` 会生成项目的基础内容。

### 二、安装 element-plus

在项目路径底下使用以下命令进行 `element3` 的安装：

> npm install element-plus --save

### 三、引入 element-plus

#### 3.1 main.js

```js
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'


const app = createApp(App)
app.use(ElementPlus)
app.mount('#app')
```

#### 3.2 引入 Element 组件

**3.2.1 在 component 目录下新建 MyElButton.vue 组件**

```js
<template>
<div>
	<el-row>
		<el-button @click="openMessage">打开通知</el-button>
		<el-button type="primary" >弹框</el-button>
		<el-button type="success" >打开对话框</el-button>
		<el-button type="info" >通知消息</el-button>
		<el-button type="warning">警告按钮</el-button>
		<el-button type="danger">危险按钮</el-button>
	</el-row>
	</div>
</template>

<script>
	import { ElMessage} from 'element-plus'
	export default {
		name: "MyElButton",
		setup(){
			/**
			 * 打开通知按钮点击事件的响应
			 */
			let openMessage =() =>{
				ElMessage({
					message: "成功",
					type: "success"
				})
			}
			return{
				openMessage
			}
		}
	}
</script>

<style scoped>
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
		margin-top: 60px;
	}
</style>
```

**3.2.2 在 App.vue 中加载 MyElButton 组件**

```js
<template>
  <div id = "app">
    <MyElButton>
    </MyElButton>
  </div>
</template>

<script>
import MyElButton from './components/MyElButton.vue'
export default {
  name: 'app',
  // 注册导入的组件
  components: {
    MyElButton
  }
}
</script>
```

### 四、运行结果

输入 `npm run serve` 运行项目，访问。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7ec0d5637cd541f6aad96f7c970bf474\~tplv-k3u1fbpfcp-zoom-1.image)

点击「打开通知」按钮，弹出通知成功。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3eb0ff1fc47f41f6bdb40b4fd5b5211f\~tplv-k3u1fbpfcp-zoom-1.image)

### 五、参考资料

* [Element Plus](https://element-plus.gitee.io/zh-CN/)
