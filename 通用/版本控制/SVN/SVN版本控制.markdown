# SVN版本控制
## 1.SVN介绍
### 1.1 SVN简介  
- SVN全称Subversion，是⼀个<font color = red>开放源代码的版本控制系统</font>，管理着随时间改变的代码。这些代码放置在⼀个中央资料档案库(repository) 中。 这个档案库很像⼀个普通的⽂件服务器,不过它会记住每⼀次⽂件的变动。 这样你就可以把档案恢复到旧的版本, 或是浏览⽂件的变动历史。说得简单⼀点SVN就是⽤于多个⼈共同开发同⼀个项⽬，管理共⽤资源的⽬的。

### 1.2 SVN主要作用
- 目录版本控制  
  Subversion 实现了⼀个 "虚拟" 的版本控管⽂件系统, 能够依时间跟踪整个⽬录的变动。⽬录和⽂件都能进⾏版本控制。
- 真实的版本历史  
   Subversion中，可以对⽂件和⽬录进行增加（add）、删除（delete）、复制（copy）和重命名（rename）等操作。所有的新加的⽂件都从⼀个新的、⼲净的版本开始。
- 自动提交  
  <font color = grey>⼀个提交动作，不是全部更新到了档案库中，就是完全不更新。这允许开发⼈员以逻辑区间建⽴并提交变动，以防⽌当部分提交成功时出现的问题。</font>

### 1.3 SVN基本操作
- Repository（源代码库）：源代码统⼀存放的地⽅
- Checkout（提取）：当你⼿上没有源代码的时候，你需要从repository checkout⼀份
- Commit（提交）：当你已经修改了代码，你就需要Commit到repository
- Update (更新)：当你已经Checkout了⼀份源代码， Update后就可以和Repository上的源代码同步

### 1.4 SVN工作流程  
1、如果本地没有代码，从服务器下载项⽬组最新代码。（Checkout）  
2、如果已经Checkout并且有⼈已Commit了代码，你可以更新以获得最新代码。（Update）  
3、进⼊⾃⼰的分⽀，进⾏⼯作，每隔⼀个⼩时向服务器⾃⼰的分⽀提交⼀次代码（很多⼈都有这个习惯。因为有时候⾃⼰对代码改来改去，最后⼜想还原到前⼀个⼩时的版本，或者看看前⼀个⼩时⾃⼰修
改了哪些代码，就需要这样做了）。（Commit）  
4、下班时间到了，把⾃⼰的分⽀合并到服务器主分⽀上，⼀天的⼯作完成，并反映给服务器。
（Commit）     

<font color = red>PS：如果两个程序员同时修改了同⼀个⽂件， SVN可以合并这两个程序员的改动，实际上SVN管理源
代码是以⾏为单位的，就是说两个程序员只要不是修改了同⼀⾏程序，SVN都会⾃动合并两种修改。如
果是同⼀⾏，SVN会提示⽂件Confict, 冲突，需要⼿动确认。</font>    
     
