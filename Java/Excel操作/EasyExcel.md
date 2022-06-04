# EasyExcel
## 1. EasyExcel简介
### 1.1 Excel简介
- Excel的两种版本
  - Excel2003
    - 底层采用特有的二进制格式,其核心结构是复合文档类型的结构,存储数据量小,一张表最大支持  65536行数据，256列
    - 文件后缀: xls
  - Excel2007
    - 其核心结构是XML类型的结构,采用的是基于XML的压缩方式,占用空间更小,操作效率更高
    - 文件后缀: xlsx

### 1.2 EasyExcel概述
- EasyExcel是是阿里巴巴的一个基于Java的简单、省内存的读写Excel的开源项目。在尽可能节约内存的情况下支持读写百M的Excel
- EasyExcel源码github地址:https://github.com/alibaba/easyexcel

### 1.3 EasyExcel性能
- Java解析、生成Excel比较有名的框架有Apache poi、jxl。但他们都存在一个严重的问题就是非常的耗内存，poi有一套SAX模式的API可以一定程度的解决一些内存溢出的问题，但POI还是有一些缺陷，比如07版Excel解压缩以及解压后存储都是在内存中完成的，内存消耗依然很大。easyexcel重写了poi对07版Excel的解析，一个3M的excel用POI sax解析依然需要100M左右内存，改用easyexcel可以降低到几M，并且再大的excel也不会出现内存溢出；03版依赖POI的sax模式，在上层做了模型转换的封装，让使用者更加简单方便
- 64M内存1分钟内读取75M(46W行25列)的Excel(当然还有急速模式能更快，但是内存占用会在100M多一点)
![20220604201537](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220604201537.png)
- EasyExcel能大大减少占用内存的主要原因是在解析Excel时没有将文件数据一次性全部加载到内存中，而是从磁盘上一行行读取数据，逐个解析。
- 下图是EasyExcel和POI在解析Excel时的对比图。
![20220604201652](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220604201652.png)
EasyExcel采用一行一行的解析模式，并将一行的解析结果以观察者的模式通知处理（AnalysisEventListener）
![20220604201727](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220604201727.png)

## 2. EasyExcel基本操作
- 当前案例采用EasyExcel版本 2.2.3
~~~java
<dependency>
	<groupId>com.alibaba</groupId>
	<artifactId>easyexcel</artifactId>
	<version>2.2.3</version>
</dependency>
~~~
### 2.1 EasyExcel导出操作
#### 2.1.1 编写实体类
- 代码示例
~~~java
/**
 * 创建User类,用于构建向Excel表格中写数据的类型;
 * @ExcelProperty:这个注解是EasyExcel提供,用于生成Excel表格头
 */
public class User {
	@ExcelProperty("用户编号")
	private Integer userId;
	@ExcelProperty("姓名")
	private String userName;
	@ExcelProperty("性别")
	private String gender;
	@ExcelProperty("工资")
	private Double salary;
	@ExcelProperty("入职时间")
	private Date hireDate;
	
	//setter/getter方法
}
~~~

#### 2.1.2 简单导出方式1
- 代码示例
~~~java
/**
 * 向Excel文档中写数据(方式一)
 * 最简单的写
 */
@Test
public void writeExcel1() {
	
	
	//创建文件保存的位置,以及文件名
	String fileName="user1.xlsx";
	
	/**
	 * 构建要写入的数据
	 * User类是一个自定义的特殊类,专门用来构建向Excel中写数据的类型类
	 * @ExcelProperty是easyexcel提供的注解,用来定义表格的头部
	 */
	List<User> data = new ArrayList<>();
	User user = new User(2001,"李雷","男",1000.123,new Date());
	data.add(user);
	//将数据写到Excel的第一个sheet标签中,并且给sheet标签起名字
	EasyExcel.write(fileName,User.class).sheet("用户信息").doWrite(data);
	//文件流会自动关闭
}
~~~

#### 2.1.3 简单导出方式2
- 代码示例
~~~java
/**
 * 向Excel文档中写数据(方式二)
 * 最简单的写
 */
@Test
public void writeExcel2() {
	//创建文件保存的位置,以及文件名
	String fileName="user2.xlsx";
	/**
	 * 构建要写入的数据
	 * User类是一个自定义的特殊类,专门用来构建向Excel中写数据的类型类
	 * @ExcelProperty是easyexcel提供的注解,用来定义表格的头部
	 */
	List<User> data = new ArrayList<>();
	User user = new User(2001,"李雷","男",1000.123,new Date());
	data.add(user);
	//创建Excel写对象
	ExcelWriter excelWriter = EasyExcel.write(fileName, User.class).build();
	//创建sheet对象
	WriteSheet writeSheet = EasyExcel.writerSheet("用户信息").build();
	//将数据写到sheet标签中
	excelWriter.write(data, writeSheet);
	//关闭流,文件流手动关闭
	excelWriter.finish();
}
~~~

