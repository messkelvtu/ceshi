### Apache POI导入导出EXCEL

##### 1、前端发起请求

不能为异步请求，浏览器禁止异步请求下载

```javascript
window.open("/kpi/regedit/exportYearPublic?exportTime="+new Date(exportTime).getTime());
```

##### 2、后台传入response对象

设置响应头

```java
response.setHeader("Cache-Control", "no-cache, no-store, must-revalidate");
response.setHeader("Pragma", "no-cache");
response.setHeader("Expires", "0");
response.setHeader("charset", "utf-8");
response.setHeader("Content-Disposition", "attachment;filename=\"" + URLEncoder.encode(year + "年公休汇总.xlsx", "UTF-8") + "\"");
```

##### 3、设置写出数据

###### 1、创建excel文件对象

```java
//创建工作簿
XSSFWorkbook xssfWorkbook = new XSSFWorkbook();
```

###### 2、用工作簿对象，创建sheet对象，并设置名称

```java
//生成sheet名称
XSSFSheet sheet = xssfWorkbook.createSheet(calendar.get(Calendar.YEAR) + "." + (calendar.get(Calendar.MONTH) + 1));
```

###### 3、用sheet对象，创建行对象

```java
//行对象，row参数为指向sheet的第row行，从0开始
XSSFRow head0 = sheet.createRow(row);
```

###### 4、用行对象，创建单元格对象

```java
//单元格对象，0指代这一行的第一个单元格
XSSFCell cell0 = head0.createCell(0);
//设置该单元格的值
cell0.setCellValue(year + "年" +"请假汇总（非公休）");
```

###### 5、样式对象，设置给单元格对象

```java
//样式对象
CellStyle cellStyle = xssfWorkbook.createCellStyle();
//该单元格应用该样式
cell8.setCellStyle(cellStyle8);
```

###### 6、设置样式

```java
cellStyle.setAlignment(HorizontalAlignment.CENTER); // 水平布局：居中
cellStyle.setVerticalAlignment(VerticalAlignment.CENTER);// 垂直布局：居中
cellStyle.setFillForegroundColor(IndexedColors.YELLOW.getIndex()); // 背景色: 浅黄色
cellStyle.setFillPattern(FillPatternType.SOLID_FOREGROUND); // 背景色填充样式：单色填充
sheet.addMergedRegion(new CellRangeAddress(row, row, 0, REGEDITHEAD.length - 1));
cellStyle.setBorderLeft(BorderStyle.THIN);// 薄边框
cellStyle.setBorderRight(BorderStyle.DOUBLE); // 双边框
```

###### 7、用工作簿对象，创建字体对象，设置给样式对象

```java
//字体对象
Font font = xssfWorkbook.createFont();
font.setBold(true); // 加粗
cellStyle.setFont(font);
```

###### 8、用sheet对象，合并单元格

参数1：开始行号

参数2：结束行号

参数3：开始单元格

参数4：结束单元格

```java
//合并单元格
sheet.addMergedRegion(new CellRangeAddress(row, row, 0, REGEDITHEAD.length));
```

###### 9、下载（导出）工作簿

```
//写出工作簿
xssfWorkbook.write(response.getOutputStream());
//关闭流
xssfWorkbook.close();
```

##### 总结

1、前端发起请求
2、设置响应头（响应类型、响应文件名等）
3、设置数据（从大到小顺序设置）

```txt
a、工作簿对象
b、sheet对象
c、行对象
d、单元格对象
```

锦上添花对象

```txt
样式对象
字体对象
```

4、写出（下载表格）
