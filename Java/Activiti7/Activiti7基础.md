# Activiti7基础
## 1. Activiti7简介
### 1.1.1 工作流概念
- 工作流(Workflow)，就是通过计算机对业务流程自动化执行管理。它主要解决的是“使在多个参与者之间按照某种预定义的规则自动进行传递文档、信息或任务的过程，从而实现某个预期的业务目标，或者促使此目标的实现”。
- 一个软件系统中具有工作流的功能，我们把它称为工作流系统，一个系统中工作流的功能是什么？就是对系统的业务流程进行自动化管理，所以工作流是建立在业务流程的基础上，所以一个软件的系统核心根本上还是系统的业务流程，工作流只是协助进行业务流程管理。即使没有工作流业务系统也可以开发运行，只不过有了工作流可以更好的管理业务流程，提高系统的可扩展性。

### 1.1.2 工作流应用
- 应用行业: 消费品行业，制造业，电信服务业，银证险等金融服务业，物流服务业，物业服务业，物业管理，大中型进出口贸易公司，政府事业机构，研究院所及教育服务业等，特别是大的跨国企业和集团公司。

- 具体应用:  
 1. 关键业务流程：订单、报价处理、合同审核、客户电话处理、供应链管理等
 2. 行政管理类:出差申请、加班申请、请假申请、用车申请、各种办公用品申请、购买申请、日报周报等凡是原来手工流转处理的行政表单。
 3. 人事管理类：员工培训安排、绩效考评、职位变动处理、员工档案信息管理等。
 4. 财务相关类：付款请求、应收款处理、日常报销处理、出差报销、预算和计划申请等。
 5. 客户服务类：客户信息管理、客户投诉、请求处理、售后服务管理等。
 6. 特殊服务类：ISO系列对应流程、质量管理对应流程、产品数据信息管理、贸易公司报关处理、物流公司货物跟踪处理等各种通过表单逐步手工流转完成的任务均可应用工作流软件自动规范地实施。

### 1.1.3 工作流实现
- 在没有专门的工作流引擎之前，我们之前为了实现流程控制，通常的做法就是采用状态字段的值来跟踪流程的变化情况。这样不用角色的用户，通过状态字段的取值来决定记录是否显示。
- 针对有权限可以查看的记录，当前用户根据自己的角色来决定审批是否合格的操作。如果合格将状态字段设置一个值，来代表合格；当然如果不合格也需要设置一个值来代表不合格的情况。
- 这是一种最为原始的方式。通过状态字段虽然做到了流程控制，但是当我们的流程发生变更的时候，这种方式所编写的代码也要进行调整。
- 那么有没有专业的方式来实现工作流的管理呢？并且可以做到业务流程变化之后，我们的程序可以不用改变，如果可以实现这样的效果，那么我们的业务系统的适应能力就得到了极大提升。

### 1.2.1 Activiti工作流概述
- Alfresco软件在2010年5月17日宣布Activiti业务流程管理（BPM）开源项目的正式启动，其首席架构师由业务流程管理BPM的专家 Tom Baeyens担任，Tom Baeyens就是原来jbpm的架构师，而jbpm是一个非常有名的工作流引擎，当然activiti也是一个工作流引擎。
- Activiti是一个工作流引擎， activiti可以将业务系统中复杂的业务流程抽取出来，使用专门的建模语言BPMN2.0进行定义，业务流程按照预先定义的流程进行执行，实现了系统的流程由activiti进行管理，减少业务系统由于流程变更进行系统升级改造的工作量，从而提高系统的健壮性，同时也减少了系统开发维护成本。
- [官方网站](https://www.activiti.org/)
  
### 1.2.2 BPM
- BPM（Business Process Management），即业务流程管理，是一种规范化的构造端到端的业务流程，以持续的提高组织业务效率。常见商业管理教育如EMBA、MBA等均将BPM包含在内。

- BPM软件: 
  1. BPM软件就是根据企业中业务环境的变化，推进人与人之间、人与系统之间以及系统与系统之间的整合及调整的经营方法与解决方案的IT工具。
  2. 通过BPM软件对企业内部及外部的业务流程的整个生命周期进行建模、自动化、管理监控和优化，使企业成本降低，利润得以大幅提升。
  3. BPM软件在企业中应用领域广泛，凡是有业务流程的地方都可以BPM软件进行管理，比如企业人事办公管理、采购流程管理、公文审批流程管理、财务管理等。
   
### 1.2.3 BPMN
- BPMN（Business Process Model AndNotation）- 业务流程模型和符号 是由BPMI（BusinessProcess ManagementInitiative）开发的一套标准的业务流程建模符号，使用BPMN提供的符号可以创建业务流程。
- 2004年5月发布了BPMN1.0规范.BPMI于2005年9月并入OMG（The Object Management Group对象管理组织)组织。OMG于2011年1月发布BPMN2.0的最终版本。
- BPMN 是目前被各 BPM 厂商广泛接受的 BPM 标准。Activiti 就是使用 BPMN 2.0 进行流程建模、流程执行管理，它包括很多的建模符号
  
