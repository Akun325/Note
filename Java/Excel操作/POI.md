# POI
## 1. POI简介
### 1.1 Excel简介
- Excel的两种版本
  - Excel2003
    - 底层采用特有的二进制格式,其核心结构是复合文档类型的结构,存储数据量小,一张表最大支持  65536行数据，256列
    - 文件后缀: xls
  - Excel2007
    - 其核心结构是XML类型的结构,采用的是基于XML的压缩方式,占用空间更小,操作效率更高
    - 文件后缀: xlsx

### 1.2 POI概述
- Apache POI是Apache软件基金会的开放源码函式库，POI提供API给Java程序对Microsoft Office格式档案读和写的功能。
- [Apache POI 官网](https://poi.apache.org/)
- Apache POI 是创建和维护操作各种符合Office Open XML（OOXML）标准和微软的OLE 2复合文档格式（OLE2）的Java API。用它可以使用Java读取和创建,修改MS Excel文件.而且,还可以使用Java读取和创建MS Word和MSPowerPoint文件。Apache POI 提供Java操作Excel解决方案（适用于Excel97-2008）。
  
### 1.3 POI API介绍
HSSF － 提供读写Microsoft Excel XLS格式档案的功能。
XSSF － 提供读写Microsoft Excel OOXML XLSX格式档案的功能。
HWPF － 提供读写Microsoft Word DOC格式档案的功能。
HSLF － 提供读写Microsoft PowerPoint格式档案的功能。
HDGF － 提供读Microsoft Visio格式档案的功能。
HPBF － 提供读Microsoft Publisher格式档案的功能。
HSMF － 提供读Microsoft Outlook格式档案的功能。
SXSSF － 提供读写Microsoft Excel OOXML XLSX格式档案的海量数据的功能。

- 工作簿 Workbook
  - Workbook是位于org.apache.poi.ss.usermodel包的接口，它是POI库的工作簿的所有类的超接口。
  - Workbook实现类
     - HSSFWorkbook :  一般用于Excel2003版及更早版本(扩展名为.xls)的导出。一张表最大支持65536行数据，256列。也就是说excel2003完全不可能满足百万数据导出的需求。但是 可以通过分表进行实现百万数据需求。
     - XSSFWorkbook:   一般用于Excel2007版(扩展名为.xlsx)的导出。一张表最大支持1048576行，16384列；
     - SXSSFWorkbook:  SXSSFWorkbook:是操作Excel2007后的版本，扩展名是.xlsx；大数据量导出格式  可以千万数据级别
  - SXSSF与XSSF的对比：
    - 在一个时间点上，只可以访问一定数量的数据
    - 不再支持Sheet.clone()
    - 不再支持公式的求值
    - 在使用Excel模板下载数据时将不能动态改变表头，因为这种方式已经提前把excel写到硬盘的了就不能再改了

- 表单 Sheet
  - Sheet是位于org.apache.poi.ss.usermodel包的接口，它是POI库的表单的所有类的超接口。
  
- 行 Row
  - Row是在org.apache.poi.ss.usermodel包的接口。它是POI库的行所有类的超接口。
  
- 单元格 Cell
  - Cell是在org.apache.poi.ss.usermodel包的接口。它是POI库的单元格的所有类的超接口。
  
- 单元格样式 CellStayle
  - Cell是在org.apache.poi.ss.usermodel包的接口。它是POI库的单元格样式的所有类的超接口。

- 字体 Font
  - Cell是在org.apache.poi.ss.usermodel包的接口。它是POI库的字体的所有类的超接口。
  


### 1.4 POI性能比较
- 分别使用HSSFWorkbook,XSSFWorkbook,SXSSFWorkbook 导出10000、20000、30000、40000、50000、60000、100000、150000、200000、500000、1000000条数据。

**PS: 使用HSSFWorkbook在导出超过50000条数据时，使用的是同一个HSSFWorkbook创建多个Sheet实现的，因为HSSFWorkbook每张表（Sheet）中最多只能存65536条数据，为了避免报内存溢出错误，所以控制每张表中存储50000条数据。**

数据量/耗时
![20220529141014](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220529141014.png)

- 结果分析
  - 5万条数据以内：
  XSSFWorkbook耗时明显比其他两个耗时长，而HSSFWorkbook和SXSSFWorkbook耗时基本相同。
  - 数据大于5万条小于50万条，HSSFWorkbook采用分表策略存储数据时：
  XSSFWorkbook耗时依然明显比其他两个耗时长，但是相比于HSSWorkbook优势就是他的数据在一个表中。而HSSFWorkbook和SXSSFWorkbook耗时基本相同。
  - 大于50万条数据：
  XSSFWorkbook按照原理来说他的上限是100万左右，但是相比于SXSSFWorkbook来说，还是SXSSFWorkbook耗时最短，所以SXSSFWorkbook更适合处理大数据量。

- 总结
  - 大文件写HSSF(03版本)
    - 优点：过程中写入内存，不操作磁盘，最后一次性全部写入到磁盘中，速度快。
    - 缺点：只能处理65536行，超出抛异常
  - 大文件写XSSF(07版本)
    - 优点： 可以写较大数据量，如20万条
    - 缺点：写入数据时的速度非常慢,比较耗内存，也会发生内存溢出，比如100万条数据
  - 大文件写SXSSF(07加强版)
    - 优点：可以写非常大的数据量，如100万条甚至更多条，写数据速度更快，占用更少的内存。
      注意： 过程中会产生临时文件，需要清理临时文件
      默认由100条记录被保存在内存中，如果超过这数量，则最前面数据写入临时文件（需要清除临时文件)
           ```
            ((SXSSFWorkbook)workbook).dispose();
           ```
       如果想自定义内存数据的数量,可以使用new SXSSFWorkbook(数量)