#### 2.1.4 排除类中某些字段
- 代码示例
~~~java
/**
 * 向Excel文档中写数据
 * 将User类中的某几个属性字段排除
 */
@Test
public void writeExcel3() {
	//创建文件保存的位置,以及文件名
	String fileName="user3.xlsx";
	/**
	 * 构建要写入的数据
	 * User类是一个自定义的特殊类,专门用来构建向Excel中写数据的类型类
	 * @ExcelProperty是easyexcel提供的注解,用来定义表格的头部
	 */
	List<User> data = new ArrayList<>();
	User user = new User(2001,"李雷","男",1000.123,new Date());
	data.add(user);
	//构建要排除掉的列
	Set<String> excludeColumnFiledNames = new HashSet<>();
	//排除掉hireDate和userName字段
	excludeColumnFiledNames.add("hireDate");
	excludeColumnFiledNames.add("userName");
	EasyExcel.write(fileName, User.class).excludeColumnFiledNames(excludeColumnFiledNames).sheet("abc").doWrite(data);
}
~~~

#### 2.1.5 只导出类中某些字段
- 代码示例
~~~java
/**
 * 向Excel文档中写数据
 * 指定将User类中的某几个字段导出
 */
@Test
public void writeExcel4() {
	//创建文件保存的位置,以及文件名
	String fileName="user4.xlsx";
	/**
	 * 构建要写入的数据
	 * User类是一个自定义的特殊类,专门用来构建向Excel中写数据的类型类
	 * @ExcelProperty是easyexcel提供的注解,用来定义表格的头部
	 */
	List<User> data = new ArrayList<>();
	User user = new User(2001,"李雷","男",1000.123,new Date());
	data.add(user);
	//构建要导出的列
	Set<String> includeColumnFiledNames = new HashSet<>();
	//要导出hireDate和userName字段
	includeColumnFiledNames.add("hireDate");
	includeColumnFiledNames.add("userName");
	EasyExcel.write(fileName, User.class).includeColumnFiledNames(includeColumnFiledNames).sheet("abc").doWrite(data);
}
~~~

#### 2.1.6 插入指定的列
- 将Java对象中指定的属性,插入到Excel表格中的指定的列(在Excel中进行列排序)
- 编写Excel的Java对象类型
~~~java
public class IndexUser {
	/**
	 * value: 设置Excel中列头的名字 index: 设置Excel中将数据插入指定的列(列排序) eg: value="姓名" index=2 :
	 * 将姓名的列头设置在Excel表格的第三列
	 */
	@ExcelProperty(value = "用户编号", index = 0)
	private Integer userId;
	@ExcelProperty(value = "姓名", index = 2)
	private String userName;
	@ExcelProperty(value = "入职时间", index = 1)
	private Date hireDate;
}
~~~
- 向Excel中写入数据
~~~java
/**
 * 插入指定列
 * Java类中有三个属性
 * 我现在要将这三个属性的数据保存到Excel表格中
 * 并且自定义在Excel中的列顺序
 */
@Test
public void writeExcel5() {
	//创建文件保存的位置,以及文件名
	String fileName="user5.xlsx";
	/**
	 * 构建要写入的数据
	 */
	List<IndexUser> data = new ArrayList<>();
	IndexUser user = new IndexUser(1001,"枫桥夜泊",new Date());
	data.add(user);
	EasyExcel.write(fileName, IndexUser.class).sheet("abc").doWrite(data);
}
~~~

#### 2.1.7 复杂头数据写入
- 复杂头数据指有多级主题
- 编写Excel的Java对象类型
~~~java
/**
 * 构建复杂头信息数据的类
 */
public class ComplexHeadUser {
	/**
	 * {"用户主题1","用户编号"}这种遇到格式设置多级主题
	 * 第一个参数为一级主题(第一行)
	 * 第二个参数为二级主题(第二行)
	 * 如果一级主题名称相同,那么他们会合并单元格
	 */
	@ExcelProperty({"用户主题1","用户编号"})
	private Integer userId;
	@ExcelProperty({"用户主题2","用户名称"})
	private String userName;
	@ExcelProperty({"用户主题3","入职时间"})
	private Date hireDate;
}
~~~
- 向Excel中写数据
~~~java
/**
 * 写入复杂头信息到Excel
 */
