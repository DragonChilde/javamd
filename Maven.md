# Maven简介

## Maven概述

Maven是Apache软件基金会组织维护的一款自动化构建工具。主要有两个作用

1. maven工程对jar包的管理过程

2. 项目的一键构建

   1. maven工程对jar包的管理过程

      ![](http://120.77.237.175:9080/photos/maven/01.png)

   2. 项目的一键构建

      1. 先来说说什么是构建？指的是项目从编译、测试、运行、打包、安装 ，部署整个过程都交给 maven 进行管理，这个过程称为构建

      2. 一键构建指的是整个构建过程，使用 maven 一个命令可以轻松完成整个工作

         ![](http://120.77.237.175:9080/photos/maven/02.png)



# Maven核心概念

## Maven目录结构

1. maven中约定的目录结构图解

   ![](http://120.77.237.175:9080/photos/maven/03.png)

2. 各个目录结构详解

   ```
   Maven工程的目录结构
   (1). src/main/java —— 存放项目的.java 文件
   (2). src/main/resources —— 存放项目资源文件，如 spring, hibernate 配置文件
   (3). src/test/java —— 存放所有单元测试.java 文件，如 JUnit 测试类
   (4). src/test/resources —— 测试资源文件
   (5). target —— 项目输出位置，编译后的 class 文件会输出到此目录
   (6). pom.xml——maven 项目核心配置文件
   注意：如果是普通的 java 项目，那么就没有 webapp 目录
   ```

## pom.xml

1. modelVersion:Maven模型的版本,对于Maven2和Maven3来说,它只能是4.0.0
2. **`groupId:`组织id,一般是公司域名的倒写,格式可以为**
   1. 域名倒写,例如 com.baidu
   2. 域名倒写+项目名,例如com.baidu.appolo
3. **`artifactId:`项目名称,也是模块名称对应groupId中项目中的子项目**
4. **`version:`项目的版本号。如果项目还在开发中,是不稳定版本,通常在版本后带 -SNAPSHOT。version使用三位数字标识,例如1.1.0**
5. name:项目的名称
6. packaging:项目打包的类型,可以使 jar、war、rar、ear、pom,默认是 jar(一般父工程设置为pom,子工程设置为jar),web项目是war包
7. **`dependencies | dependency`：Maven 的一个重要作用就是管理jar包,为了一个项目可以构建或运行,项目中不可避免的,会依赖很多其他的jar包,在Maven中,这些 jar 就被称为依赖,使用标签dependency来配置。而这种依赖的配置正是通过坐标来定位的,由此我们也不难看出,maven 把所有的 jar包也都视为项目存在了**
8. properties:是用来定义一些配置属性的,例如project.build.sourceEncoding（项目构建源码编码方式）,可以设置为UTF-8,防止中文乱码,也可定义相关构建版本号,便于日后统一升级(配置属性)
9. build:表示与构建相关的配置,例如设置编译插件的jdk版本(构建)
10. **`parent:`在Maven中,如果多个模块都需要声明相同的配置,例如：groupId、version、有相同的依赖、或者相同的组件配置等,也有类似Java的继承机制,用parent声明要继承的父工程的pom配置(继承)**
    1. **`modules:`在Maven的多模块开发中,为了统一构建整个项目的所有模块,可以提供一个额外的模块,该模块打包方式为pom,并且在其中使用modules聚合的其它模块,这样通过本模块就可以一键自动识别模块间的依赖关系来构建所有模块,叫Maven的聚合**
    2. description:描述信息
    3. relativePath
11. 父项目的pom.xml文件的相对路径。默认值为…/pom.xml。maven首先从当前构建项目开始查找父项目的pom文件,然后从本地仓库,最后从远程仓库。RelativePath允许你选择一个不同的位置
12. 如果默认…/pom.xml没找到父元素的pom,不配置relativePath指向父项目的pom则会报错

## 仓库repository

1. 本地仓库 ：用来存储从远程仓库或中央仓库下载的插件和 jar 包，项目使用一些插件或 jar 包，优先从本地仓库查找

   ```
   <!--本地仓库配置mavne安装目录下的settings.xml文件里-->
   <localRepository>D:\Server\LocalRepository</localRepository>
   ```

2. 远程仓库

   1. 中央仓库：在maven软件中内置一个远程仓库地址http://repo1.maven.org/maven2,它是中央仓库,服务于整个互联网,它是由Maven团队自己维护,里面存储了非常全的jar包,它包含了世界上大部分流行的开源项目构件
   2. 私服:在局域网环境中部署的服务器,为当前局域网范围内的所有Maven工程服务。公司中常常使
   3. 中央仓库的镜像：架设在不同位置,欧洲,美洲,亚洲等每个洲都有若干的服务器,为中央仓库分担流量。减轻中央仓库的访问,下载的压力。所在洲的用户首先访问的是本洲的镜像服务器

![](http://120.77.237.175:9080/photos/maven/04.png)

## Maven的生命周期

maven 对项目构建过程分为三套相互独立的生命周期，请注意这里说的是“三套”，而且“相互独立”,这三套生命周期分别是

1. Clean Lifecycle 在进行真正的构建之前进行一些清理工作
2. Default Lifecycle 构建的核心部分，编译，测试，打包，部署等等
3. Site Lifecycle 生成项目报告，站点，发布站点

![](http://120.77.237.175:9080/photos/maven/05.png)

对于我们程序员而言,**无论我们要进行哪个阶段的构建,直接执行相应的命令即可,无需担心它前边阶段是否构建,Maven都会自动构建**。这也就是Maven这种自动化构建工具给我们带来的好处

## Maven常用命令

- mvn clean:将target目录删除,但是已经 install 到仓库里的包不会删除

- mvn compile:编译(执行的是主程序下的代码）

- mvn test:测试(不仅仅编译了src/main/java下的代码，也编译了java/test/java下的代码）

- mvn package:打包(执行的时候回打成war包，编译了src/main/java下的代码，也编译了java/test/java下的代码）

- mvn install:安装(执行的时候回打成war包，编译了src/main/java下的代码，也编译了java/test/java下的代码，将这个包安装到了本地仓库中）

- mvn deploy:部署(需要配置才能执行）

  > **`注意：`**运行Maven命令时一定要进入pom.xml文件所在的目录！

## 插件plugings

maven提供的功能,用来执行清理、编译、测试、报告、打包的程序

- **clean插件`maven-clean-plugin:2.5`**
  clean阶段是独立的一个阶段,功能就是清除工程目前下的target目录
- **resources插件`maven-resources-plugin:2.6`**
  resource插件的功能就是把项目需要的配置文件拷贝到指定的目当,默认是拷贝src\main\resources目录下的件到classes目录下
- **compile插件`maven-compiler-plugin`**
  compile插件执行时先调用resouces插件,功能就是把src\mainjava源码编译成字节码生成class文件,并把编译好的class文件输出到target\classes目录下
- **test测试插件**
  单元测试所用的compile和resources插件和主代码是相同的,但执行的目标不行,目标testCompile和testResources是把src\test\java下的代码编译成字节码输出到target\test-classes,同时把src\test\resources下的配置文件拷贝到target\test-classes
- **package打包插件`maven-jar-plugin`**
  这个插件是把class文件、配置文件打成一个jar(war或其它格式)包
- **deploy发布插件`maven-install-plugin`**
  发布插件的功能就是把构建好的artifact部署到本地仓库,还有一个deploy插件是将构建好的artifact部署到远程仓库

![](http://120.77.237.175:9080/photos/maven/06.png)

## 坐标gav

- Maven把任何一个插件都作为仓库中的一个项目进行管理,用一组(三个)向量组成的坐标来表示。坐标在仓库中可以唯一定位一个Maven项目
- groupId:组织名,通常是公司或组织域名倒序+项目名
- artifactId:模块名,通常是工程名
- version:版本号
  (需要特别指出的是,项目在仓库中的位置是由坐标来决定的:groupId、artifactId和version决定项目在仓库中
  的路径,artifactId和version决定jar包的名称)

## 依赖dependency

- 一个Maven项目正常运行需要其它项目的支持,Maven会根据坐标自动到本地仓库中进行查找。对于程序员自己的Maven项目需要进行安装,才能保存到仓库中
-  不用maven的时候所有的jar都不是你的,需要去各个地方下载拷贝,用了maven所有的jar包都是你的,想要谁,叫谁的名字就行。maven帮你下载

# Maven全局配置

在mavne安装目录下的settings.xml文件里配置如下,

```
 <!--指定JDK全局JDK版本-->
 <profiles>
     <profile>  
             <id>jdk-1.8</id> 
              <activation>  
                  <activeByDefault>true</activeByDefault>  
                  <jdk>1.8</jdk>  
              </activation>  
              <properties>  
              	<!--源码编译jdk版本-->	
                  <maven.compiler.source>1.8</maven.compiler.source>  
                  <!--运行代码的jdk版本-->
                  <maven.compiler.target>1.8</maven.compiler.target>  
                  <!--项目构建使用的编码,避免中文乱码-->
                  <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
                  <!--生成报告的编码-->
                  <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>  
              </properties>   
    </profile>  
<profiles>
```

# IDEA配置Maven

![](http://120.77.237.175:9080/photos/maven/07.jpg)

- -DarchetypeCatalog=internal
  这个参数的意思是:
  如果我们使用maven为我们提供好的骨架来创建maven工程,一般是要联网的.
  为了在不联网的情况下我们可以正常创建工程,配了这样一个参数,只要我们之前联网下载过之前相关创建工程的插件,它就会从本地仓库找到对应插件,而不会联网下载

## 使用骨架创建maven的java工程

![](http://120.77.237.175:9080/photos/maven/08.jpg)

## 创建一个maven的web工程

![](http://120.77.237.175:9080/photos/maven/09.jpg)

> 注：如果不想用模板，只想创建普通的maven项目， 在上图不用勾选从原型创建即可

## Servlet冲突问题

1. 开发过程中,web的工程的项目中,因为创建了Servlet,但报错,因为缺少了相应的jar包,要解决问题,就是要将`servlet-api-xxx.jar`包放进来,作为maven工程应当添加servlet的坐标,从而导入它的jar包

2. jar包冲突问题

   在开发过程中,pom文件中导入的的`servlet-api-xxx.jar`和maven自带的tomcat中的Jar包发生了冲突

   	<!--放置的都是项目所要依赖的jar包-->
   	<!--provided的意思是编译时使用它,运行时不使用-->
   	<dependency>
   	  <groupId>javax.servlet</groupId>
   	  <artifactId>servlet-api</artifactId>
   	  <version>2.5</version>
   	  <scope>provided</scope>	<!--解决jar包冲突问题就是添加scope,将其设置为provided-->
   	</dependency>
   	<dependency>
   	  <groupId>javax.servlet.jsp</groupId>
   	  <artifactId>jsp-api</artifactId>
   	  <version>2.0</version>
   	  <scope>provided</scope>	<!--解决jar包冲突问题就是添加scope,将其设置为provided-->
   	</dependency>

## war和war exploded的区别

1. war模式这种可以称之为是发布模式，看名字也知道，这是先打成war包，再发布
2. war exploded模式是直接把文件夹、jsp页面 、classes等等移到Tomcat 部署文件夹里面，进行加载部署。因此这种方式支持热部署，一般在开发的时候也是用这种方式
3. 在平时开发的时候，使用热部署的话，应该对Tomcat进行相应的设置，这样的话修改的jsp界面什么的东西才可以及时的显示出来

# Maven的依赖范围

maven的依赖范围包括:compile、provide、runtime、test、system

1. compile:编译范围的依赖会用在编译,测试,运行,由于运行时需要,所以编译范围的依赖会被打包(会被打包)
2. test:test范围依赖在编译和运行时都不需要,只在测试编译和测试运行时需要。例如:Junit。由于运行时不需要,所以test范围依赖不会被打包(不会打包)
3. provide:provide依赖只有当jdk或者一个容器已提供该依赖之后才使用。provide依赖在编译和测试时需要,在运行时不需要。例如:servletapi被Tomcat容器提供了(不会打包)
4. runtime:runtime依赖在运行和测试系统时需要,但在编译时不需要。例如:jdbc的驱动包。由于运行时需要,所以runtime范围的依赖会被打包(会打包)
5. system:system范围依赖与provide类似,但是必须显示的提供一个对于本地系统中jar文件的路径。一般不推荐使用

| 依赖范围 | 对于编译classpath有效 | 对于测试classpath有效 | 对于运行时classpath有效 |            例子            |
| :------: | :-------------------: | :-------------------: | :---------------------: | :------------------------: |
| compile  |           Y           |           Y           |            Y            |        spring-core         |
|   test   |           -           |           Y           |            Y            |           Junit            |
| provided |           Y           |           Y           |            -            |        servlet-api         |
| runtime  |           -           |           Y           |            Y            |          JDBC驱动          |
|  system  |           Y           |           Y           |            -            | 本地的,maven仓库之外的类库 |

例:

```
		<!--导入mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
			<!--runtime表示编译器不使用它,运行期使用它-->
			<scope>runtime</scope>
            <version>8.0.17</version>
        </dependency>
        <!--导入servlet相关依赖,request不报错-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
			<!--provided的意思是编译时使用它,运行时不使用-->
            <scope>provided</scope>
        </dependency>
		<--jsp-->	
		<dependency>
		  <groupId>javax.servlet.jsp</groupId>
		  <artifactId>jsp-api</artifactId>
		  <version>2.0</version>
		  <scope>provided</scope>
		</dependency>
		<!--system 编译测试有用、不会运行打成jar-->
		<dependency>
            <groupId>com.sun</groupId>
            <artifactId>tools</artifactId>
            <scope>system</scope>
            <optional>true</optional>
            <version>${java.version}</version>
            <systemPath>${java.home}/../lib/tools.jar</systemPath>
        </dependency>

```

# Maven的常用设置

## 全局变量

1. 在Maven的pom.xml文件中,properties用于定义全局变量,POM中通过${property_name}的形式引用变量的值。定义全局变量:

   ```
   	<properties>
   		<spring.version>4.3.10.RELEASE</spring.version>
   	</properties>
   ```

2. 引用全局变量:

   ```
   	<dependency>
   		<groupId>org.springframework</groupId>
   		<artifactId>spring-context</artifactId>
   		<version>${spring.version}</version>
   	</dependency>
   ```

## Maven系统采用的变量

```
<properties>
	<!--源码编译jdk版本-->		
	<maven.compiler.source>1.8</maven.compiler.source>
	<!--运行代码的jdk版本-->
	<maven.compiler.target>1.8</maven.compiler.target>
	<!--项目构建使用的编码,避免中文乱码-->
	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	<!--生成报告的编码-->
	<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
</properties>
```

## 指定资源位置

1.  src/main/java和src/test/java这两个目录中的所有*.java文件会分别在comile和test-comiple阶段被编译,编译结果分别放到了target/classes和targe/test-classes目录中,但是这两个目录中的其他文件都会被忽略掉,如果需要把src目录下的文件包放到target/classes目录,作为输出的jar一部分。需要指定资源文件位置。以下内容放到buid标签中

2. 假设要把源目录里src/main/java/com的*.txt输出到编译后以target/classes目录中

3. 在pom.xml添加如下内容,然后重新打包即可

   ```
   	<build>
   	     <resources>
   	         <resource>
   	             <!--所在的目录-->
   	             <directory>src/main/java</directory>
   	             <includes>
   	                 <!--包括目录下的.properties,.xml 文件都会扫描到-->
   	                 <include>**/*.properties</include>
   	                 <include>**/*.xml</include>
   	                 <include>**/*.txt</include>
   	             </includes>
   	             <!--filtering 选项 false 不启用过滤器， *.property 已经起到过滤的作用了 -->
   	             <filtering>false</filtering>
   	         </resource>
   	     </resources>
   	 </build>
   ```

## 将pom文件中的配置引入到properties文件中

```
    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.MF</include>
                    <include>**/*.XML</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <!--资源文件的路径,默认位于${basedir}/src/main/resources/目录下-->
                <directory>src/main/resources</directory>
                <!--一组文件名的匹配模式,被匹配的资源文件将被构建过程处理-->
                <includes>
                    <include>**/*</include>
                    <include>*</include>
                </includes>
                <!--filtering默认false,true表示通过参数对资源文件中的${key}
                在编译时进行动态变更。替换源可紧-Dkey和pom中的<properties>值
                或<filters>中指定的properties文件-->
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>

```

## Maven默认属性

```
	${basedir} 项目根目录  
	${version}表示项目版本;  
	${project.basedir}同${basedir};  
	${project.version}表示项目版本,与${version}相同;  
	${project.build.directory} 构建目录，缺省为target  
	${project.build.sourceEncoding}表示主源码的编码格式;  
	${project.build.sourceDirectory}表示主源码路径;  
	${project.build.finalName}表示输出文件名称;  
	${project.build.outputDirectory} 构建过程输出目录，缺省为target/classes 
```

```
    输出结果为:
	test=${pro.name}
	# F:\\mavenTest\\A 项目根目录
	basedir=${basedir}
	basedir2=${project.basedir}
	# version=1.0-SNAPSHOT
	version=${version}
	# project.build.directory=F:\\mavenTest\\A\\target 构建目录,缺省为target
	project.build.directory=${project.build.directory}
	# project.build.sourceDirectory=F:\\mavenTest\\A\\src\\main\\java
	# 表示主源码的编码格式
	project.build.sourceDirectory=${project.build.sourceDirectory}
	# project.build.finalName=A-1.0-SNAPSHOT 表示输出文件名称
	project.build.finalName=${project.build.finalName}
	# project.build.outputDirectory=F:\\mavenTest\\A\\target\\classes
	# 构建过程输出目录,缺省为target/classes
	project.build.outputDirectory=${project.build.outputDirectory}
```

# Maven项目依赖、依赖冲突

## 什么是依赖传递

1. 在maven中,依赖是可以传递的,假设存在三个项目,分别是项目A,项目B以及项目C。假设C依赖B,B依赖A,那么我们可以根据maven项目依赖的特征不难推出项目C也依赖A

![](http://120.77.237.175:9080/photos/maven/10.png)

2. 通过上面的图可以看到,我们的web项目直接依赖了spring-webmvc,而spring-webmvc依赖了sping-aop、spring-beans等。最终的结果就是在我们的web项目中间接依赖了spring-aop、spring-beans等

![](http://120.77.237.175:9080/photos/maven/11.png)

## 什么是依赖冲突

1. 加入如下坐标

由于spring-webmvc中依赖了spring-core,而spring-core中依赖了commons-logging(1.1.3),而我们又引入了commons-loging1.2,就造成了冲突

![](http://120.77.237.175:9080/photos/maven/12.png)

2. 根据路径近者优先原则,我们项目中引入的commons-logging为1.2

   ![](http://120.77.237.175:9080/photos/maven/13.png)

## 如何解决依赖冲突

### 使用maven提供的依赖调解原则

1. 依赖调节原则 — 第一声明者优先原则(在 pom 文件中定义依赖，以先声明的依赖为准。其实就是根据坐标导入的顺序来确定最终使用哪个传递过来的依赖)
   结论：通过上图可以看到，spring-aop和spring-webmvc都传递过来了spring-beans，但是因为spring-aop在前面，所以最终使用的spring-beans是由spring-aop传递过来的，而spring-webmvc传递过来的spring-beans则被忽略了

   ![](http://120.77.237.175:9080/photos/maven/14.png)

2. **`根据路径近者优先原则,`** 我们项目中引入的commons-logging为1.2

3. 如果在同一个pom中引入了两个相同的jar包,以引入的最后一个为准

   如下的配置引入的是1.2

   ```
       <dependencies>
           <dependency>
               <groupId>commons-logging</groupId>
               <artifactId>commons-logging</artifactId>
               <version>1.1.2</version>
           </dependency>
           <dependency>
               <groupId>commons-logging</groupId>
               <artifactId>commons-logging</artifactId>
               <version>1.2</version>
           </dependency>
       </dependencies>
   
   ```

### 可选依赖optional

1.  新建项目A(类似于mysql)、B(类似于oracle),项目C(类似于业务层,引入A、B工程),项目D(类似于逻辑工程,引入了C)
   C项目pom.xml如下：

   ```
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <parent>
           <artifactId>mavenTest</artifactId>
           <groupId>com.xiaozhi</groupId>
           <version>1.0-SNAPSHOT</version>
       </parent>
       <modelVersion>4.0.0</modelVersion>
   
       <artifactId>C</artifactId>
   
       <dependencies>
           <dependency>
               <groupId>com.xiaozhi</groupId>
               <artifactId>A</artifactId>
               <version>1.0-SNAPSHOT</version>
               <!-- optional=true,依赖不会传递，该项目依赖A
                   之后依赖该项目的项目如果想要使用A，需要重新引入 -->
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>com.xiaozhi</groupId>
               <artifactId>B</artifactId>
               <version>1.0-SNAPSHOT</version>
               <optional>true</optional>
           </dependency>
       </dependencies>
   </project>
   
   ```

   ![](http://120.77.237.175:9080/photos/maven/15.png)

详细说明以下问题

1. 由于projectC使用到了两个来自projectA的类(OptionalFeatureAClass)和projectB的类(OptionalFeatureBClass).如果projectC没有依赖packageA和packageB，那么编译将会失败。

2. projectD依赖projectC，但是对于projectD来说，类(OptionalFeatureAClass)和类(OptionalFeatureBClass)是可选的特性，所以为了让最终的war/ejbpackage不包含不必要的依赖，使用optional声明当前依赖是可选的,默认情况下也不会被其他项目继承(好比Java中的final类，不能被其他类继承一样)

   ![](http://120.77.237.175:9080/photos/maven/16.png)

3. 如果projectD确实需要用到projectC中的OptionalFeatureAClass怎么办呢？
   那我们就需要在projectD的pom.xml中显式的添加声明projectA依赖,继续看下图ProjectD需要用到ProjectA的OptionalFeatureAClass,那么需要在ProjectD的pom.xml文件中显式的添加对ProjectA的依赖
   ![](http://120.77.237.175:9080/photos/maven/17.png)

4. 到这也就很好理解为什么Maven为什么要设计optional关键字了,假设一个关于数据库持久化的项目(ProjectC),为了适配更多类型的数据库持久化设计,比如Mysql持久化设计(ProjectA)和Oracle持久化设计(ProjectB),当我们的项目(ProjectD)要用的ProjectC的持久化设计,不可能既引入mysql驱动又引入oracle驱动吧,所以我们要显式的指定一个,就是这个道理了
   

## 排除依赖

可以使用exclusions标签将传递过来的依赖排除出去

![](http://120.77.237.175:9080/photos/maven/18.png)

## 版本锁定[ 掌握 ]

1. 采用直接锁定版本的方法确定依赖jar包的版本，版本锁定后则不考虑依赖的声明顺序或依赖的
   路径，以锁定的版本为准添加到工程中，此方法在企业开发中经常使用

2.  版本锁定的使用方式：

   1. 在dependencyManagement标签中锁定依赖的版本

      ![](http://120.77.237.175:9080/photos/maven/19.png)

   2. 在dependencies标签中声明需要导入的maven坐标

      ![](http://120.77.237.175:9080/photos/maven/20.png)

# 分模块构建maven工程

## 分模块构建maven工程分析

不管是下面那种拆分方式,通常都会提供一个父工程，将一些公共的代码和配置提取到父工程中进行统一管理和配置

1. 按照业务模块进行拆分,每个模块拆分成一个maven工程,例如将一个项目分为用户模块、订单模块、购物车模块等,每个模块都对应一个maven工程
2. 按照层进行拆分,例如持久层、业务层、表现层等,每个层对应就是一个maven工程

![](http://120.77.237.175:9080/photos/maven/21.png)

## maven工程的继承

1. 在Java语言中，类之间是可以继承的，通过继承，子类就可以引用父类中非private的属性和方法。同样，在maven工程之间也可以继承，子工程继承父工程后，就可以使用在父工程中引入的依赖。继承的目的是为了消除重复代码
2. 被继承的maven工程通常被称为父工程,父工程的打包方式必须为pom,所以我们区分某个maven工程是否为父工程就要看这个工程的打包方式是否为pom
3. 继承其他maven父工程的通常称为子工程,在pom.xml文件中通过parent标签进行父工程的继承

![](http://120.77.237.175:9080/photos/maven/22.png)

##  maven工程的聚合

1.  在maven工程的pom.xml文件中可以使用<modules>标签将其他maven工程聚合到一起，聚合的目的是为了进行统一操作
2. 例如拆分后的maven工程有多个，如果要进行打包，就需要针对每个工程分别执行打包命令，操作起来非常繁琐。这时就可以使用modules标签将这些工程统一聚合到maven工程中，需要打包的时候，只需要在此工程中执行一次打包命令，其下被聚合的工程就都会被打包了
   ![](http://120.77.237.175:9080/photos/maven/23.png)