## 2. POI基本操作
- POI使用07版本写和读的代码实现(07版本和03版本基本是一样的，只是创建工作薄的对象不同而已)
### 2.1 POI导出操作
#### 2.1.1 简单导出操作
- 代码示例
~~~java
    @Test
    public void test1() throws IOException {
 
        //创建03工作簿
        Workbook wb=new HSSFWorkbook();
        //创建sheet
        Sheet sheet = wb.createSheet();
        //创建sheet页中的行,第一行(这里的0是下角标)
        Row row = sheet.createRow(0);
        //创建行中的单元格
        Cell cell = row.createCell(0);
        //给第一个单元格设置值
        cell.setCellValue(1);
        //逐一给单元格设置值不同类型的值(这里的data类型会出现看不懂的玩意不要在意,下边有具体的案例仅供参考)
        row.createCell(1).setCellValue(false);
        row.createCell(2).setCellValue(2.00);
        row.createCell(3).setCellValue("哈哈");
        row.createCell(4).setCellValue(new Date());
        //创建文件输出流（这里的路径是自定义的我写的是D盘下当然了后缀一定要注意）
        FileOutputStream fos=new FileOutputStream("aaa.xls"); 
        //给工作簿指定文件输出流(写文件)
        wb.write(fos);
        //关闭流(关流是重中之重必须写)
                fos.close();            
    }
~~~

#### 2.1.2 处理Date类型
![20220529160216](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220529160216.png)
- 直接传Date类型Excel会解析成浮点类型
- 代码示例
~~~java
   @Test
    public void  tyest2() throws IOException {
        //创建工作谱对象
        Workbook hwb =new HSSFWorkbook();
        //创建sheet页
        Sheet sheet = hwb.createSheet("sheet1");
        //创建row/单元格
        Row row = sheet.createRow(0);
        Cell cell = row.createCell(0);
        cell.setCellValue(new Date());

        //处理时间对象
        //创建帮助对象
        CreationHelper helper = hwb.getCreationHelper();
        //创建cell的样式对象
        CellStyle cellStyle = hwb.createCellStyle();

        //处理时间
        cellStyle.setDataFormat(helper.createDataFormat().getFormat("yyy-MM-ddhh:mm:ss"));

        cell=row.createCell(1);
        //传入时间对象
        cell.setCellValue(new Date());
        //设置样式
        cell.setCellStyle(cellStyle);
        row.createCell(2);
        //传入日历对象
        cell.setCellValue(Calendar.getInstance());
        //设置样式
        cell.setCellStyle(cellStyle);
        //创建文件输出流
        FileOutputStream fos = new FileOutputStream("bbb.xls");
        //通过输出流写表格
        hwb.write(fos);
        //关流
        fos.close();
    }