## 2. Activiti7表设计
### 2.1 数据库环境
- activiti运行需要有数据库的支持，支持的数据库有：h2, mysql, oracle, postgres, mssql, db2。
- activiti 支持的数据库和版本如下:  

  ![20220504150653](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504150653.png)

### 2.2 25张表介绍
- Activiti运行需要25张表
- 表命名规则:
  - 第一部分ACT表示是Activiti工作流需要的表,第二部分是表示表的用途的两个字母标识。 用途也和服务的 API 对应。
  1. act_hi_*：'hi’表示 history，此前缀的表包含历史数据，如历史(结束)流程实例，变量，任务等等。
  2. act_ge_*：'ge’表示 general，此前缀的表为通用数据，用于不同场景中。
  3. act_evt_*：'evt’表示 event，此前缀的表为事件日志。
  4. act_procdef_*：'procdef’表示 processdefine，此前缀的表为记录流程定义信息。
  5. act_re_*：'re’表示 repository，此前缀的表包含了流程定义和流程静态资源(图片，规则等等)。
  6. act_ru_*：'ru’表示 runtime，此前缀的表是记录运行时的数据，包含流程实例，任务，变量，异步任务等运行中的数据。Activiti只在流程实例执行过程中保存这些数据，在流程结束时就会删除这些记录


- 表具体作用介绍: 
  - 通用数据(act_ge_)
   ![20220504165011](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504165011.png)

  - 流程定义表(act_re_)
    ![20220504165044](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504165044.png)

   - 运行实例表(act_ru_)
    ![20220504165120](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504165120.png)

   - 历史流程表(act_hi_)
    ![20220504165152](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504165152.png)

   - 其他表
    ![20220504165225](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504165225.png)



## 3. Activiti7基本操作
### 3.1 BPMN流程符号
- BPMN 2.0 流程符号主要分为下列四种：
  1. 流对象
  2. 数据
  3. 流向
  4. 泳道

- 流对象（Flow Objects）：是定义业务流程的主要图形元素，包括三种：事件、活动、网关
  -  事件（Events）：指的是在业务流程的运行过程中发生的事情，分为：
     - 开始：表示一个流程的开始  
     - 中间：发生的开始和结束事件之间，影响处理的流程
     - 结束：表示该过程结束
   ![20220504185412](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504185412.png)

  - 活动（Activities）：包括任务和子流程两类。子流程在图形的下方中间外加一个小加号（+）来区分。
    ![20220504211728](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504211728.png)

  - 网关（Gateways）：用于表示流程的分支与合并。
     - 排他网关：只有一条路径会被选择
     - 并行网关：所有路径会被同时选择
     - 包容网关：可以同时执行多条线路，也可以在网关上设置条件
     - 事件网关：专门为中间捕获事件设置的，允许设置多个输出流指向多个不同的中间捕获事件。当流程执行到事件网关后，流程处于等待状态，需要等待抛出事件才能将等待状态转换为活动状态。
     ![20220504185616](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504185616.png)

- 数据（Data）：数据主要通过四种元素表示
   - 数据对象（Data Objects）
   - 数据输入（Data Inputs）
   - 数据输出（Data Outputs）
   - 数据存储（Data Stores）

- 流向 Flow ：流对象彼此互相连接或者连接到其他信息的方法主要有三种
   - 顺序流：用一个带实心箭头的实心线表示，用于指定活动执行的顺序
   - 信息流：用一条带箭头的虚线表示，用于描述两个独立的业务参与者（业务实体/业务角色）之间发送和接受的消息流动
   - 关联：用一根带有线箭头的点线表示，用于将相关的数据、文本和其他人工信息与流对象联系起来。用于展示活动的输入和输出

![20220504211827](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504211827.png)

- 泳道（Swimlanes）：通过泳道对主要的建模元素进行分组，将活动划分到不同的可视化类别中来描述由不同的参与者的责任与职责。

- BPMN实例
   - 实例1：拍卖服务BPMN模板                 
     ![20220504190000](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504190000.png)

   - 实例2：书籍销售流程 BPMN             
     ![20220504190046](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504190046.png)

