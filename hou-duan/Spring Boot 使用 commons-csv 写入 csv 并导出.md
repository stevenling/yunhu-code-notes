# 一、概述
通过 `commons-csv` 来写入数据并且导出到客户端。
# 二、引入依赖
```yaml
<dependency>
  <groupId>org.apache.commons</groupId>
  <artifactId>commons-csv</artifactId>
  <version>1.9.0</version>
</dependency>
```
# 三、步骤
## 3.1 写入 csv 数据
```java
     public byte[] writeCsv() {
         final String[] headerArr = new String[]{"* 编号", "* 收货人姓名", "* 收货地址省份", "*收货地址城市", "* 收货地址区/县", "* 收货详细地址", "* 邮编", "* 固定电话", "* 移动电话", "订单附言", "*配送方式", "外部订单号"};

         byte[] bytes = new byte[0];
         try {
             // 字节数组输出流在内存中创建一个字节数组缓冲区，所有发送到输出流的数据保存在该字节数组缓冲区中。
             ByteArrayOutputStream byteArrayOutputStream = null;
             byteArrayOutputStream = new ByteArrayOutputStream();
             byte[] bom = {(byte) 0xEF, (byte) 0xBB, (byte) 0xBF};
             byteArrayOutputStream.write(bom);
             
             // 将输出的字符流变为字节流
             OutputStreamWriter outputStreamWriter = new OutputStreamWriter(byteArrayOutputStream, StandardCharsets.UTF_8);

             // 在写操作期间，字符将被写入内部缓冲区而不是磁盘。 一旦缓冲区被填充或关闭写入器，缓冲区中的所有字符将被写入磁盘。
             BufferedWriter bufferedWriter = new BufferedWriter(outputStreamWriter);
             CSVFormat csvFormat = CSVFormat.DEFAULT;
             csvPrinter = new CSVPrinter(bufferedWriter, csvFormat);
             // 写入标题
             csvPrinter.printRecord(headerArr);
             // 写入内容
             for (int i = 2; i < 10; i++)
             {
                 csvPrinter.printRecord("userId" + i, "userName" + i);
             }
             csvPrinter.flush();
             bytes = byteArrayOutputStream.toString(StandardCharsets.UTF_8.name()).getBytes();
             return bytes;
         }catch (IOException e){
             e.printStackTrace();
         }
         return bytes;
    }
```
## 3.2 设置下载响应
```java
    /**
     * 设置下载响应
     * @param fileName 文件名 eg: abc.csv
     * @param bytes 输出流
     * @param response 响应报文
     */
    public void setResponseProperties(String fileName, byte[] bytes, HttpServletResponse response) {
        try {
            // 设置编码格式
            fileName = URLEncoder.encode(fileName, StandardCharsets.UTF_8.name());
            response.setContentType("application/csv");
            response.setCharacterEncoding(StandardCharsets.UTF_8.name());
            response.setHeader("Pragma", "public");
            response.setHeader("Cache-Control", "max-age=30");
            response.setHeader("Content-Disposition", "attachment; filename=" + fileName);
            OutputStream outputStream = response.getOutputStream();
            outputStream.write(bytes);
            outputStream.flush();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
```