~~~

#### 2.1.3 背景色设置
-代码示例
~~~java
    @Test
    public void test3() throws IOException {

        //创建工作谱
        Workbook workbook = new HSSFWorkbook();

        //创建sheet页
        Sheet createSheet = workbook.createSheet("sheet1");

        //创建行
        Row row = createSheet.createRow(1);

        //设置行高
        row.setHeightInPoints(30);

        //创建列
        Cell cell = row.createCell(1);

        //给列设置值
        cell.setCellValue("我是单元格");

        //创建样式对象
        CellStyle style = workbook.createCellStyle();

        //设置背景色
        style.setFillForegroundColor(IndexedColors.BLUE.getIndex());
        style.setFillPattern(FillPatternType.SOLID_FOREGROUND);
        cell.setCellStyle(style);

        //文件输出流
        OutputStream outputStream = new FileOutputStream("ccc.xls");

        //写入
        workbook.write(outputStream);

        //关流
        outputStream.close();
    }
  ~~~
- Excel背景色对应枚举
![20220529165528](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220529165528.png)

~~~java
1.IndexedColors.AQUA.getIndex()
2.IndexedColors.AUTOMATIC.getIndex()
3.IndexedColors.BLUE.getIndex()
4.IndexedColors.BLUE_GREY.getIndex()
5.IndexedColors.BRIGHT_GREEN.getIndex()
6.IndexedColors.BROWN.getIndex()
7.IndexedColors.CORAL.getIndex()
8.IndexedColors.CORNFLOWER_BLUE.getIndex()
9.IndexedColors.DARK_BLUE.getIndex()
10.IndexedColors.DARK_GREEN.getIndex()
11.IndexedColors.DARK_RED.getIndex()
12.IndexedColors.DARK_TEAL.getIndex()
13.IndexedColors.DARK_YELLOW.getIndex()
14.IndexedColors.GOLD.getIndex()
15.IndexedColors.GREEN.getIndex()
16.IndexedColors.GREY_25_PERCENT.getIndex()
17.IndexedColors.GREY_40_PERCENT.getIndex()
18.IndexedColors.GREY_50_PERCENT.getIndex()
19.IndexedColors.GREY_80_PERCENT.getIndex()
20.IndexedColors.INDIGO.getIndex()
21.IndexedColors.LAVENDER.getIndex()
22.IndexedColors.LEMON_CHIFFON.getIndex()
23.IndexedColors.LIGHT_BLUE.getIndex()
24.IndexedColors.LEMON_CHIFFON.getIndex()
25.IndexedColors.LIGHT_BLUE.getIndex()
26.IndexedColors.LIGHT_CORNFLOWER_BLUE.getIndex()
27.IndexedColors.LIGHT_GREEN.getIndex()
28.IndexedColors.LIGHT_ORANGE.getIndex()
29.IndexedColors.LIGHT_TURQUOISE.getIndex()
30.IndexedColors.LIGHT_YELLOW.getIndex()
31.IndexedColors.LIME.getIndex()
32.IndexedColors.MAROON.getIndex()
33.IndexedColors.OLIVE_GREEN.getIndex()
34.IndexedColors.ORANGE.getIndex()
35.IndexedColors.ORCHID.getIndex()
36.IndexedColors.PALE_BLUE.getIndex()
37.IndexedColors.PINK.getIndex()
38.IndexedColors.PLUM.getIndex()
39.IndexedColors.RED.getIndex()
40.IndexedColors.ROSE.getIndex()
41.IndexedColors.ROYAL_BLUE.getIndex()
42.IndexedColors.SEA_GREEN.getIndex()
43.IndexedColors.SKY_BLUE.getIndex()
44.IndexedColors.TAN.getIndex()
45.IndexedColors.TEAL.getIndex()
46.IndexedColors.TURQUOISE.getIndex()
47.IndexedColors.VIOLET.getIndex()
48.IndexedColors.WHITE.getIndex()
49.IndexedColors.YELLOW.getIndex()
~~~