@Test
public void writeExcel6() {
	//创建文件保存的位置,以及文件名
	String fileName="user6.xlsx";
	/**
	 * 构建要写入的数据
	 */
	List<ComplexHeadUser> data = new ArrayList<>();
	ComplexHeadUser user = new ComplexHeadUser(1001,"枫桥夜泊",new Date());
	data.add(user);
	EasyExcel.write(fileName, ComplexHeadUser.class).sheet("abc").doWrite(data);
}
~~~

#### 2.1.8 重复写到Excel的同一个sheet中
- 代码示例
~~~java
/**
 * 重复写到同一个sheet中
 */
@Test
public void writeExcel7() {
	//创建文件保存的位置,以及文件名
	String fileName="user7.xlsx";
	//构建数据
	List<User> data = new ArrayList<>();
	User user1 = new User(2001,"李雷1","男",1000.123,new Date());
	User user2 = new User(2002,"李雷2","男",1000.123,new Date());
	User user3 = new User(2003,"李雷3","男",1000.123,new Date());
	User user4 = new User(2004,"李雷4","男",1000.123,new Date());
	data.add(user1);
	data.add(user2);
	data.add(user3);
	data.add(user4);
	//创建ExcelWriter对对象并设置用哪个Class(数据类型)去写
	ExcelWriter excelWriter = EasyExcel.write(fileName, User.class).build();
	//创建sheet
	WriteSheet writeSheet = EasyExcel.writerSheet("用户信息").build();
	//模拟重复写入,模拟重复写入5次
	for (int i = 0; i < 5; i++) {
		excelWriter.write(data, writeSheet);
	}
	//关闭流
	excelWriter.finish();
}
~~~

#### 2.1.9 重复的写到Excel的不同sheet中
- 代码示例
~~~java
/**
 * 重复写到不同sheet中
 */
@Test
public void writeExcel8() {
	//创建文件保存的位置,以及文件名
	String fileName="user8.xlsx";
	//构建数据
	List<User> data = new ArrayList<>();
	User user1 = new User(2001,"李雷1","男",1000.123,new Date());
	User user2 = new User(2002,"李雷2","男",1000.123,new Date());
	User user3 = new User(2003,"李雷3","男",1000.123,new Date());
	User user4 = new User(2004,"李雷4","男",1000.123,new Date());
	data.add(user1);
	data.add(user2);
	data.add(user3);
	data.add(user4);
	//创建ExcelWriter对对象并设置用哪个Class(数据类型)去写
	ExcelWriter excelWriter = EasyExcel.write(fileName, User.class).build();
	
	//模拟重复写入,模拟重复写入5次
	for (int i = 0; i < 5; i++) {
		//创建sheet(因为写入到不同的sheet,所以需要多次创建sheet)
		//WriteSheet writeSheet = EasyExcel.writerSheet(i,"用户信息"+i).build(); //用下面的格式也可以
		WriteSheet writeSheet = EasyExcel.writerSheet("用户信息"+i).build();
		excelWriter.write(data, writeSheet);
	}
	//关闭流
	excelWriter.finish();
}
~~~

#### 2.1.10 日期、数字类型格式化
~~~java
对于日期和数字有时候我们需要对其展示的样式进行格式化,easyexcel提供了一下注解
@DateTimeFormat : 日期格式化
@NumberFormat   : 数字格式化(小数或者百分数)
~~~
- 编写Excel的Java对象类型
~~~java
/**
 * 创建User类,用于构建向Excel表格中写数据的类型;
 * @ExcelProperty:这个注解是EasyExcel提供,用于生成Excel表格头
 */
public class User {
	@ExcelProperty("用户编号")
	private Integer userId;
	@ExcelProperty("姓名")
	private String userName;
	@ExcelProperty("性别")
	private String gender;
	//格式化小数类型,如果是百分数那么定义为  #.##% 比如:9.12%
	@NumberFormat("#.##")
	@ExcelProperty("工资")
	private Double salary;
	//格式化时间
	@DateTimeFormat("yyyy年MM月dd日 HH时mm分ss秒")
	@ExcelProperty("入职时间")
	private Date hireDate;
}
~~~
- 向Excel中写数据
~~~java
/**
 * 日期、数字格式化
 */
