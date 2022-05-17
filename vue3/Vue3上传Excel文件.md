## 一、安装 axios
### 1.1 安装 axios
> npm install axios -S

### 1.2 安装 vue-axios
> npm install vue-axios -S


在 `package.json` 中的 `dependencies`检查是否成功安装好。
### 1.3 全局引入 axios
在 `main.js`中 `import` 和引入。
```javascript
import axios from 'axios'

const app = createApp(App)

// 引入
app.config.globalProperties.$axios = axios
```
## 二、编写上传界面
```javascript
<template>
  <el-upload
      ref="uploadRef"
      class="upload-demo"
      action="/uploadExcel"
      accept=".xls,.xlsx,.csv"
      :on-success="handleFileUploadSuccess"
      :on-change="handleChange"
      :before-upload="handleBeforeUpload"
      :auto-upload="false"
      :limit="5"
      :file-list="fileList"
  >
    <template #trigger>
      <el-button type="primary">选择 Excel 文件</el-button>
    </template>

    <el-button class="ml-3" type="success" @click="submitUpload">
      生成
    </el-button>
  </el-upload>
</template>


export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data(){
    return {
      fileList: [],   // 定义一个空数组
    }
  },

  methods: {
		
    /**
     * 上传之前的处理
     * @returns {boolean}
     */
    handleBeforeUpload(rawFile) {
      return true;
    },


    /**
     * 上传成功的处理
     */
    handleFileUploadSuccess(file){
    },

    /**
     * 提交上传的文件
     */
    submitUpload(){
      // 判断是否有文件再上传
      if (this.fileList.length === 0) {
          return this.$message.warning('请选取文件后再上传')
      }
      // 下面的代码将创建一个空的 FormData 对象:
      const formData = new FormData();
			
			// 参数的键名为 'file'
      this.fileList.forEach((file) => {
          formData.append('file', file.raw);
      })
			
			// 通过 axios 进行上传
			// uploadExcelFile 后端处理的路径
      this.params = formData;
      axios({
        method: 'post',
        url: 'http://localhost:8080/uploadExcelFile',
        data: this.params,
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded'
        }
      })
    },

    /**
     * 过滤上传的文件类型
     */
    handleChange(file, fileList){
      if (!/\.(xlsx|xls|XLSX|XLS|csv)$/.test(file.name)) {
        this.$message({
          type: 'warning',
          message: '上传文件只能为 Excel 文件!'
        })
        this.fileList = []
        return false
      }
      // 手动上传需要保存 file.raw
      this.file = file.raw 
    }
  }
}
</script>

```
## 三、后端处理 Post 请求
### 3.1 后端处理函数
```javascript
@CrossOrigin
@Controller
public class ExcelController {
    @Autowired
    private UploadDAO uploadDAO;
	
    /**
     * 读取用户上传过来的 Excel 文件
     * @param file 上传的 Excel 文件
     * @param response
     */
    @PostMapping(value = "/uploadExcelFile")
    @ResponseBody
    public void readUploadExcel(@RequestParam("file") MultipartFile file) throws IOException {
        EasyExcel.read(file.getInputStream(), ExcelEntity.class, new ExcelEntityListener(uploadDAO)).sheet(0).doRead();
    }
}
```
`file` 就是传递过来的 `Excel` 文件。