### 3.2 流程设计器
- 本文使用的是IDEA的插件actiBPM,因为该插件停止维护,只有IDEA2019版之前版本能使用该插件
- Eclipse可以使用插件 activity-designer
- actiBPM的简单使用

1. 新建流程(IDEA工具)
  首先选中存放图形的目录(选择resources下的bpmn目录)，点击菜单：New  -> BpmnFile，如图：         

![20220504212558](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504212558.png)

  弹出如下图所示框，输入evection 表示 出差审批流程：     

![20220504212620](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504212620.png)

  起完名字evection后（默认扩展名为bpmn），就可以看到流程设计页面，如图所示：

![20220504212641](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504212641.png)

 左侧区域是绘图区，右侧区域是palette画板区域
 鼠标先点击画板的元素即可在左侧绘图

2. 绘制流程
  使用滑板来绘制流程，通过从右侧把图标拖拽到左侧的画板，最终效果如下：

![20220504212733](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504212733.png)

3. 指定流程定义Key
流程定义key即流程定义的标识，通过properties视图查看流程的key

![20220504212805](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504212805.png)

4. 指定任务负责人
在properties视图指定每个任务结点的负责人，如：填写出差申请的负责人为 zhangsan

![20220504212829](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504212829.png)

5. 生成.png图片文件
   
   ① 修改文件后缀为xml
     首先将evection.bpmn文件改名为evection.xml，如下图：

![20220504213631](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504213631.png)
  
   ② 使用designer设计器打开.xml文件
    在evection.xml文件上面，点右键并选择Diagrams菜单，再选择Show BPMN2.0 Designer…

![20220504213829](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504213829.png)


6. 解决中文乱码
   
   ① 打开Settings，找到File Encodings，把encoding的选项都选择UTF-8

   ![20220504214059](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504214059.png)

   ② 打开IDEA安装路径，找到如下的安装目录

   ![20220504214132](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504214132.png)

   根据自己所安装的版本来决定，我使用的是64位的idea，所以在idea64.exe.vmoptions文件的最后一行追加一条命令： -Dfile.encoding=UTF-8 
   一定注意，不要有空格，否则重启IDEA时会打不开，然后 重启IDEA。

   ![20220504214155](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504214155.png)

   ③ 如果以上方法已经做完，还出现乱码，就再修改一个文件，并在文件的末尾添加： -Dfile.encoding=UTF-8，然后重启idea

   ![20220504214218](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504214218.png)

7. 导出为图片文件
   点击Export To File的小图标，打开如下窗口，注意填写文件名及扩展名，选择好保存图片的位置：

![20220504214248](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504214248.png)

   然后，我们把png文件拷贝到resources下的bpmn目录，并且把evection.xml改名为evection.bpmn。

### 3.3 Activiti工作流服务介绍 
![20220504202347](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504202347.png)

- RepositoryService
  - 是activiti的资源管理类，提供了管理和控制流程发布包和流程定义的操作。使用工作流建模工具设计的业务流程图需要使用此service将流程定义文件的内容部署到计算机。
  - 除了部署流程定义以外还可以：查询引擎中的发布包和流程定义。
  - 暂停或激活发布包，对应全部和特定流程定义。 暂停意味着它们不能再执行任何操作了，激活是对应的反向操作。获得多种资源，像是包含在发布包里的文件， 或引擎自动生成的流程图。
  - 获得流程定义的pojo版本， 可以用来通过java解析流程，而不必通过xml。

- RuntimeService
  - Activiti的流程运行管理类。可以从这个服务类中获取很多关于流程执行相关的信息

- TaskService
  - Activiti的任务管理类。可以从这个类中获取任务的信息。

- HistoryService
  - Activiti的历史管理类，可以查询历史信息，执行流程时，引擎会保存很多数据（根据配置），比如流程实例启动时间，任务的参与者， 完成任务的时间，每个流程实例的执行路径，等等。 这个服务主要通过查询功能来获得这些数据。

- ManagementService
  - Activiti的引擎管理类，提供了对 Activiti 流程引擎的管理和维护功能，这些功能不在工作流驱动的应用程序中使用，主要用于 Activiti 系统的日常维护。
</br>
- 类关系图
  ![20220504211027](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504211027.png)
**在新版本中，IdentityService，FormService两个Serivce都已经删除了。**

### 3.4 Activiti执行流程
- 部署activiti
  - Activiti是一个工作流引擎（其实就是一堆jar包API），业务系统访问(操作)activiti的接口，就可以方便的操作流程相关数据，这样就可以把工作流环境与业务系统的环境集成在一起。