@Test
public void writeExcel9() {
	//创建文件保存的位置,以及文件名
	String fileName="user9.xlsx";
	//构建数据
	List<User> data = new ArrayList<>();
	User user1 = new User(2001,"李雷1","男",1000.12223,new Date());
	User user2 = new User(2002,"李雷2","男",1000.12223,new Date());
	User user3 = new User(2003,"李雷3","男",1000.12223,new Date());
	User user4 = new User(2004,"李雷4","男",1000.13323,new Date());
	data.add(user1);
	data.add(user2);
	data.add(user3);
	data.add(user4);
	
	EasyExcel.write(fileName, User.class).sheet("用戶信息").doWrite(data);
}
~~~

#### 2.1.11 写入图片到Excel
- 编写Excel的Java对象类型
~~~java
/**
 * 构建向Excel中写图片的类 支持5种数据类型的图片类型 
 * 1. 图片可以是一个File格式 
 * 2. 图片可以是一个InputStream输入流的方式 
 * 3. 图片可以是一个byte[]数组的方式 
 * 4. 图片可以是 一个网络的java.net.URL对象方式 
 * 5. 图片也可以是一个String类型方式(当是String类型是，需要StringImageConverter类型转换器)
 */
@ContentRowHeight(100)  // 内容高度
@ColumnWidth(100 / 8)   // 列宽
public class ImageData {
	// 使用抽象文件表示一个图片
	private File file;
	// 使用输入流保存一个图片
	private InputStream inputStream;
	/**
	 * 当使用String类型保存一个图片的时候需要使用StringImageConverter转换器
	 */
	@ExcelProperty(converter = StringImageConverter.class)
	private String string;
	// 使用二进制数据保存为一个图片
	private byte[] byteArray;
	// 使用网络链接保存一个图片
	private URL url;
}
~~~

- 向Excel中写数据
~~~java
/**
 * 将图片写入到Excel表格中
 * EasyExcel提供了5种数据格式进行图片写入
 * @throws IOException 
 */
@Test
public void writeExcel10() throws IOException {
	//创建文件保存的位置,以及文件名
	String fileName="user10.xlsx";
	
	List<ImageData> list = new ArrayList<ImageData>();
	ImageData imageData = new ImageData();
	//根据File写入
	imageData.setFile(new File("imgs/img.png"));
	//根据InputStream写入
	imageData.setInputStream(new FileInputStream("imgs/img.png"));
	//根据String类型写入
	imageData.setString("imgs/img.png");
	/*
	 * 使用二进制方式写入步骤:
	 * 1. 获取文件大小
	 * 2. 读成二进制流
	 */
	File file = new File("imgs/img.png");
	//创建一个指定大小的二进制数组
	byte[] byteArray = new byte[(int)file.length()];
	FileInputStream in = new FileInputStream(file);
	//将文件流写入到二进制数组中
	in.read(byteArray, 0, (int)file.length());
	imageData.setByteArray(byteArray);
	//只用网络链接
	imageData.setUrl(new URL("https://pics5.baidu.com/feed/e850352ac65c10384dcf6805a7540815b17e890b.jpeg?token=46fd105a95d8754d3daf93de7e8a7f6f"));
	list.add(imageData);
	EasyExcel.write(fileName, ImageData.class).sheet().doWrite(list);
	//关闭输入流
	in.close();
}
~~~

#### 2.1.12 设置Excel的列宽行高
- 编写Excel的Java对象类型
~~~java
@ContentRowHeight(30) //设置内容高度
@HeadRowHeight(40)    //设置标题高度
@ColumnWidth(25)      //设置列宽
public class WidthAndHeightData {

	@ExcelProperty("字符串标题")
	private String string;
	@ExcelProperty("日期标题")
	private Date date;
	@ColumnWidth(50)   //设置列宽(可以修饰类,也可以修饰具体属性)
	@ExcelProperty("数字标题")
	private Double doubleData;
}
~~~

- 向Excel中写数据
~~~java
/**
 * 设置Excel的列宽行高
 */
@Test
public void writeExcel11(){
	//创建文件保存的位置,以及文件名
	String fileName="user11.xlsx";
	//构建数据
	List<WidthAndHeightData> list = new ArrayList<WidthAndHeightData>();
	WidthAndHeightData wahd = new WidthAndHeightData();
	wahd.setString("小明");
	wahd.setDoubleData(200.12);
	wahd.setDate(new Date());
	list.add(wahd);
	//写数据
	EasyExcel.write(fileName, WidthAndHeightData.class).sheet("信息").doWrite(list);
}
~~~

