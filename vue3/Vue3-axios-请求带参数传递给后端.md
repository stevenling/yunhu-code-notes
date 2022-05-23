## 一、概述
做一个下载 `Excel` 表格的网页小工具，下拉菜单有多个选择，所以必须带一个参数告诉后端下载的是哪一个。
## 二、代码
### 2.1 vue 代码
`el-select` 通过 `v-model` 与 `downloadIndex` 绑定在一块。
因此前端选择哪一项，`downloadIndex` 就是那一项的值。

```javascript
/**
 * 点击下载按钮
 */
downloadExcel() {
  axios({
    method: "GET",
    url: "http://localhost:8085/downloadCsvFile",
    responseType: "blob",
    params: {
      downloadIndex: this.downloadIndex
    } 
  })
}
```
### 2.2 后端 controller 代码
```java
@GetMapping(value = "/downloadCsvFile")
public void downloadCsvFile(HttpServletResponse response, @RequestParam String downloadIndex) throws IOException {
    log.info("downloadIndex " + downloadIndex);
}
```