#### 2.1.4 样式设置
- 代码示例
~~~java
    @Test
    public void test4() throws IOException {
        //创建工作簿workbook
        Workbook workbook = new HSSFWorkbook();

        //创建表单sheet
        Sheet sheet = workbook.createSheet("sheet1");

        //创建行对象
        Row row = sheet.createRow(2);
        //创建单元对象
        Cell cell = row.createCell(2);

        //向单元中写入内容
        cell.setCellValue("老司机，快上车");

        //创建样式对象
        CellStyle style = workbook.createCellStyle();
        style.setBorderTop(BorderStyle.THIN);//上边框
        style.setBorderBottom(BorderStyle.THIN);//下边框
        style.setBorderLeft(BorderStyle.THIN);//左边框
        style.setBorderRight(BorderStyle.THIN);//右边框

        //创建字体对象
        Font font = workbook.createFont();
        font.setFontName("华文行楷");//设置字体
        font.setFontHeightInPoints((short) 28);//设置字号
        font.setColor((short) 12);//设置颜色
        style.setFont(font);

        //设置行高和列宽
        row.setHeightInPoints(50);//行高
        sheet.setColumnWidth(2,31*256);//列宽

        //居中显示
        style.setAlignment(HorizontalAlignment.CENTER);//水平居中
        style.setVerticalAlignment(VerticalAlignment.CENTER);//垂直居中

        //向单元格设置样式
        cell.setCellStyle(style);

        //创建文件流
        FileOutputStream fos = new FileOutputStream("ddd.xls");

        //写入文件
        workbook.write(fos);
        fos.close();
    }
~~~

#### 2.1.5 合并单元格
- 代码示例
~~~java
@Test
    public void test5() throws IOException {

        //创建 工作铺
        Workbook workbook = new HSSFWorkbook();

        //创建sheet页
        Sheet sheet = workbook.createSheet("sheet1");

        //创建行
        Row row = sheet.createRow(0);

        //创建列
        Cell cell = row.createCell(0);

        //给列设置值
        cell.setCellValue("我是单元格");

        //单元格合并 参数 firstRow lastRow firstCol lastCol
        sheet.addMergedRegion(new CellRangeAddress(0, 2, 0, 1));

        //文件输出流
        OutputStream stream = new FileOutputStream("eee.xls");

        //写入
        workbook.write(stream);

        //关流
        stream.close();
    }
~~~

#### 2.1.6 绘制图形
- 代码示例
~~~java
@Test
    public void test6() throws IOException {
        //创建工作簿workbook
        Workbook workbook = new HSSFWorkbook();

        //创建表单sheet
        Sheet sheet = workbook.createSheet("sheet1");

        //读取图片流
        FileInputStream fis = new FileInputStream("C:\\Users\\86132\\Desktop\\新建文件夹 (2)\\1\\00001.jpg");

        //转换为二进制数组
        byte[] bytes = IOUtils.toByteArray(fis);
        fis.read(bytes);

        //向POI内存中添加一种图片，返回图片在图片集合中的索引
        int index = workbook.addPicture(bytes, Workbook.PICTURE_TYPE_JPEG);

        //绘制图片工具类
        CreationHelper helper = workbook.getCreationHelper();

        //创建一个绘图对象
        Drawing patriarch = sheet.createDrawingPatriarch();

        //创建描点，设置图片坐标
        ClientAnchor anchor = helper.createClientAnchor();
        anchor.setRow1(0);
        anchor.setCol1(0);

        //绘制图片
        Picture picture = patriarch.createPicture(anchor, index);
        picture.resize();//图片自适应

        //创建文件流
        FileOutputStream fos = new FileOutputStream("fff.xls");

        //写入文件
        workbook.write(fos);
        fos.close();
    }
~~~