#### 2.1.13 设置写入Excel的样式
- 编写Excel的Java对象类型
~~~java
//头背景设置成红色 IndexedColors.RED.getIndex()
@HeadStyle(fillPatternType = FillPatternType.SOLID_FOREGROUND, fillForegroundColor = 10)
//头字体设置成20
@HeadFontStyle(fontHeightInPoints = 20)
//内容的背景设置成绿色 IndexedColors.GREEN.getIndex()
@ContentStyle(fillPatternType = FillPatternType.SOLID_FOREGROUND, fillForegroundColor = 17)
//内容字体设置成20
@ContentFontStyle(fontHeightInPoints = 20)
public class DemoStyleData {

	// 字符串的头背景设置成粉红 IndexedColors.PINK.getIndex()
	@HeadStyle(fillPatternType = FillPatternType.SOLID_FOREGROUND, fillForegroundColor = 14)
	// 字符串的头字体设置成20
	@HeadFontStyle(fontHeightInPoints = 30)
	// 字符串的内容的背景设置成天蓝 IndexedColors.SKY_BLUE.getIndex()
	@ContentStyle(fillPatternType = FillPatternType.SOLID_FOREGROUND, fillForegroundColor = 40)
	// 字符串的内容字体设置成20
	@ContentFontStyle(fontHeightInPoints = 30)
	@ExcelProperty("字符串标题")
	private String string;
	@ExcelProperty("日期标题")
	private Date date;
	@ExcelProperty("数字标题")
	private Double doubleData;
}
~~~

- 向Excel中写数据
~~~java
/**
 * 注解形式设置Excel样式
 */
@Test
public void writeExcel12(){
	//创建文件保存的位置,以及文件名
	String fileName="user12.xlsx";
	//构建数据
	List<DemoStyleData> list = new ArrayList<>();
	DemoStyleData dsld = new DemoStyleData();
	dsld.setDate(new Date());
	dsld.setDoubleData(200.12);
	dsld.setString("韓梅梅");
	list.add(dsld);
	EasyExcel.write(fileName, DemoStyleData.class).sheet("信息").doWrite(list);
}
~~~

#### 2.1.14 合并单元格
- 编写Excel的Java对象类型
~~~java
/*
 * @OnceAbsoluteMerge: 指定从哪一行开始，哪一行结束，哪一列开始，哪一列结束，进行合并单元格
 * firstRowIndex   : 起始行索引 从0开始
 * lastRowIndex    : 结束行索引
 * firstColumnIndex: 起始列索引 从0开始
 * lastColumnIndex : 结束列索引
 * 例如: 第2-3行,2-3列进行合并
 */
@OnceAbsoluteMerge(firstRowIndex = 1,lastRowIndex = 2,firstColumnIndex = 1,lastColumnIndex = 2)
public class DemoMergeData {
	//每隔两行合并一次(竖着合并单元格)
	//@ContentLoopMerge(eachRow = 2)
	@ExcelProperty("字符串标题")
	private String string;
	@ExcelProperty("日期标题")
	private Date date;
	@ExcelProperty("数字标题")
	private Double doubleData;
}
~~~

- 向Excel中写数据
~~~java
/**
 * 合并单元格
 */
@Test
public void writeExcel13(){
	//创建文件保存的位置,以及文件名
	String fileName="user13.xlsx";
	//构建数据
	List<DemoMergeData> list = new ArrayList<>();
	DemoMergeData dmd = new DemoMergeData();
	dmd.setDate(new Date());
	dmd.setDoubleData(200.12);
	dmd.setString("韓梅梅");
	DemoMergeData dmd2 = new DemoMergeData();
	dmd2.setDate(new Date());
	dmd2.setDoubleData(200.12);
	dmd2.setString("韓梅梅");
	DemoMergeData dmd3 = new DemoMergeData();
	dmd3.setDate(new Date());
	dmd3.setDoubleData(200.12);
	dmd3.setString("韓梅梅");
	DemoMergeData dmd4 = new DemoMergeData();
	dmd4.setDate(new Date());
	dmd4.setDoubleData(200.12);
	dmd4.setString("韓梅梅");
	DemoMergeData dmd5 = new DemoMergeData();
	dmd5.setDate(new Date());
	dmd5.setDoubleData(200.12);
	dmd5.setString("韓梅梅");
	list.add(dmd);
	list.add(dmd2);
	list.add(dmd3);
	list.add(dmd4);
	list.add(dmd5);
	EasyExcel.write(fileName, DemoMergeData.class).sheet("信息").doWrite(list);
}
~~~