![SVN工作流程图](https://note.youdao.com/yws/res/b/WEBRESOURCE05b3fb63c826cef3b9cc04a85daf408b)    
### 1.5 SVN生命周期
#### 1.5.1 创建版本库  
- 版本库相当于⼀个集中的空间，⽤于存放开发者所有的⼯作成果。版本库不仅能存放⽂件，还包括了
每次修改的历史，即每个⽂件的变动历史。
- Create 操作是⽤来创建⼀个新的版本库。⼤多数情况下这个操作只会执⾏⼀次。当你创建⼀个新的版
本库的时候，你的版本控制系统会让你提供⼀些信息来标识版本库，例如创建的位置和版本库的名字。  

#### 1.5.2 检出  
- Checkout 操作是⽤来从版本库创建⼀个⼯作副本(从远程仓库拉取到本地)。⼯作副本是开发者私⼈的⼯作空间，可以进⾏内容的修改，然后提交到版本库中。 

#### 1.5.3 更新  
- 顾名思义，update 操作是⽤来更新版本库的。这个操作将⼯作副本与版本库进⾏同步。由于版本库是由整个团队共⽤的，当其他⼈提交了他们的改动之后，你的⼯作副本就会过期。所以你必须通过update操作从远程仓库获取最新版本的代码  

#### 1.5.4 执行变更
- 当你从远程仓库获取到代码后可以对文件和目录进行新增、删除、修改等操作，这些变动只是对本地项目进行操作，只有在commit操作后，才会提交到远程仓库。

#### 1.5.5 复查变化  
- 当你检出⼯作副本或者更新⼯作副本后，你的⼯作副本就跟版本库完全同步了。但是当你对⼯作副本
进⾏⼀些修改之后，你的⼯作副本会⽐版本库要新。在 commit 操作之前复查下你的修改是⼀个很好的
习惯。  
Status 操作列出了⼯作副本中所进⾏的变动。正如我们之前提到的，你对⼯作副本的任何改动都会成
为待变更列表的⼀部分。Status 操作就是⽤来查看这个待变更列表。  
Status 操作只是提供了⼀个变动列表，但并不提供变动的详细信息。你可以⽤ diff 操作来查看这些变
动的详细信息。  

#### 1.5.6 修复错误(版本回滚)  
- 我们来假设你对⼯作副本做了许多修改，但是现在你不想要这些修改了，这时候 revert 操作将会帮助你。  Revert操作重置了对⼯作副本的修改。它可以重置⼀个或多个⽂件/⽬录。当然它也可以重置整个⼯作副本。在这种情况下，revert操作将会销毁待变更列表并将⼯作副本恢复到原始状态。

#### 1.5.7 版本冲突
- 合并的时候可能会发⽣冲突。Merge 操作会⾃动处理可以安全合并的东⻄。其它的会被当做冲突。例
如，"hello.c" ⽂件在⼀个分⽀上被修改，在另⼀个分⽀上被删除了。这种情况就需要⼈为处理。
Resolve 操作就是⽤来帮助⽤户找出冲突并告诉版本库如何处理这些冲突。  
<font color = red> PS:当两人修改同一行代码并提交到远程仓库（或者更新）时才会出现冲突</font>

#### 1.5.8  提交修改
- Commit 操作是⽤来将更改从⼯作副本到版本库。这个操作会修改版本库的内容，其它开发者可以通
过更新他们的⼯作副本来查看这些修改。  
<font color = red>PS:在提交之前，你必须将⽂件/⽬录添加到待变更列表(版本控制管理中)中。列表中记录了将会被提交的改动。当提交的时候，我们通常会提供⼀个注释来说明为什么会进⾏这些改动。这个注释也会成为版本库历史记录的⼀部分。Commit是⼀个原⼦操作，也就是说要么完全提交成功，要么失败回滚。⽤户不会看到成功提交
⼀半的情况。 </font>
---

## 2. SVN安装  
### 2.1 SVN下载
- SVN服务器下载  [服务器下载链接](https://www.visualsvn.com/downloads/)
- SVN客户端下载  [客户端下载链接](https://tortoisesvn.net/downloads.html)
---

## 3.SVN操作
- SVN原生操作     [SVN原生操作](https://www.runoob.com/svn/svn-tutorial.html)
- TortoiseSVN操作 [TortoiseSVN操作](https://www.runoob.com/svn/tortoisesvn-intro.html)  
<font color = red>PS:素材目录中有SVN笔记</font>
---

## 4.IDEA集成SVN
### 4.1 IDEA配置SVN
1. File —> Other Settings（全局配置；Settings是局部配置） —> Version Control —> Subversion  ![image.png](https://note.youdao.com/yws/res/2/WEBRESOURCE7d33afdfde81d72547d2211460baf892)   
  
2. 配置svn  
![image.png](https://note.youdao.com/yws/res/5/WEBRESOURCEb510ecad00a22e8df7007c7461c50ca5) <font color = red>PS:找不到svn.exe⽂件，TortoiseSVN的bin⽬录下⾯没有svn.exe
之所以没有是因为安装TortoiseSVN的时候没有勾选指定安装项，添加command line client
tools</font>
3. 重启Idea

### 4.2 检索项目（git中clone操作）  
1. 选择 VCS —> Checkout from Version Control —> Subversion
![image.png](https://note.youdao.com/yws/res/f/WEBRESOURCEbb4f714aa002690c3d080863120f6a6f)         

2. 添加远程仓库中项⽬的URL  
![image.png](https://note.youdao.com/yws/res/4/WEBRESOURCE78fbd15385ec1c88bea9e915929de2e4)   
3. 点击添加的URL，选择 Checkout  
![image.png](https://note.youdao.com/yws/res/b/WEBRESOURCE6e7212f1311bddf845396d20f476f3ab)
  
4. 选择检索的项⽬的存放位置   
![image.png](https://note.youdao.com/yws/res/e/WEBRESOURCEdb58d919be945fe078e3f81412e908ce)
  
5. 选择Destination，根据⾃⼰的偏好选择，其他配置默认，单击 OK
![image.png](https://note.youdao.com/yws/res/3/WEBRESOURCE03620b18dbe3d7c5f2cbb7ed59131f63)
  
6. 选择 1.8 Format，点击 OK  
![image.png](https://note.youdao.com/yws/res/c/WEBRESOURCE51d2d458c7d26f2db77f43151a56be3c)  
  
7. 已经check out⼀个项⽬，是否要打开，选择 Yes  
![image.png](https://note.youdao.com/yws/res/a/WEBRESOURCE4ba7404da704986540eb51b63d1b243a)  
8. 选择 Add   
![image.png](https://note.youdao.com/yws/res/8/WEBRESOURCEb6bae1a7ebb253f9a93cad6ea9f531c8)  
9. 此时，项⽬就可以与远程仓库关联  
![image.png](https://note.youdao.com/yws/res/7/WEBRESOURCEde09ca1a7db9fa26465939eb3e58c587)  

### 4.3 提交代码
1. 选择 VCS —> Commit...   
![image.png](https://note.youdao.com/yws/res/4/WEBRESOURCE057b623a152279d5f31882102d1324b4)   

2. 选择需要提交的⽂件，填写提交信息，选择 Commit  
![image.png](https://note.youdao.com/yws/res/f/WEBRESOURCE030e88c41c83dbf78af78dd2e332c94f)        
3. 提交成功后，会在Idea最下⾯显示提交状态  
![image.png](https://note.youdao.com/yws/res/1/WEBRESOURCE936a05b31e7f1d6ae95d972dc67a3781)  
<font color = red>PS:提交前，先进行更新操作</font>

### 4.4 更新版本
1. 选择 VCS —> Update Project...  
![image.png](https://note.youdao.com/yws/res/4/WEBRESOURCE60bdb653cadcbca76ab6725082e91464)  

2. 默认即可，直接选择 OK   
![image.png](https://note.youdao.com/yws/res/6/WEBRESOURCEaa022237ae6b4c2bd399b42bd0b875e6)   

3. 更新成功的提示信息  
![image.png](https://note.youdao.com/yws/res/b/WEBRESOURCE2390d3372d84d7a25dcf5e7160ad693b)

### 4.5 项目导入远程仓库
1. 选择 VCS —> Import into Version Control —> Import into Subversion  
![image.png](https://note.youdao.com/yws/res/1/WEBRESOURCE23595b66b61e1b22fb98708d84ec4861)    

2. 选择 "+" 添加项⽬导⼊的地址（可⼿动添加⼀个⽂件夹，项⽬中的⽂件会放置在该⽂件夹中，⽂件
名⾃定义）  
![image.png](https://note.youdao.com/yws/res/f/WEBRESOURCEc491c5ad0813bec7663396e1bc99d92f)  

3. 选择要导⼊的路径，选择 Import  
![image.png](https://note.youdao.com/yws/res/b/WEBRESOURCEb41390417dd324253888f8b2d334ce3b)    

4. 选择要导⼊的项⽬，点击 OK    
![image.png](https://note.youdao.com/yws/res/5/WEBRESOURCEd2ee47b8fcab6134876e39d0e90ba625)  

5. 检查导⼊的路径，填写导⼊信息，选择 OK   
![image.png](https://note.youdao.com/yws/res/9/WEBRESOURCE90f1950c71947ffeb546df38b1b04229)  
6. 在远程仓库中检查是否导⼊成功即可。

### 4.6 版本冲突解决
1. 如果未更新，就提交资源（有其他⽤户也提交过资源），则提交失败  
![image.png](https://note.youdao.com/yws/res/6/WEBRESOURCEb4abe859694199576010eab1c4092e36)    

2. 此时，执⾏更新操作，将其他⼈提交过的资源更新到本地，会提示冲突  
![image.png](https://note.youdao.com/yws/res/5/WEBRESOURCEfbdc079a92ef23e8f831e8c17589fab5)  
3. 通常选择合并，再选择需要保留的代码，选择好之后选择 Apply  
![image.png](https://note.youdao.com/yws/res/8/WEBRESOURCEf730bfca66ea93c522e1916c5a643e58)  
4. 提示更新成功  
![image.png](https://note.youdao.com/yws/res/4/WEBRESOURCEa5f662ec3dfe96666a08d08222d0ee24)  
5. 再次选择提交，成功解决冲突  
![image.png](https://note.youdao.com/yws/res/b/WEBRESOURCEdda5e0e2e86194b808ed6bad575c3e2b)