- 流程定义
  - 使用activiti流程建模工具(activity-designer)定义业务流程(.bpmn文件) 。
  - .bpmn文件就是业务流程定义文件，通过xml定义业务流程。

- 流程定义部署
  - activiti部署业务流程定义（.bpmn文件）。
  - 使用activiti提供的api把流程定义内容存储起来，在Activiti执行过程中可以查询定义的内容
  - Activiti执行把流程定义内容存储在数据库中

- 启动流程实例
  - 流程实例也叫：ProcessInstance
  - 启动一个流程实例表示开始一次业务流程的运行。
  - 在员工请假流程定义部署完成后，如果张三要请假就可以启动一个流程实例，如果李四要请假也启动一个流程实例，
  - 两个流程的执行互相不影响。

- 用户查询待办任务(Task)
  - 因为现在系统的业务流程已经交给activiti管理，通过activiti就可以查询当前流程执行到哪了，当前用户需要办理什么任务了，这些activiti帮我们管理了，而不需要开发人员自己编写在sql语句查询。

- 用户办理任务
  - 用户查询待办任务后，就可以办理某个任务，如果这个任务办理完成还需要其它用户办理，比如采购单创建后由部门经理审核，这个过程也是由activiti帮我们完成了。

- 流程结束
  - 当任务办理完成没有下一个任务结点了，这个流程实例就完成了。

### 3.5 搭建项目环境
- 开发环境
  - 编译器: Jdk1.8或以上版本
  - 数据库: Mysql 5及以上的版本 
  - 服务器: Tomcat8.5 
  - IDE: IDEA

- POM文件 依赖
  1. activiti-engine-7.0.0.beta1.jar
  2. activiti 依赖的 jar 包： mybatis、 alf4j、 log4j 等
  3. activiti 依赖的 spring 包
  4. Mysql数据库驱动
  5. 第三方数据连接池 dbcp
  6. 单元测试 Junit-4.12.jar

~~~xml
<properties>
    <slf4j.version>1.6.6</slf4j.version>
    <log4j.version>1.2.12</log4j.version>
    <activiti.version>7.0.0.Beta1</activiti.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.activiti</groupId>
        <artifactId>activiti-engine</artifactId>
        <version>${activiti.version}</version>
    </dependency>
    <dependency>
        <groupId>org.activiti</groupId>
        <artifactId>activiti-spring</artifactId>
        <version>${activiti.version}</version>
    </dependency>
    <!-- bpmn 模型处理 -->
    <dependency>
        <groupId>org.activiti</groupId>
        <artifactId>activiti-bpmn-model</artifactId>
        <version>${activiti.version}</version>
    </dependency>
    <!-- bpmn 转换 -->
    <dependency>
        <groupId>org.activiti</groupId>
        <artifactId>activiti-bpmn-converter</artifactId>
        <version>${activiti.version}</version>
    </dependency>
    <!-- bpmn json数据转换 -->
    <dependency>
        <groupId>org.activiti</groupId>
        <artifactId>activiti-json-converter</artifactId>
        <version>${activiti.version}</version>
    </dependency>
    <!-- bpmn 布局 -->
    <dependency>
        <groupId>org.activiti</groupId>
        <artifactId>activiti-bpmn-layout</artifactId>
        <version>${activiti.version}</version>
    </dependency>
    <!-- activiti 云支持 -->
    <dependency>
        <groupId>org.activiti.cloud</groupId>
        <artifactId>activiti-cloud-services-api</artifactId>
        <version>${activiti.version}</version>
    </dependency>
    <!-- mysql驱动 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.40</version>
    </dependency>
    <!-- mybatis -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.4.5</version>
    </dependency>
    <!-- 链接池 -->
    <dependency>
        <groupId>commons-dbcp</groupId>
        <artifactId>commons-dbcp</artifactId>
        <version>1.4</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
    <!-- log start -->
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>${log4j.version}</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>${slf4j.version}</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>${slf4j.version}</version>
    </dependency>
</dependencies>
~~~

- 添加log4j日志配置
```properties
# Set root category priority to INFO and its only appender to CONSOLE.
#log4j.rootCategory=INFO, CONSOLE debug info warn error fatal
log4j.rootCategory=debug, CONSOLE, LOGFILE
# Set the enterprise logger category to FATAL and its only appender to CONSOLE.
log4j.logger.org.apache.axis.enterprise=FATAL, CONSOLE
# CONSOLE is set to be a ConsoleAppender using a PatternLayout.
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.layout.ConversionPattern=%d{ISO8601} %-6r[%15.15t] %-5p %30.30c %x - %m\n
# LOGFILE is set to be a File appender using a PatternLayout.
log4j.appender.LOGFILE=org.apache.log4j.FileAppender
log4j.appender.LOGFILE.File=f:\act\activiti.log
log4j.appender.LOGFILE.Append=true
log4j.appender.LOGFILE.layout=org.apache.log4j.PatternLayout
log4j.appender.LOGFILE.layout.ConversionPattern=%d{ISO8601} %-6r[%15.15t] %-5p %30.30c %x - %m\n
```