### 2.2 EasyExcel导入操作
#### 2.2.1 简单导入方式
- Excel表格数据类型
![20220604215105](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220604215105.png)
- 编写Excel的Java对象类型
~~~java
/**
 * 承装Excel表格数据的类
 * 注意点: Java类中的属性的顺序和Excel中的列头的顺序是相同的
 * @Author 夜泊 
 * @BLOG   https://hd1611756908.github.io/
 */
public class DemoData {
    
	private String name;
	private Date hireDate;
	private Double salary;
}
~~~
- 读取Excel数据
~~~java
/*
 * 最简单的读取Excel内容(方式一)
 * 默认读取Excel文件中的第一个sheet
 */
@Test
public void testRead1() {
	//读取的文件路径
	String fileName="user11.xlsx";
	Class<DemoData> head = DemoData.class; //创建一个数据格式来承装读取到数据
	//读取数据
	EasyExcel.read(fileName, head, new AnalysisEventListener<DemoData>() {

		/**
		 * 解析每一条数据的时候被调用
		 */
		@Override
		public void invoke(DemoData data, AnalysisContext context) {
			//在这里操作，将解析的每一条数据保存到数据库中,在这里可以调用数据库
			System.out.println("解析的数据为: "+data);
		}

		/**
		 * 解析完所有数据的时候被调用
		 */
		@Override
		public void doAfterAllAnalysed(AnalysisContext context) {
			System.out.println("数据解析完成..........");
		}
	}).sheet().doRead();
	
}

/*
 * 最简单的读取Excel内容(方式二)
 * 默认读取Excel文件中的第一个sheet
 */
@Test
public void testRead2() {
	//读取的文件路径
	String fileName="user11.xlsx";
	Class<DemoData> head = DemoData.class; //创建一个数据格式来承装读取到数据
	//创建ExcelReader对象
	ExcelReader excelReader = EasyExcel.read(fileName, head, new AnalysisEventListener<DemoData>() {

		@Override
		public void invoke(DemoData data, AnalysisContext context) {
			System.out.println("读取到的数据为:"+data);
		}

		@Override
		public void doAfterAllAnalysed(AnalysisContext context) {
			System.out.println("数据解析已完成");
		}
	}).build();
	//创建sheet对象,并读取Excel的第1个sheet(下标从0开始)
	ReadSheet readSheet = EasyExcel.readSheet(0).build();
	excelReader.read(readSheet);
	//关闭流操作，在读取文件时会创建临时文件,如果不关闭,磁盘爆掉
	excelReader.finish();
}
~~~

#### 2.2.2 根据Excel中指定的列名或者列的下标读取指定列的数据
- 编写Excel的Java对象类型
~~~java
public class IndexOrNameData {
	/**
	 * 强制读取第三个 这里不建议 index 和 name 同时用，要么一个对象只用index，要么一个对象只用name去匹配
	 */
	@ExcelProperty(index = 2)
	private Double doubleData;
	/**
	 * 用名字去匹配，这里需要注意，如果名字重复，会导致只有一个字段读取到数据
	 */
	@ExcelProperty("字符串标题")
	private String string;
	@ExcelProperty("日期标题")
	private Date date;
}
~~~
- 读取Excel数据操作
~~~java
/*
 * 根据名称或者下标获取指定的Excel表格数据
 */
@Test
public void testRead3() {
	//读取的文件路径
	String fileName="user11.xlsx";
	Class<IndexOrNameData> head = IndexOrNameData.class; //创建一个数据格式来承装读取到数据
	//读取数据
	EasyExcel.read(fileName, head, new AnalysisEventListener<IndexOrNameData>() {

		/**
		 * 解析每一条数据的时候被调用
		 */
		@Override
		public void invoke(IndexOrNameData data, AnalysisContext context) {
			//在这里操作，将解析的每一条数据保存到数据库中,在这里可以调用数据库
			System.out.println("解析的数据为: "+data);
		}

		/**
		 * 解析完所有数据的时候被调用
		 */
		@Override
		public void doAfterAllAnalysed(AnalysisContext context) {
			System.out.println("数据解析完成..........");
		}
	}).sheet().doRead();
	
}
~~~

#### 2.2.3  格式化Excel中的数据格式
- 编写Excel的Java对象类型
~~~java
/**
 * 定义需要被格式化的字段
 * 如果使用@NumberFormat("#.##")注解，建议数据类型采用String,如果使用double可能不能被格式化
 * @Author 夜泊
 * @BLOG https://hd1611756908.github.io/
 */
public class ConverterData {