#### 2.1.7 文本提取
- 代码如下
~~~java
  @Test
	public void textExtraction() throws IOException {
        InputStream inp = new FileInputStream("cellValue.xls");
        HSSFWorkbook wb = new HSSFWorkbook(new POIFSFileSystem(inp));
        ExcelExtractor extractor = new ExcelExtractor(wb);
        extractor.setFormulasNotResults(true);
        extractor.setIncludeSheetNames(false);
        String text = extractor.getText();
        System.out.println(text);
        wb.close();
	}
~~~

### 2.2 POI导入操作
#### 2.2.1 简单导入操作
- 代码示例
~~~java
    @Test
    public void test9() throws IOException {

        //创建文件输入流
        FileInputStream fis=new FileInputStream("D:\\55.xls");

        //创建POI的文件系统
        POIFSFileSystem system = new POIFSFileSystem(fis);

        //创建工作铺对象
        Workbook workbook = new HSSFWorkbook(system);

        //获得第一个sheet页
        Sheet sheet = workbook.getSheetAt(0);

        if(sheet==null) {
            return;
        }

        //遍历row
        for (int i = 0; i < sheet.getLastRowNum(); i++) {
            Row row = sheet.getRow(i);
            if(row==null) {
                continue;
            }

            //遍历列
            for (int j = 0; j < row.getLastCellNum(); j++) {
                //获取单元格
                Cell cell = row.getCell(j);
                if(cell==null) {
                    continue;
                }
                System.out.print("  "+cell.getStringCellValue());
            }
            System.out.println();
        }
    }
~~~

#### 2.2.2 读取不同的数据类型
- 代码示例
~~~java
public class ParseExcelTest {

    //sheet.getLastRowNum() 返回最后一行的下标
    //row.getLastCellNum() 返回单元格的列号
    public static void main(String[] args) throws Exception {
        //（1）创建工作簿workbook
        XSSFWorkbook workbook = new XSSFWorkbook("D:\\excel\\poi\\test.xlsx");
        //（2）获取表单sheet
        XSSFSheet sheet = workbook.getSheetAt(0);
        //（3）获取sheet中的每一行和每一个单元格
        for (int rowNum=0; rowNum <= sheet.getLastRowNum(); rowNum++ ){
            StringBuilder sb = new StringBuilder();
            //获取每一行
            XSSFRow row = sheet.getRow(rowNum);
            for (int cellNum=0; cellNum < row.getLastCellNum(); cellNum++){
                XSSFCell cell = row.getCell(cellNum);//获取列
                Object value = getCellValue(cell);
                sb.append(value).append("-");
            }
            System.out.println(sb.toString());
        }
    }
    public static Object getCellValue(Cell cell){
        //获取单元格的属性类型
        CellType cellType = cell.getCellType();
        //根据单元格数据类型获取数据
        Object value = null;
        switch (cellType){
            case STRING://字符串
                value = cell.getStringCellValue();
                break;
            case BOOLEAN://布尔
                value = cell.getBooleanCellValue();
                break;
            case NUMERIC:
                if (DateUtil.isCellDateFormatted(cell))//日期
                    value = cell.getDateCellValue();
                else //数字
                    value = cell.getNumericCellValue();
                break;
            case FORMULA://公式
                value = cell.getCellFormula();
                break;
            default:
                break;
        }
        return value;
    }
}
~~~

#### 2.2.3 计算公式
- 代码示例
~~~java
@Test
public void testFormula() throws Exception{
  InputStream is = new FileInputStream(path + "计算公式.xls");
  Workbook workbook = new HSSFWorkbook(is);
  Sheet sheet = workbook.getSheetAt(0);
  // 读取第五行第一列
  Row row = sheet.getRow(4);
  Cell cell = row.getCell(0);
  //公式计算器
  FormulaEvaluator formulaEvaluator = new HSSFFormulaEvaluator((HSSFWorkbook) workbook);
  // 输出单元内容
  int cellType = cell.getCellType();
  switch (cellType) {
    case Cell.CELL_TYPE_FORMULA://2
      //得到公式
      String formula = cell.getCellFormula();
      System.out.println(formula);
      
      CellValue evaluate = formulaEvaluator.evaluate(cell);
      //String cellValue = String.valueOf(evaluate.getNumberValue());
      String cellValue = evaluate.formatAsString();
      System.out.println(cellValue);
      break;
  }
}

~~~