
## 一、概述
通过 `ref` 返回一个响应式的数据，可以在 `html` 模板上直接使用。
## 二、步骤
### 2.1 引入 ref
```js
import { ref } from "vue";
```
### 2.2 setup() 
```js
export default {
    name: 'App',
    setup() {
        const serverValue = ref();
        onMounted(() => {
          axios({
            method: "GET",
            url: "http://localhost:8080/getVaule",
          }).then((res) => {
              console.log("res.data = " + res.data);
              // 必须在加上 .value 
              serverValue.value = res.data;
              ElMessage.success("获取成功");
            }).catch((err) => {
              console.log("err = " + err);
              ElMessage.error("获取失败");
            });
        });

        return {
          serverValue // 不可以有初始值
        }
  } 
}
```
### 2.3 使用 serverValue
```html
<h1>{{ serverValue }}</h1>
```
### 2.4 后端
通过 `axios` 的 `Get` 请求后端返回一个 `String`，然后前端显示。
```java
@GetMapping(value = "/getVaule")
public String getValue(){
    return "孙悟空";
}
```