	@NumberFormat("#.#")
	@ExcelProperty("数字标题")
	private String salary;
	@DateTimeFormat("yyyy-MM-dd HH:mm:ss")
	@ExcelProperty("日期标题")
	private Date hireDate;
}
~~~

- 读取Excel数据操作
~~~java
/*
 * 格式化Excel中的数据格式(例如时间)
 */
@Test
public void testRead4() {
	//读取的文件路径
	String fileName="user11.xlsx";
	Class<ConverterData> head = ConverterData.class; //创建一个数据格式来承装读取到数据
	//读取数据
	EasyExcel.read(fileName, head, new AnalysisEventListener<ConverterData>() {

		/**
		 * 解析每一条数据的时候被调用
		 */
		@Override
		public void invoke(ConverterData data, AnalysisContext context) {
			//在这里操作，将解析的每一条数据保存到数据库中,在这里可以调用数据库
			System.out.println("解析的数据为: "+data);
		}

		/**
		 * 解析完所有数据的时候被调用
		 */
		@Override
		public void doAfterAllAnalysed(AnalysisContext context) {
			System.out.println("数据解析完成..........");
		}
	}).sheet().doRead();
}
~~~

#### 2.2.4 读取多个或者全部的Sheet
- 编写Excel的Java对象类型
~~~java
/**
 * 承装Excel表格数据的类
 * 注意点: Java类中的属性的顺序和Excel中的列头的顺序是相同的
 */
public class DemoData {

	private String name;
	private Date hireDate;
	private Double salary;
}
~~~

- 读取Excel数据操作
~~~java
/*
 * 读取Excel中的多个或者全部sheet
 */
@Test
public void testRead5() {
	//读取的文件路径
	String fileName="user11.xlsx";
	Class<DemoData> head = DemoData.class; //创建一个数据格式来承装读取到数据
	/**
	 * 读取全部sheet
	 */
	EasyExcel.read(fileName, head, new AnalysisEventListener<DemoData>() {

		@Override
		public void invoke(DemoData data, AnalysisContext context) {
			// 每解析一条数据被调用一次
			System.out.println("解析的数据为: "+data);
		}

		@Override
		public void doAfterAllAnalysed(AnalysisContext context) {
			// 数据解析完成之后被调用
			System.out.println("数据解析完成......");
		}
	}).doReadAll();
	
	System.out.println("============================================================");
	
	/**
	 * 读取其中的某几个sheet
	 */
	//构建ExcelReader对象
	ExcelReader excelReader = EasyExcel.read(fileName).build();
	//创建想要获取的sheet对象
	ReadSheet sheet1 = EasyExcel.readSheet(0).head(DemoData.class).registerReadListener(new AnalysisEventListener<DemoData>() {

		@Override
		public void invoke(DemoData data, AnalysisContext context) {
			// 每解析一条数据被调用一次
			System.out.println("解析的数据为: "+data);				
		}

		@Override
		public void doAfterAllAnalysed(AnalysisContext context) {
			// 数据解析完成之后被调用
			System.out.println("数据解析完成......");				
		}
	}).build();
	
	ReadSheet sheet2 = EasyExcel.readSheet(2).head(DemoData.class).registerReadListener(new AnalysisEventListener<DemoData>() {

		@Override
		public void invoke(DemoData data, AnalysisContext context) {
			// 每解析一条数据被调用一次
			System.out.println("解析的数据为: "+data);				
		}

		@Override
		public void doAfterAllAnalysed(AnalysisContext context) {
			// 数据解析完成之后被调用
			System.out.println("数据解析完成......");				
		}
	}).build();
	//读取sheet
	excelReader.read(sheet1,sheet2);
	//关闭
	excelReader.finish();
}
~~~

### 2.3 填充Excel
#### 2.3.1 简单填充
- 创建Excel模板格式
  ~~~
  填充单个属性使用{}作为占位符,在大括号里面定义属性名称,如果{}想不作为占位符展示出来,可以使用反斜杠进行转义.
  ~~~
  ![20220604215837](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220604215837.png)

- 填充数据的Java类
~~~java
/**
 * 简单的单个对象填充
 */
@Test
public void excelFull() {
	//读取模板文件
	String templateFileName="templateFileName.xlsx";
	//通过模板文件生成的填充之后的文件
	String fullFile="fullFile.xlsx";
	//构建要填充的数据
	FillData data = new FillData();
	data.setName("张三");
	data.setNumber(5.2);
	//填充操作
	EasyExcel.write(fullFile).withTemplate(templateFileName).sheet().doFill(data);
}
~~~
- 填充之后的效果
  ![20220604215939](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220604215939.png)