- 添加Activiti配置文件
  默认方式的要求是在 resources 下创建 activiti.cfg.xml 文件，注意：默认方式目录和文件名不能修改，因为activiti的源码中已经设置，到固定的目录读取固定文件名的文件。
  ~~~xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:tx="http://www.springframework.org/schema/tx"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
                    http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/contex
  http://www.springframework.org/schema/context/spring-context.xsd
  http://www.springframework.org/schema/tx
  http://www.springframework.org/schema/tx/spring-tx.xsd">
  </beans>
  ~~~
  **两种配置方式式：一种是单独配置数据源，一种是不单独配置数据源***
  - 方式1 直接配置processEngineConfiguration
    processEngineConfiguration 用来创建 ProcessEngine，在创建 ProcessEngine 时会执行数据库的操作。
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:tx="http://www.springframework.org/schema/tx"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
                              http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/contex
   http://www.springframework.org/schema/context/spring-context.xsd
   http://www.springframework.org/schema/tx
   http://www.springframework.org/schema/tx/spring-tx.xsd">
      <!-- 默认id对应的值 为processEngineConfiguration -->
      <!-- processEngine Activiti的流程引擎 -->
      <bean id="processEngineConfiguration"
            class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">
           <property name="jdbcDriver" value="com.mysql.jdbc.Driver"/>
           <property name="jdbcUrl" value="jdbc:mysql:///activiti"/>
           <property name="jdbcUsername" value="root"/>
           <property name="jdbcPassword" value="123456"/>
           <!-- activiti数据库表处理策略 -->
           <property name="databaseSchemaUpdate" value="true"/>
       </bean>
   </beans>
  ```
  - 方式2 配置数据源后，在processEngineConfiguration 引用
  ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:tx="http://www.springframework.org/schema/tx"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
                         http://www.springframework.org/schema/beans/spring-beans.xsd
   http://www.springframework.org/schema/contex
   http://www.springframework.org/schema/context/spring-context.xsd
   http://www.springframework.org/schema/tx
   http://www.springframework.org/schema/tx/spring-tx.xsd">

       <!-- 这里可以使用 链接池 dbcp-->
       <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
           <property name="driverClassName" value="com.mysql.jdbc.Driver" />
           <property name="url" value="jdbc:mysql:///activiti" />
           <property name="username" value="root" />
           <property name="password" value="123456" />
           <property name="maxActive" value="3" />
           <property name="maxIdle" value="1" />
       </bean>

       <bean id="processEngineConfiguration"
             class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">
           <!-- 引用数据源 上面已经设置好了-->
           <property name="dataSource" ref="dataSource" />
           <!-- activiti数据库表处理策略 -->
           <property name="databaseSchemaUpdate" value="true"/>
      </bean>
   </beans>
  ```

- Java程序生成表结构
  创建一个工具类，调用Activiti的工具类来生成activiti需要的表结构
~~~java
public class Test01 {
/**
* 生成Activiti的相关的表结构
*/
   @Test
   public void test01(){
      // 使用classpath下的activiti.cfg.xml中的配置来创建 ProcessEngine对象
      ProcessEngine engine = ProcessEngines.getDefaultProcessEngine();
      System.out.println(engine);
   }
}
~~~
1. 运行以上程序段即可完成 activiti 表创建，通过改变 activiti.cfg.xml 中databaseSchemaUpdate 参数的值执行不同的数据表处理策略。
2. 上 边 的 方法 getDefaultProcessEngine方法在执行时，从activiti.cfg.xml 中找固定的名称
processEngineConfiguration 。

### 3.6 创建工作流引擎
- 前面使用的是getDefaultProcessEngine()方法来加载classpath下的 activiti.cfg.xml文件，有些情况
下我们可能没有按照默认的方式来处理,此时我们可以用如下方法
~~~java
/**
* 自定义的方式来加载配置文件
*/
@Test
public void test02(){
  // 首先创建ProcessEngineConfiguration对象
  ProcessEngineConfiguration configuration =
  ProcessEngineConfiguration.createProcessEngineConfigurationFromResource("activiti.cfg.xml");
  // 通过ProcessEngineConfiguration对象来创建 ProcessEngine对象
  ProcessEngine processEngine = configuration.buildProcessEngine();
}
~~~
上边的代码要求activiti.cfg.xml中必须有一个processEngineConfiguration的bean