为了获取到 Excel 里面的值，我们还需要一个 Excel 处理框架，我选择了阿里的 `easyExcel`。
### 3.2 添加 dependency
```javascript
<!-- 导入 easyexcel 用来做 excel 处理-->
<dependency>
	<groupId>com.alibaba</groupId>
	<artifactId>easyexcel</artifactId>
	<version>3.0.5</version>
</dependency>
```
```javascript
<dependency>
	<groupId>com.alibaba</groupId>
	<artifactId>fastjson</artifactId>
	<version>1.2.80</version>
</dependency>
```
一块添加 `fastjson`后面会用来处理获取到的数据，把它 `json`化。
### 3.3 创建 Excel 文件的实体类 ExcelEntity.java
```javascript
@Data
public class ExcelEntity{
    @ExcelProperty("订单号")
    private String orderId;         // 订单号

    private String externalOrderId; // 外部订单号
    private String orderCommodityState; // 重要：订单商品状态：「待发货」
    private Date tradeSuccessTime; // 交易成功时间
    private String commodityName; // 重要：商品名称
    private String commodityType; // 商品类型
    private String commodityCategory; // 商品类目
    private String commoditySpecification; // 重要：商品规格
    private String specificationCode; // 规格编码, 默认为 ""
    private String commodityCode; // 商品编码，默认为 ""
    private double commodityUnitPrice; // 商品单价
    private String commodityCostPrice; // 商品成本价
    private String commodityPromotionMethod; // 商品优惠方式
    private double commodityPromotionAfterPrice; // 商品优惠后价格
    private int commodityNumber; // 重要：商品数量
    private double commodityMoneyTotal; // 商品金额小计
    private String shopPromotion; // 店铺优惠
    private String commodityActualDealPrice; // 商品实际成交金额
    private String commodityCreditIntegralNumber; // 商品抵用积分数
    private String commodityMessage; // 商品留言
    private String consignee; // 重要：收货人
    private String consigneeTel; // 重要：收货人手机号
    private String consigneeProvince; // 重要：收货人所在省份
    private String consigneeCity; // 重要：收货人所在城市
    private String consigneeDistrict; // 重要：收货人所在地区
    private String consigneeDetailAddress; // 重要：收货人所在地址
    private String buyerRemark; // 买家备注
    private String commodityShipState; // 商品发货状态
    private String commodityShipStyle; // 商品发货方式
    private String commodityShipLogisticsCompany; // 商品发货物流公司
    private String commodityShipLogisticsId; // 商品发货物流单号
    private String commodityShipEmployee; // 发货员工
    private Date commodityShipTime; // 发货时间
    private String commodityRefundState; // 商品退款状态
    private String commodityRefundMoney; // 商品已退款金额
    private String businessmanOrderRemark; // 商家订单备注
    private Date checkInTime; // 入住时间
}
```
这个会将 `Excel` 文件中列的数据与类的字段绑定在一块，按照声明的顺序，如果不想按照顺序可以使用 `@ExcelProperty("列名")`来绑定在一块。
### 3.4 创建 ExcelEntityListener.java 监听器
```javascript
/**
 *
 * @Author 云胡
 * @Description Excel 工具类
 * @Date 2022-04-15
 */

@Slf4j
public class ExcelEntityListener implements ReadListener<ExcelEntity> {
    public ExcelEntityListener(UploadDAO uploadDAO) {
        this.uploadDAO = uploadDAO;
    }

    /**
     * 这个每一条数据解析都会来调用
     *
     * @param context
     */
    @Override
    public void invoke(ExcelEntity data, AnalysisContext context) {
        log.info("解析到一条数据:{}", JSON.toJSONString(data));
    }

    /**
     * 所有数据解析完成了 都会来调用
     * @param context
     */
    @Override
    public void doAfterAllAnalysed(AnalysisContext context) {

        log.info("所有数据解析完成！");
    }
}
```
## 3.5 创建 UploadDao.java
```javascript
@Repository
public class UploadDAO {
    public void save(List<ExcelEntity> list) {
        // 如果是mybatis,尽量别直接调用多次insert,自己写一个mapper里面新增一个方法batchInsert,所有数据一次性插入
    }
}
```
目前没有使用，后续可以存到数据库里。