#### 2.3.2 列表填充
- 创建Excel模板格式
  ~~~
  当占位符是一个列表时,{}里面的属性前面要加一个点代表多个
  ~~~
![20220604220035](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220604220035.png)

- 填充数据的Java类
~~~java
public class FillData {
	private String name;
    private double number;
}
~~~

- 填充的代码
~~~java
/**
 * 简单的列表对象填充
 */
@Test
public void excelFull2() {
	//读取模板文件
	String templateFileName="templateFileName2.xlsx";
	//通过模板文件生成的填充之后的文件
	String fullFile="fullFile2.xlsx";
	//构建要填充的数据列表
	List<FillData> list = new ArrayList<FillData>();
	FillData data1 = new FillData();
	data1.setName("张三");
	data1.setNumber(5.2);
	FillData data2 = new FillData();
	data2.setName("张三");
	data2.setNumber(5.2);
	list.add(data1);
	list.add(data2);
	//填充操作
	EasyExcel.write(fullFile).withTemplate(templateFileName).sheet().doFill(list);
}
~~~

- 填充之后的效果
  ![20220604220144](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220604220144.png)

### 2.5 Web操作(Excel上传/下载)
#### 2.5.1 Excel下载
- HTML
~~~html
<a href="${pageContext.request.contextPath }/DownLoadExcel">下载</a>
~~~

- 封装数据的Java类
~~~java
public class User {

	@ExcelProperty("用户编号")
	private Integer userId;
	@ExcelProperty("用户名称")
	private String userName;
	@ExcelProperty("用户性别")
	private String gender;
	@ExcelProperty("创建时间")
	private Date createTime;
}
~~~

- 下载操作
~~~java
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	/**
	 * EasyExcel下载步骤
	 */
	//设置响应头
	response.setContentType("application/vnd.ms-excel");
	response.setCharacterEncoding("utf-8");
	//设置防止文件名中文乱码
	String fileName = URLEncoder.encode("中文文件名","utf-8");
	response.setHeader("Content-disposition", "attachment;filename="+ fileName + ".xlsx");
	//构建写入到Excel中的数据(此数据可以从数据库中获取)
	List<User> users = new ArrayList<>();
	User user1 = new User(1001, "李雷1", "男", new Date());
	User user2 = new User(1002, "李雷2", "男", new Date());
	User user3 = new User(1003, "李雷3", "男", new Date());
	users.add(user1);
	users.add(user2);
	users.add(user3);
	EasyExcel.write(response.getOutputStream(),User.class).sheet("用户信息").doWrite(users);
}
~~~


#### 2.5.2 Excel上传
- 前端代码
~~~html
<form action="${pageContext.request.contextPath }/UploadExcelServlet" method="post" enctype="multipart/form-data">
	<input type="file" name="file">
	<input type="submit" value="上传">
</form>
~~~

- 接收从Excel中获取到的数据
~~~java
public class User {

	@ExcelProperty("用户编号")
	private Integer userId;
	@ExcelProperty("用户名称")
	private String userName;
	@ExcelProperty("用户性别")
	private String gender;
	@ExcelProperty("创建时间")
	private Date createTime;
}
~~~

- 上传操作
~~~java
@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
	
	DiskFileItemFactory factory = new DiskFileItemFactory();
	ServletFileUpload fileUpload = new ServletFileUpload(factory);
	//设置单个文件为3M
	fileUpload.setFileSizeMax(1024*1024*3);
	//总文件大小为30M
	fileUpload.setSizeMax(1024*1024*3*10);
	
	try {
		List<FileItem> list = fileUpload.parseRequest(req);
		
		for (FileItem fileItem : list) {
			
			//判断是否为附件
			if(!fileItem.isFormField()) {
				//是附件
				InputStream inputStream = fileItem.getInputStream();
				EasyExcel.read(inputStream,User.class,new AnalysisEventListener<User>() {

					@Override
					public void doAfterAllAnalysed(AnalysisContext arg0) {
						System.out.println("Excel全部读完被执行......");
					}

					@Override
					public void invoke(User data, AnalysisContext arg1) {
						//读完一行就执行一次(调用数据库进行插入操作)
						System.out.println("解析一行: "+data);
					}
				}).sheet().doRead();
			}else {
				//普通表单
			}
		}
	} catch (FileUploadException e) {
		e.printStackTrace();
	}
	//上传完成进行转发或者重定向
}
~~~