### 3.7 部署流程
- 将上面在设计器中定义的流程部署到activiti数据库中，就是流程定义部署。
- 通过调用activiti的api将流程定义的bpmn和png两个文件一个一个添加部署到activiti中，也可以将两个文件打成zip包进行部署。
- 单个文件部署方式
  - 分别将bpmn文件和png图片文件部署。
~~~java
package com.itheima.test;

import org.activiti.engine.ProcessEngine;
import org.activiti.engine.ProcessEngines;
import org.activiti.engine.RepositoryService;
import org.activiti.engine.repository.Deployment;
import org.junit.Test;

public class ActivitiDemo {
    /**
     * 部署流程定义
     */
    @Test
    public void testDeployment(){
//        1、创建ProcessEngine
        ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
//        2、得到RepositoryService实例
        RepositoryService repositoryService = processEngine.getRepositoryService();
//        3、使用RepositoryService进行部署
        Deployment deployment = repositoryService.createDeployment()
                .addClasspathResource("bpmn/evection.bpmn") // 添加bpmn资源
                .addClasspathResource("bpmn/evection.png")  // 添加png资源
                .name("出差申请流程")
                .deploy();
//        4、输出部署信息
        System.out.println("流程部署id：" + deployment.getId());
        System.out.println("流程部署名称：" + deployment.getName());
    }
}

~~~
- 压缩包部署方式
  - 将evection.bpmn和evection.png压缩成zip包。
```java
@Test
	public void deployProcessByZip() {
		// 定义zip输入流
		InputStream inputStream = this
				.getClass()
				.getClassLoader()
				.getResourceAsStream(
						"bpmn/evection.zip");
		ZipInputStream zipInputStream = new ZipInputStream(inputStream);
		// 获取repositoryService
		RepositoryService repositoryService = processEngine
				.getRepositoryService();
		// 流程部署
		Deployment deployment = repositoryService.createDeployment()
				.addZipInputStream(zipInputStream)
				.deploy();
		System.out.println("流程部署id：" + deployment.getId());
		System.out.println("流程部署名称：" + deployment.getName());
	}

```

- 操作影响的表
  - 流程定义部署后操作activiti的3张表如下：
  act_re_deployment     流程定义部署表，每部署一次增加一条记录
  act_re_procdef            流程定义表，部署每个新的流程定义都会在这张表中增加一条记录
  act_ge_bytearray        流程资源表 
  - 接下来我们来看看，写入了什么数据：
```sql
SELECT * FROM act_re_deployment #流程定义部署表，记录流程部署信息
```
![20220504221641](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504221641.png)
</br>

```sql
SELECT * FROM act_re_procdef #流程定义表，记录流程定义信息
```
注意，KEY 这个字段是用来唯一识别不同流程的关键字
![20220504221701](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504221701.png)
</br>

```sql
SELECT * FROM act_ge_bytearray #资源表 
```
![20220504221732](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504221732.png)

  - act_re_deployment和act_re_procdef一对多关系，一次部署在流程部署表生成一条记录，但一次部署可以部署多个流程定义，每个流程定义在流程定义表生成一条记录。每一个流程定义在act_ge_bytearray会存在两个资源记录，bpmn和png。

**PS：一次部署一个流程，这样部署表和流程定义表是一对一有关系，方便读取流程部署及流程定义信息。**

### 3.8 开启流程实例
- 流程定义部署在activiti后就可以通过工作流管理业务流程了，也就是说上边部署的出差申请流程可以使用了。针对该流程，启动一个流程表示发起一个新的出差申请单，这就相当于java类与java对象的关系，类定义好后需要new创建一个对象使用，当然可以new多个对象。对于请出差申请流程，张三发起一个出差申请单需要启动一个流程实例，出差申请单发起一个出差单也需要启动一个流程实例。

- 代码示例
~~~java
/**
     * 启动流程实例
     */
    @Test
    public void testStartProcess(){
        // 1、创建ProcessEngine
        ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
        //2、获取RunTimeService
        RuntimeService runtimeService = processEngine.getRuntimeService();
        //3、根据流程定义Id启动流程
        ProcessInstance processInstance = runtimeService
                .startProcessInstanceByKey("myEvection");
        // 输出内容
        System.out.println("流程定义id：" + processInstance.getProcessDefinitionId());
        System.out.println("流程实例id：" + processInstance.getId());
        System.out.println("当前活动Id：" + processInstance.getActivityId());
    }