## 四、结果展示
### 4.1 本地 Excel 测试数据
| 订单号 | 外部订单号 | 订单商品状态 | 交易成功时间 | 商品名称 | 商品类型 | 商品类目 | 商品规格 | 规格编码 | 商品编码 | 商品单价 | 商品成本价 | 商品优惠方式 | 商品优惠后价格 | 商品数量 | 商品金额小计 | 店铺优惠（分摊） | 商品实际成交金额 | 商品抵用积分数 | 商品留言 | 收货人/提货人 | 收货人手机号/提货人手机号 | 收货人省份 | 收货人城市 | 收货人地区 | 详细收货地址/提货地址 | 买家备注 | 商品发货状态 | 商品发货方式 | 商品发货物流公司 | 商品发货物流单号 | 发货员工 | 商品发货时间 | 商品退款状态 | 商品已退款金额 | 商家订单备注 | 入住时间 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| E20220331142238091806171 | C1728795398635640 | 待发货 |  | Muchshow美腹贴 塑形新科技 | 普通类型商品 | 其他 | 美腹贴女士2盒 |  |  | 278.8 |  |  | 278.8 | 1 | 278.8 | 69.20(分销商等级折扣：47.80) | 348 | 0 |  | 孙悟空 | 13666666666 | 云南省 | 昆明市 | 盘龙区 | 云南省昆明市盘龙区佛寺 |  | 未发货 | 快递 |  |  |  |  |  | 0 |  |  |
| E20220330182239081300015 | C1728719896969425 | 待发货 |  | 【环保袋中的爱马仕】德国单肩时尚环保袋 超轻便携 创意礼品  博物馆系列 | 普通类型商品 | 其他 | 麦田 |  |  | 73.5 |  |  | 73.5 | 1 | 73.5 | 24.50(分销商等级折扣：9.80) | 98 | 0 |  | 猪八戒 | 	13999999999 | 山东省 | 淄博市 | 张店区 | 山东省淄博市张店区佛寺 |  | 未发货 | 快递 |  |  |  |  |  | 0 |  |  |

### 4.2 上传
![image.png](https://cdn.nlark.com/yuque/0/2022/png/726118/1650019040799-71c5f6d0-1260-4d35-a4e3-ce3c40b6bdb5.png#clientId=u93623316-934d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=204&id=xuSsW&margin=%5Bobject%20Object%5D&name=image.png&originHeight=255&originWidth=660&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4236&status=done&style=none&taskId=uddf51250-5f46-47fe-8149-93479cad65d&title=&width=528)
点击选择 Excel 文件，将本地 Excel 文件上传，然后点击生成。
## 4.3 log 输出的 Excel 数据
```systemverilog
2022-04-15 18:40:29.964  INFO 17068 --- [nio-8080-exec-1] c.e.c.Util.ExcelEntityListener             : 解析到一条数据:{"commodityActualDealPrice":"348","commodityCategory":"其他","commodityCreditIntegralNumber":"0","commodityMoneyTotal":278.8,"commodityName":"Muchshow美腹贴 塑形新科技","commodityNumber":1,"commodityPromotionAfterPrice":278.8,"commodityRefundMoney":"0","commodityShipState":"未发货","commodityShipStyle":"快递","commoditySpecification":"美腹贴女士2盒","commodityType":"普通类型商品","commodityUnitPrice":278.8,"consignee":"孙悟空","consigneeCity":"昆明市","consigneeDetailAddress":"云南省昆明市盘龙区佛寺","consigneeDistrict":"盘龙区","consigneeProvince":"云南省","consigneeTel":"13666666666","externalOrderId":"C1728795398635640","orderCommodityState":"待发货","orderId":"E20220331142238091806171","shopPromotion":"69.20(分销商等级折扣：47.80)"}
2022-04-15 18:40:29.967  INFO 17068 --- [nio-8080-exec-1] c.e.c.Util.ExcelEntityListener             : 解析到一条数据:{"commodityActualDealPrice":"98","commodityCategory":"其他","commodityCreditIntegralNumber":"0","commodityMoneyTotal":73.5,"commodityName":"【环保袋中的爱马仕】德国单肩时尚环保袋 超轻便携 创意礼品  博物馆系列","commodityNumber":1,"commodityPromotionAfterPrice":73.5,"commodityRefundMoney":"0","commodityShipState":"未发货","commodityShipStyle":"快递","commoditySpecification":"麦田","commodityType":"普通类型商品","commodityUnitPrice":73.5,"consignee":"猪八戒","consigneeCity":"淄博市","consigneeDetailAddress":"山东省淄博市张店区佛寺","consigneeDistrict":"张店区","consigneeProvince":"山东省","consigneeTel":"13999999999","externalOrderId":"C1728719896969425","orderCommodityState":"待发货","orderId":"E20220330182239081300015","shopPromotion":"24.50(分销商等级折扣：9.80)"}
2022-04-15 18:40:29.968  INFO 17068 --- [nio-8080-exec-1] c.e.c.Util.ExcelEntityListener             : 所有数据解析完成！
```