~~~
输出结果: 

![20220504222942](https://note--source.oss-cn-shenzhen.aliyuncs.com//notePic/20220504222942.png)

- 操作影响的表
act_hi_actinst 流程实例执行历史
act_hi_identitylink 流程的参与用户历史信息
act_hi_procinst 流程实例历史信息
act_hi_taskinst 流程任务历史信息
act_ru_execution 流程执行信息
act_ru_identitylink 流程的参与用户信息
act_ru_task 任务信息

### 3.9 任务查询
- 流程启动后，任务的负责人就可以查询自己当前需要处理的任务，查询出来的任务都是该用户的待办任务。
- 代码示例
~~~java
    /**
    * 查询当前个人待执行的任务
    */
    @Test
    public void testFindPersonalTaskList() {
        //任务负责人
        String assignee = "zhangsan";
        ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
        //创建TaskService
        TaskService taskService = processEngine.getTaskService();
        //根据流程key 和 任务负责人 查询任务
        List<Task> list = taskService.createTaskQuery()
                .processDefinitionKey("myEvection") //流程Key
                .taskAssignee(assignee)//只查询该任务负责人的任务
                .list();
        for (Task task : list) {
            System.out.println("流程实例id：" + task.getProcessInstanceId());
            System.out.println("任务id：" + task.getId());
            System.out.println("任务负责人：" + task.getAssignee());
            System.out.println("任务名称：" + task.getName());
        }
    }
~~~
输出结果如下：
~~~
流程实例id：2501
任务id：2505
任务负责人：zhangsan
任务名称：创建出差申请
~~~
### 3.9 完成任务
- 任务负责人查询待办任务，选择任务进行处理，完成任务。
- 代码实例
~~~java
// 完成任务
    @Test
    public void completTask(){
        //获取引擎
        ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
        //获取taskService
        TaskService taskService = processEngine.getTaskService();

        //根据流程key 和 任务的负责人 查询任务
        //返回一个任务对象
        Task task = taskService.createTaskQuery()
                .processDefinitionKey("myEvection") //流程Key
                .taskAssignee("zhangsan")  //要查询的负责人
                .singleResult();
        //完成任务,参数：任务id
        taskService.complete(task.getId());
    }
~~~

### 3.10 流程定义信息查询
- 查询流程相关信息，包含流程定义，流程部署，流程定义版本
- 代码示例
```java
    /**
     * 查询流程定义
     */
    @Test
    public void queryProcessDefinition(){
        //获取引擎
        ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
        //repositoryService
        RepositoryService repositoryService = processEngine.getRepositoryService();
        //得到ProcessDefinitionQuery 对象
        ProcessDefinitionQuery processDefinitionQuery = repositoryService.createProcessDefinitionQuery();
        //查询出当前所有的流程定义
        //条件：processDefinitionKey =evection
        //orderByProcessDefinitionVersion 按照版本排序
        //desc倒叙
        //list 返回集合
        List<ProcessDefinition> definitionList = processDefinitionQuery.processDefinitionKey("myEvection")
                .orderByProcessDefinitionVersion()
                .desc()
                .list();
        //输出流程定义信息
        for (ProcessDefinition processDefinition : definitionList) {
            System.out.println("流程定义 id="+processDefinition.getId());
            System.out.println("流程定义 name="+processDefinition.getName());
            System.out.println("流程定义 key="+processDefinition.getKey());
            System.out.println("流程定义 Version="+processDefinition.getVersion());
            System.out.println("流程部署ID ="+processDefinition.getDeploymentId());
        }

    }
```
输出结果：
```
流程定义id：myEvection:1:4
流程定义名称：出差申请单
流程定义key：myEvection
流程定义版本：1
```

### 3.11 流程删除
- 代码示例
```java
public void deleteDeployment() {
		// 流程部署id
		String deploymentId = "1";
		
    ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
    // 通过流程引擎获取repositoryService
		RepositoryService repositoryService = processEngine
				.getRepositoryService();
		//删除流程定义，如果该流程定义已有流程实例启动则删除时出错
		repositoryService.deleteDeployment(deploymentId);
		//设置true 级联删除流程定义，即使该流程有流程实例启动也可以删除，设置为false非级别删除方式，如果流程
		//repositoryService.deleteDeployment(deploymentId, true);
	}

```

- 说明：
1. 使用repositoryService删除流程定义，历史表信息不会被删除
2. 如果该流程定义下没有正在运行的流程，则可以用普通删除。
3. 如果该流程定义下存在已经运行的流程，使用普通删除报错，可用级联删除方法将流程及相关记录全部删除。先删除没有完成流程节点，最后就可以完全删除流程定义信息
4. 项目开发中级联删除操作一般只开放给超级管理员使用
   

### 3.12 流程资源下载
- 现在我们的流程资源文件已经上传到数据库了，如果其他用户想要查看这些资源文件，可以从数据库中把资源文件下载到本地。

- 解决方案：
  1. jdbc对blob类型，clob类型数据读取出来，保存到文件目录
  2. 使用activiti的api来实现

- 方案2的实现:使用activiti的api来实现
  - 使用commons-io.jar 解决IO的操作
  - 引入commons-io依赖包
```xml
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.6</version>
</dependency>
```
通过流程定义对象获取流程定义资源，获取bpmn和png

```java
import org.apache.commons.io.IOUtils;

@Test
    public void deleteDeployment(){
        //获取引擎
        ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
        //获取repositoryService
        RepositoryService repositoryService = processEngine.getRepositoryService();
        //根据部署id 删除部署信息,如果想要级联删除，可以添加第二个参数，true
        repositoryService.deleteDeployment("1");
    }

    public void  queryBpmnFile() throws IOException {
        //1、得到引擎
        ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
        //2、获取repositoryService
        RepositoryService repositoryService = processEngine.getRepositoryService();
        //3、得到查询器：ProcessDefinitionQuery，设置查询条件,得到想要的流程定义
        ProcessDefinition processDefinition = repositoryService.createProcessDefinitionQuery()
                .processDefinitionKey("myEvection")
                .singleResult();
        //4、通过流程定义信息，得到部署ID
        String deploymentId = processDefinition.getDeploymentId();
        //5、通过repositoryService的方法，实现读取图片信息和bpmn信息
        //png图片的流
        InputStream pngInput = repositoryService.getResourceAsStream(deploymentId, processDefinition.getDiagramResourceName());
        //bpmn文件的流
        InputStream bpmnInput = repositoryService.getResourceAsStream(deploymentId, processDefinition.getResourceName());
        //6、构造OutputStream流
        File file_png = new File("d:/evectionflow01.png");
        File file_bpmn = new File("d:/evectionflow01.bpmn");
        FileOutputStream bpmnOut = new FileOutputStream(file_bpmn);
        FileOutputStream pngOut = new FileOutputStream(file_png);
        //7、输入流，输出流的转换
        IOUtils.copy(pngInput,pngOut);
        IOUtils.copy(bpmnInput,bpmnOut);
        //8、关闭流
        pngOut.close();
        bpmnOut.close();
        pngInput.close();
        bpmnInput.close();
    }

```

- 说明：
   1. deploymentId为流程部署ID
   2. resource_name为act_ge_bytearray表中NAME_列的值
   3. 使用repositoryService的getDeploymentResourceNames方法可以获取指定部署下得所有文件的名称
   4. 使用repositoryService的getResourceAsStream方法传入部署ID和资源图片名称可以获取部署下指定名称文件的输入流

### 3.13 流程历史信息的查看
- 即使流程定义已经删除了，流程执行的历史信息通过前面的分析，依然保存在activiti的act_hi_*相关的表中。所以我们还是可以查询流程执行的历史信息，可以通过HistoryService来查看相关的历史记录。
- 代码示例
```java
    /**
     * 查看历史信息
     */
    @Test
    public void findHistoryInfo(){
        //获取引擎
        ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
        //获取HistoryService
        HistoryService historyService = processEngine.getHistoryService();
        //获取 actinst表的查询对象
        HistoricActivityInstanceQuery instanceQuery = historyService.createHistoricActivityInstanceQuery();
        //查询 actinst表，条件：根据 InstanceId 查询
        //instanceQuery.processInstanceId("2501");
        //查询 actinst表，条件：根据 DefinitionId 查询
        instanceQuery.processDefinitionId("myEvection:1:4");
        //增加排序操作,orderByHistoricActivityInstanceStartTime 根据开始时间排序 asc 升序
        instanceQuery.orderByHistoricActivityInstanceStartTime().asc();
        //查询所有内容
        List<HistoricActivityInstance> activityInstanceList = instanceQuery.list();
        //输出
        for (HistoricActivityInstance hi : activityInstanceList) {
            System.out.println(hi.getActivityId());
            System.out.println(hi.getActivityName());
            System.out.println(hi.getProcessDefinitionId());
            System.out.println(hi.getProcessInstanceId());
            System.out.println("<==========================>");
        }
    }
```