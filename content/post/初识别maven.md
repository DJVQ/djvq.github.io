---
title: "初识maven"
date: 2021-06-26
tags: ["学习","maven"]
description: "初步学习maven"
---
# 目录
* [Project Object Model](#Project)
    * [简介](#简介)
* [基本配置](#基本配置)
    * [maven的协作相关属性](#maven的协作相关属性)
    * [pom之间的关系](#pom之间的关系)
        * [继承](#继承)
            * [被继承项目与继承项目是父子目录关系](#被继承项目与继承项目是父子目录关系)
            * [被继承项目与继承项目的目录结构不是父子关系](#被继承项目与继承项目的目录结构不是父子关系)
        * [聚合](#聚合)
            * [被聚合项目和子模块项目在目录结构上是父子关系](#被聚合项目和子模块项目在目录结构上是父子关系)
            * [被聚合项目与子模块项目在目录结构上不是父子关系](#被聚合项目与子模块项目在目录结构上不是父子关系)
            * [聚合与继承同时进行](#聚合与继承同时进行)
        * [依赖](#依赖)
          * [依赖管理](#依赖管理)
    * [属性](#属性)
* [构建配置](#构建配置)
* [项目信息](#项目信息)
* [环境目录](#环境配置)

## **Project Object Model**
### **简介**
Project Object Model 即项目对象类型，简写为 POM。pom.xml就是maven的配置文件，用以描述项目的各种信息。

## **基本配置**
**project** 是pom.xml中描述的根。
**modelVersion** 指定pom.xml符合哪个版本的描述符。maven 2 和 3只能为4.0.0。

### **maven的协作相关属性**
一个最简单的pom.xml的定义必须包含modelVersion、groupId、artifactId和version这四个元素，当然这其中的元素也是可以从它的父项目中继承的。在Maven中，使用groupId、artifactId和version组成groupdId:artifactId:version的形式来唯一确定一个项目：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"  
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0  
                      http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!-- 
        含义：组织标识，定义了项目属于哪个组，风向标，坐标，或者说若把本项目打包，一般对应项目包结构
        用途：此名称则是本地仓库中的路径，列如：otowa.user.dao，在M2_REPO目录下，将是: otowa/user/dao目录
        命名规范:项目名称，模块，子模块
    -->
    <groupId>otowa.user.dao</groupId>
    <!-- 
        含义：项目名称也可以说你所模块名称，定义当面Maven项目在组中唯一的ID
        用途：例如：user-dao，在M2_REPO目录下，将是：otowa/user/dao/user-dao目录
        命名规范:唯一就好
    -->
    <artifactId>user-dao</artifactId>
    <!-- 
        含义：项目当前的版本号
        用途：例如：0.0.1-SNAPSHOT，在M2_REPO目录下，将是：otowa/user/dao/user-dao/0.0.1-SNAPSHOT目录
        注意：不要在 artifactId 中包含点号(.)
    -->
    <version>0.0.1-SNAPSHOT</version>
    <!-- 打包的格式，可以为：pom , jar , maven-plugin , ejb , war , ear , rar , par -->
    <packaging>war</packaging>
    <!-- 元素声明了一个对用户更为友好的项目名称 -->
    <name>maven</name>
</project>
```
在maven中，根据groupId、artifactId、version组合成groupId:artifactId:version来唯一识别一个jar包。
对于version：

* maven 有自己的版本规范，一般是如下定义 major version、minor version、incremental version-qualifier ，比如 1.2.3-beta-01。要说明的是，maven 自己判断版本的算法是 major、minor、incremental 部分用数字比较，qualifier 部分用字符串比较，所以要小心 alpha-2 和 alpha-15 的比较关系，最好用 alpha-02 的格式。

* maven 在版本管理时候可以使用几个特殊的字符串 SNAPSHOT、LATEST、RELEASE。比如 1.0-SNAPSHOT。各个部分的含义和处理逻辑如下说明：
    * SNAPSHOT - 这个版本一般用于开发过程中，表示不稳定的版本。
    * LATEST - 指某个特定构件的最新发布，这个发布可能是一个发布版，也可能是一个 snapshot 版，具体看哪个时间最后。
    * RELEASE ：指最后一个发布版。


### **pom之间的关系**

Maven在建立项目的时候是基于Maven项目下的pom.xml进行的，我们项目依赖的信息和一些基本信息都是在这个文件里面定义的。那如果当我们有多个项目要进行，这多个项目有些配置内容是相同的，有些是要彼此关联的，那如果按照传统的做法的话我们就需要在多个项目中都定义这些重复的内容。这无疑是非常耗费时间和不易维护的。好在Maven给我们提供了一个pom的继承和聚合的功能。

要继承pom就需要有一个父pom，在Maven中定义了超级pom.xml，任何没有申明自己父pom.xml的pom.xml都将默认继承自这个超级pom.xml。

　　位置：

　　　　在Maven 2.xxx版本中，比如maven-2.0.9-uber.jar，打开此Jar文件后，在包包org.apache.maven.project下会有pom-4.0.0.xml的文件。

　　　　但是到了3.xxx版本之后在： aven安装目录下的：/lib/maven-model-builder-version.jar中 \org\apache\maven\mode目录中的pom-4.0.0.xml。

超级pom的定义：
```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

<!-- START SNIPPET: superpom -->
<project>
  <modelVersion>4.0.0</modelVersion>

  <repositories>
    <repository>
      <id>central</id>
      <name>Central Repository</name>
      <url>https://repo.maven.apache.org/maven2</url>
      <layout>default</layout>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
    </repository>
  </repositories>

  <pluginRepositories>
    <pluginRepository>
      <id>central</id>
      <name>Central Repository</name>
      <url>https://repo.maven.apache.org/maven2</url>
      <layout>default</layout>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <releases>
        <updatePolicy>never</updatePolicy>
      </releases>
    </pluginRepository>
  </pluginRepositories>

  <build>
    <directory>${project.basedir}/target</directory>
    <outputDirectory>${project.build.directory}/classes</outputDirectory>
    <finalName>${project.artifactId}-${project.version}</finalName>
    <testOutputDirectory>${project.build.directory}/test-classes</testOutputDirectory>
    <sourceDirectory>${project.basedir}/src/main/java</sourceDirectory>
    <scriptSourceDirectory>${project.basedir}/src/main/scripts</scriptSourceDirectory>
    <testSourceDirectory>${project.basedir}/src/test/java</testSourceDirectory>
    <resources>
      <resource>
        <directory>${project.basedir}/src/main/resources</directory>
      </resource>
    </resources>
    <testResources>
      <testResource>
        <directory>${project.basedir}/src/test/resources</directory>
      </testResource>
    </testResources>
    <pluginManagement>
      <!-- NOTE: These plugins will be removed from future versions of the super POM -->
      <!-- They are kept for the moment as they are very unlikely to conflict with lifecycle mappings (MNG-4453) -->
      <plugins>
        <plugin>
          <artifactId>maven-antrun-plugin</artifactId>
          <version>1.3</version>
        </plugin>
        <plugin>
          <artifactId>maven-assembly-plugin</artifactId>
          <version>2.2-beta-5</version>
        </plugin>
        <plugin>
          <artifactId>maven-dependency-plugin</artifactId>
          <version>2.8</version>
        </plugin>
        <plugin>
          <artifactId>maven-release-plugin</artifactId>
          <version>2.3.2</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>

  <reporting>
    <outputDirectory>${project.build.directory}/site</outputDirectory>
  </reporting>

  <profiles>
    <!-- NOTE: The release profile will be removed from future versions of the super POM -->
    <profile>
      <id>release-profile</id>

      <activation>
        <property>
          <name>performRelease</name>
          <value>true</value>
        </property>
      </activation>

      <build>
        <plugins>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-source-plugin</artifactId>
            <executions>
              <execution>
                <id>attach-sources</id>
                <goals>
                  <goal>jar</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-javadoc-plugin</artifactId>
            <executions>
              <execution>
                <id>attach-javadocs</id>
                <goals>
                  <goal>jar</goal>
                </goals>
              </execution>
            </executions>
          </plugin>
          <plugin>
            <inherited>true</inherited>
            <artifactId>maven-deploy-plugin</artifactId>
            <configuration>
              <updateReleaseInfo>true</updateReleaseInfo>
            </configuration>
          </plugin>
        </plugins>
      </build>
    </profile>
  </profiles>

</project>
<!-- END SNIPPET: superpom -->
```

由上面的超级pom.xml的内容我们可以看到pom.xml中没有groupId、artifactId和version的定义，所以在建立自己的pom.xml的时候就需要定义这三个元素。和java里面的继承类似，子pom.xml会完全继承父pom.xml中所有的元素，而且对于相同的元素，一般子pom.xml中的会覆盖父pom.xml中的元素，但是有几个特殊的元素它们会进行合并而不是覆盖。这些特殊的元素是：
* dependencies
* developers
* contributors
* plugin列表，包括plugin下面的* * reports
* resources

#### **继承**
##### **被继承项目与继承项目是父子目录关系**
现在假设有一个项目projectA，它的pom.xml定义如下：
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>com.tiantian.mavenTest</groupId>  
  <artifactId>projectA</artifactId>  
  <packaging>jar</packaging>  
  <version>1.0-SNAPSHOT</version>  
</project>
```
然后有另一个项目projectB，而且projectB是跟projectA的pom.xml文件处于同一个目录下，这时候如果projectB需要继承自projectA的话可以这样定义projectB的pom.xml文件。
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">  
  <parent>  
    <groupId>com.tiantian.mavenTest</groupId>  
    <artifactId>projectA</artifactId>  
    <version>1.0-SNAPSHOT</version>  
  </parent>  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>com.tiantian.mavenTest</groupId>  
  <artifactId>projectB</artifactId>  
  <packaging>jar</packaging>  
  <version>1.0-SNAPSHOT</version>  
</project>
```
由projectB的pom.xml文件的定义可以知道，当需要继承指定的一个Maven项目时，需要在pom.xml中定义一个parent元素，在这个元素中指明需要继承项目的groupId、artifactId和version。
##### **被继承项目与继承项目的目录结构不是父子关系**
当被继承项目与继承项目的目录结构不是父子关系的时候，再利用上面的配置是不能实现Maven项目的继承关系的，这个时候就需要在子项目的pom.xml文件定义中的parent元素下再加上一个relativePath元素的定义，用以描述父项目的pom.xml文件相对于子项目的pom.xml文件的位置。

　　假设有两个项目，projectA和projectB，projectB还是继承自projectA，但是现在projectB不在projectA的子目录中，而是与projectA处于同一目录中。这个时候projectA和projectB的目录结构如下：

```
　　------projectA

　　　　------pom.xml

　　------projectB

　　　　------pom.xml
```

　　这个时候我们可以看出projectA的pom.xml相对于projectB的pom.xml的位置是“../projectA/pom.xml”，所以这个时候projectB的pom.xml的定义应该如下所示：
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">  
  <parent>  
    <groupId>com.tiantian.mavenTest</groupId>  
    <artifactId>projectA</artifactId>  
    <version>1.0-SNAPSHOT</version>  
       <relativePath>../projectA/pom.xml</relativePath>  
  </parent>  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>com.tiantian.mavenTest</groupId>  
  <artifactId>projectB</artifactId>  
  <packaging>jar</packaging>  
  <version>1.0-SNAPSHOT</version>  
</project>
```
#### **聚合**
如果projectA聚合到projectB，那么就可以说projectA是projectB的子模块， projectB是被聚合项目，也可以类似于继承那样称为父项目。对于聚合而言，这个主体应该是被聚合的项目。所以，需要在被聚合的项目中定义它的子模块，而不是像继承那样在子项目中定义父项目。具体做法是：

修改被聚合项目的pom.xml中的packaging元素的值为pom
在被聚合项目的pom.xml中的modules元素下指定它的子模块项目
对于聚合而言，当在被聚合的项目上使用Maven命令时，实际上这些命令都会在它的子模块项目上使用。这就是Maven中聚合的一个非常重要的作用。假设这样一种情况，同时需要打包或者编译projectA、projectB、projectC和projectD，按照正常的逻辑一个一个项目去使用mvn compile或mvn package进行编译和打包，对于使用Maven而言，这样使用是非常麻烦的。因为Maven给我们提供了聚合的功能。只需要再定义一个超级项目，然后在超级项目的pom.xml中定义这个几个项目都是聚合到这个超级项目的。之后只需要对这个超级项目进行mvn compile，它就会把那些子模块项目都进行编译。
##### **被聚合项目和子模块项目在目录结构上是父子关系**
拿上面定义的projectA和projectB来举例子，现在假设需要把projectB聚合到projectA中。projectA和projectB的目录结构如下所示：
```
　　------projectA

　　　　------projectB

　　　　　　-----pom.xml

　　　　------pom.xml
```
　　这个时候projectA的pom.xml应该这样定义：
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>com.tiantian.mavenTest</groupId>  
  <artifactId>projectA</artifactId>  
  <version>1.0-SNAPSHOT</version>  
  <packaging>pom</packaging>  
  <modules>  
       <module>projectB</module>  
  </modules>  
</project>
```
由上面的定义我们可以看到被聚合的项目的packaging类型应该为pom，而且一个项目可以有多个子模块项目。对于聚合这种情况，我们使用子模块项目的artifactId来作为module的值，表示子模块项目相对于被聚合项目的地址，在上面的示例中就表示子模块projectB是处在被聚合项目的子目录下，即与被聚合项目的pom.xml处于同一目录。这里使用的module值是子模块projectB对应的目录名projectB，而不是子模块对应的artifactId。这个时候当我们对projectA进行mvn package命令时，实际上Maven也会对projectB进行打包。
##### **被聚合项目与子模块项目在目录结构上不是父子关系**
那么当被聚合项目与子模块项目在目录结构上不是父子关系的时候，应该怎么来进行聚合呢？还是像继承那样使用relativePath元素吗？答案是非也，具体做法是在module元素中指定以相对路径的方式指定子模块。下面一个例子:

　　继续使用上面的projectA和projectB，还是需要把projectB聚合到projectA，但是projectA和projectB的目录结构不再是父子关系，而是如下所示的这种关系：
```
　　------projectA

　　　　------pom.xml

　　------projectB

　　　　------pom.xml
```
　　这个时候projectA的pom.xml文件就应该这样定义：
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
   
  <groupId>com.tiantian.mavenTest</groupId>  
  <artifactId>projectA</artifactId>  
  <version>1.0-SNAPSHOT</version>  
  <packaging>pom</packaging>  
  <modules>  
       <module>../projectB</module>  
  </modules>  
</project>
```
注意看module的值是“../projectB”，“..”是代表当前目录的上层目录，所以它表示子模块projectB是被聚合项目projectA的pom.xml文件所在目录（即projectA）的上层目录下面的子目录，即与projectA处于同一目录层次。注意，这里的projectB对应的是projectB这个项目的目录名称，而不是它的artifactId。
##### **聚合与继承同时进行**
假设有这样一种情况，有两个项目，projectA和projectB，现在需要projectB继承projectA，同时需要把projectB聚合到projectA。然后projectA和projectB的目录结构如下：
```
------projectA

    ------pom.xml

------projectB

    ------pom.xml
```


那么这个时候按照上面说的那样，projectA的pom.xml中需要定义它的packaging为pom，需要定义它的modules，所以projectA的pom.xml应该这样定义：
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>com.tiantian.mavenTest</groupId>  
  <artifactId>projectA</artifactId>  
  <version>1.0-SNAPSHOT</version>  
  <packaging>pom</packaging>  
  <modules>  
       <module>../projectB</module>  
  </modules>  
</project>
```
而projectB是继承自projectA的，所以需要在projectB的pom.xml文件中新增一个parent元素，用以定义它继承的项目信息。所以projectB的pom.xml文件的内容应该这样定义：
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <parent>  
       <groupId>com.tiantian.mavenTest</groupId>  
       <artifactId>projectA</artifactId>  
       <version>1.0-SNAPSHOT</version>  
       <relativePath>../projectA/pom.xml</relativePath>  
  </parent>  
  <groupId>com.tiantian.mavenTest</groupId>  
  <artifactId>projectB</artifactId>  
  <version>1.0-SNAPSHOT</version>  
  <packaging>jar</packaging>  
</project>
```

#### **依赖**
依赖关系列表（dependency list）是POM的重要部分

项目之间的依赖是通过pom.xml文件里面的dependencies元素下面的dependency元素进行的。一个dependency元素定义一个依赖关系。在dependency元素中我们主要通过依赖项目的groupId、artifactId和version来定义所依赖的项目。

先来看一个简单的项目依赖的示例吧，假设现在有一个项目projectA，然后它里面有对junit的依赖，那么它的pom.xml就类似以下这个样子：
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
  <modelVersion>4.0.0</modelVersion>  
  <groupId>com.tiantian.mavenTest</groupId>  
  <artifactId>projectB</artifactId>  
  <version>1.0-SNAPSHOT</version>  
  <packaging>jar</packaging>  
   
  <dependencies>  
    <dependency>  
      <groupId>junit</groupId>  
      <artifactId>junit</artifactId>  
      <version>3.8.1</version>  
      <scope>test</scope>  
      <optional>true</optional>  
    </dependency>  
  </dependencies>  
</project>
```
**groupId**, **artifactId**, **version**:描述了依赖的项目唯一标志。

**type**：对应于依赖项目的packaging类型，默认是jar。

**scope**:用于限制相应的依赖范围、传播范围。scope的主要取值范围如下（还有一个是在Maven2.0.9以后版本才支持的import，关于import作用域将在后文《Dependency介绍》中做介绍）：

* **test**:在测试范围有效，它在执行命令test的时候才执行，并且它不会传播给其他模块进行引入，比如 junit,dbunit 等测试框架。
* **compile**(default 默认):这是它的默认值，这种类型很容易让人产生误解，以为只有在编译的时候才是需要的，其实这种类型表示所有的情况都是有用的，包括编译和运行时。而且这种类型的依赖性是可以传递的。
* **runtime**:在程序运行的时候依赖，在编译的时候不依赖。
* **provided**:这个跟compile很类似，但是它表示你期望这个依赖项目在运行时由JDK或者容器来提供。这种类型表示该依赖只有在测试和编译的情况下才有效，在运行时将由JDK或者容器提供。这种类型的依赖性是不可传递的。比如 javaee：

    * eclipse开发web环境中是没有javaee必须要手动添加。
    * myeclipse新建web项目会有JavaEE(servlet-api.jar,jsp-api.jar...)web容器依赖的jar包，一般都是做开发的时候才使用。但是myeclipse不会把这些 jar包发布的，lib下你是找不到javaee引入的jar包,因为myeclipse发布项目的时候会忽略它。为什么？因为tomcat容器bin/lib已经存在了这个jar包了。
* **system**：这种类型跟provided类似，唯一不同的就是这种类型的依赖要自己提供jar包，这需要与另一个元素systemPath来结合使用。systemPath将指向我们系统上的jar包的路径，而且必须是给定的绝对路径。
    * **systemPath**：上面已经说过了这个元素是在scope的值为system的时候用于指定依赖的jar包在系统上的位置的，而且是绝对路径。该元素必须在依赖的 jar包的scope为system时才能使用，否则Maven将报错。
    * **optional**：当该项目本身作为其他项目的一个依赖时标记该依赖为可选项。假设现在projectA有一个依赖项projectB，把projectB这个依赖项设为optional，这表示projectB在projectA的运行时不一定会用到。这个时候如果我们有另一个项目projectC，它依赖于projectA，那么这个时候因为projectB对于projectA是可选的，所以Maven在建立projectC的时候就不会安装projectB，这个时候如果projectC确实需要使用到projectB，那么它就可以定义自己对projectB的依赖。当一个依赖是可选的时候，我们把optional元素的值设为true，否则就不设置optional元素。
    * **exclusions**：考虑这样一种情况，的projectA依赖于projectB，然后projectB又依赖于projectC，但是在projectA里面不需要projectB依赖的projectC，那么这个时候我们就可以在依赖projectB的时候使用exclusions元素下面的exclusion排除projectC。这个时候我们可以这样定义projectA对projectB的依赖：
```xml
<dependencies>  
     <dependency>  
            <groupId>com.tiantian.mavenTest</groupId>  
            <artifactId>projectB</artifactId>  
            <version>1.0-SNAPSHOT</version>  
            <exclusions>  
                   <exclusion>  
                          <groupId>com.tiantian.mavenTest</groupId>  
                          <artifactId>projectC</artifactId>  
                   </exclusion>  
            </exclusions>  
     </dependency>  
</dependencies>
```

##### **依赖管理**

dependencyManagement 是表示依赖 jar 包的声明。即你在项目中的 dependencyManagement 下声明了依赖，maven 不会加载该依赖，dependencyManagement 声明可以被子 POM 继承。

dependencyManagement 的一个使用案例是当有父子项目的时候，父项目中可以利用 dependencyManagement 声明子项目中需要用到的依赖 jar 包，之后，当某个或者某几个子项目需要加载该依赖的时候，就可以在子项目中 dependencies 节点只配置 groupId 和 artifactId 就可以完成依赖的引用。如果某个子项目需要另外的一个版本，只需要声明version即可。dependencyManagement中定义的只是依赖的声明，并不实现引入，因此子项目需要显式的声明需要用的依赖。

dependencyManagement 主要是为了统一管理依赖包的版本，确保所有子项目使用的版本一致，类似的还有plugins和pluginManagement。


**举例**
在父项目的POM.xml中配置:
```xml
<dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
                <version>1.2.3.RELEASE</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

```

子项目则无需指定版本信息：
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

### **属性**
**在pom.xml文件中我们可以使用${propertyName}的形式引用属性。是值的占位符，类似EL，类似ant的属性，比如${X}，可用于pom文件任何赋值的位置。有以下分类：**

* **env.propertyName**：这种形式表示引用的是环境变量，比如我们需要引用当前系统的环境变量PATH的时候，就可以使用${env.PATH}。
* **project.propertyName**：这种形式表示引用的是当前这个pom.xml中project根元素下面的子元素的值。比如我们需要引用当前project下面的version的时候，就可以使用${project.version}。
* **settings.propertyName**：这种形式引用的是Maven本地配置文件settings.xml或本地Maven安装目录下的settings.xml文件根元素settings下的元素。比如我们需要引用settings下的本地仓库localRepository元素的值时，我们可以用${settings.localRepository}
* **Java System Properties**：java的系统属性，所有在java中使用java.lang.System.getProperties()能够获取到的属性都可以在pom.xml中引用，比如${java.home}。
* **自定义**：pom.xml中properties元素下面的子元素作为属性。假如在pom.xml中有如下一段代码<properties><hello.world>helloWorld</hello.world></properties>，那么我们就可以使用${hello.world}引用到对应的helloWorld。

## **构建配置**
**build**

build 可以分为 "project build" 和 "profile build"。
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <!-- "Project Build" contains more elements than just the BaseBuild set -->
  <build>...</build>

  <profiles>
    <profile>
      <!-- "Profile Build" contains a subset of "Project Build"s elements -->
      <build>...</build>
    </profile>
  </profiles>
</project>
```

基本构建配置：
```xml
<build>
  <defaultGoal>install</defaultGoal>
  <directory>${basedir}/target</directory>
  <finalName>${artifactId}-${version}</finalName>
  <filters>
    <filter>filters/filter1.properties</filter>
  </filters>
  ...
</build>
```

**defaultGoal** : 默认执行目标或阶段。如果给出了一个目标，它应该被定义为它在命令行中（如 jar：jar）。如果定义了一个阶段（如安装），也是如此。

**directory** ：构建时的输出路径。默认为：${basedir}/target 。

**finalName** ：这是项目的最终构建名称（不包括文件扩展名，例如：my-project-1.0.jar）

**filter** ：定义 * .properties 文件，其中包含适用于接受其设置的资源的属性列表（如下所述）。换句话说，过滤器文件中定义的“name = value”对在代码中替换\$ {name}字符串。

**resources**

资源的配置。资源文件通常不是代码，不需要编译，而是在项目需要捆绑使用的内容。
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <build>
    ...
    <resources>
      <resource>
        <targetPath>META-INF/plexus</targetPath>
        <filtering>false</filtering>
        <directory>${basedir}/src/main/plexus</directory>
        <includes>
          <include>configuration.xml</include>
        </includes>
        <excludes>
          <exclude>**/*.properties</exclude>
        </excludes>
      </resource>
    </resources>
    <testResources>
      ...
    </testResources>
    ...
  </build>
</project>
```

* **resources**: 资源元素的列表，每个资源元素描述与此项目关联的文件和何处包含文件。
* **targetPath**: 指定从构建中放置资源集的目录结构。目标路径默认为基本目录。将要包装在 jar 中的资源的通常指定的目标路径是 META-INF。
* **filtering**: 值为 true 或 false。表示是否要为此资源启用过滤。请注意，该过滤器 * .properties 文件不必定义为进行过滤 - 资源还可以使用默认情况下在 POM 中定义的属性（例如\$ {project.version}），并将其传递到命令行中“-D”标志（例如，“-Dname = value”）或由 properties 元素显式定义。过滤文件覆盖上面。
* **directory**: 值定义了资源的路径。构建的默认目录是${basedir}/src/main/resources。
* **includes**: 一组文件匹配模式，指定目录中要包括的文件，使用*作为通配符。
* **exclude**s: 与 includes 类似，指定目录中要排除的文件，使用*作为通配符。注意：如果 include 和 exclude 发生冲突，maven 会以 exclude 作为有效项。
* **testResources**: testResources 与 resources 功能类似，区别仅在于：testResources 指定的资源仅用于 test 阶段，并且其默认资源目录为：${basedir}/src/test/resources 。

**plugins**
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <build>
    ...
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-jar-plugin</artifactId>
        <version>2.6</version>
        <extensions>false</extensions>
        <inherited>true</inherited>
        <configuration>
          <classifier>test</classifier>
        </configuration>
        <dependencies>...</dependencies>
        <executions>...</executions>
      </plugin>
    </plugins>
  </build>
</project>
```

* **groupId, artifactId, version**：和基本配置中的 groupId、artifactId、version 意义相同。
* **extensions** ：值为 true 或 false。是否加载此插件的扩展名。默认为 false。
* **inherited** ：值为 true 或 false。这个插件配置是否应该适用于继承自这个插件的 POM。默认值为 true。
* **configuration**：针对个人插件的配置，这里不扩散讲解。
* **dependencies** ：这里的 dependencies 是插件本身所需要的依赖。
* **executions**：需要记住的是，插件可能有多个目标。每个目标可能有一个单独的配置，甚至可能将插件的目标完全绑定到不同的阶段。执行配置插件的目标的执行。
  * **id**: 执行目标的标识。
  * **goals**: 像所有多元化的 POM 元素一样，它包含单个元素的列表。在这种情况下，这个执行块指定的插件目标列表。
  * **phase**: 这是执行目标列表的阶段。这是一个非常强大的选项，允许将任何目标绑定到构建生命周期中的任何阶段，从而改变 maven 的默认行为。
  * **inherited**: 像上面的继承元素一样，设置这个 false 会阻止 maven 将这个执行传递给它的子代。此元素仅对父 POM 有意义。
  * **configuration**: 与上述相同，但将配置限制在此特定目标列表中，而不是插件下的所有目标。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <build>
    <plugins>
      <plugin>
        <artifactId>maven-antrun-plugin</artifactId>
        <version>1.1</version>
        <executions>
          <execution>
            <id>echodir</id>
            <goals>
              <goal>run</goal>
            </goals>
            <phase>verify</phase>
            <inherited>false</inherited>
            <configuration>
              <tasks>
                <echo>Build Dir: ${project.build.directory}</echo>
              </tasks>
            </configuration>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
</project>
```

**pluginManagement**

与 dependencyManagement 很相似，在当前 POM 中仅声明插件，而不是实际引入插件。子 POM 中只配置 groupId 和 artifactId 就可以完成插件的引用，且子 POM 有权重写 pluginManagement 定义。它的目的在于统一所有子 POM 的插件版本。

**directories**
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <build>
    <sourceDirectory>${basedir}/src/main/java</sourceDirectory>
    <scriptSourceDirectory>${basedir}/src/main/scripts</scriptSourceDirectory>
    <testSourceDirectory>${basedir}/src/test/java</testSourceDirectory>
    <outputDirectory>${basedir}/target/classes</outputDirectory>
    <testOutputDirectory>${basedir}/target/test-classes</testOutputDirectory>
    ...
  </build>
</project>
```

目录元素集合存在于 build 元素中，它为整个 POM 设置了各种目录结构。由于它们在配置文件构建中不存在，所以这些不能由配置文件更改。

如果上述目录元素的值设置为绝对路径（扩展属性时），则使用该目录。否则，它是相对于基础构建目录：${basedir}。

**extensions**

扩展是在此构建中使用的 artifacts 的列表。它们将被包含在运行构建的 classpath 中。它们可以启用对构建过程的扩展（例如为 Wagon 传输机制添加一个 ftp 提供程序），并使活动的插件能够对构建生命周期进行更改。简而言之，扩展是在构建期间激活的 artifacts。扩展不需要实际执行任何操作，也不包含 Mojo。因此，扩展对于指定普通插件接口的多个实现中的一个是非常好的。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <build>
    ...
    <extensions>
      <extension>
        <groupId>org.apache.maven.wagon</groupId>
        <artifactId>wagon-ftp</artifactId>
        <version>1.0-alpha-3</version>
      </extension>
    </extensions>
    ...
  </build>
</project>
```

**reporting**

报告包含特定针对 site 生成阶段的元素。某些 maven 插件可以生成 reporting 元素下配置的报告，例如：生成 javadoc 报告。reporting 与 build 元素配置插件的能力相似。明显的区别在于：在执行块中插件目标的控制不是细粒度的，报表通过配置 reportSet 元素来精细控制。

而微妙的区别在于 reporting 元素下的 configuration 元素可以用作 build 下的 configuration ，尽管相反的情况并非如此（ build 下的 configuration 不影响 reporting 元素下的 configuration ）。

另一个区别就是 plugin 下的 outputDirectory 元素。在报告的情况下，默认输出目录为 ${basedir}/target/site。
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <reporting>
    <plugins>
      <plugin>
        ...
        <reportSets>
          <reportSet>
            <id>sunlink</id>
            <reports>
              <report>javadoc</report>
            </reports>
            <inherited>true</inherited>
            <configuration>
              <links>
                <link>http://java.sun.com/j2se/1.5.0/docs/api/</link>
              </links>
            </configuration>
          </reportSet>
        </reportSets>
      </plugin>
    </plugins>
  </reporting>
  ...
</project>
```

## **项目信息**
项目信息相关的这部分标签都不是必要的，也就是说完全可以不填写。

它的作用仅限于描述项目的详细信息。

下面的示例是项目信息相关标签的清单：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...

  <!-- 项目信息 begin -->

  <!--项目名-->
  <name>maven-notes</name>

  <!--项目描述-->
  <description>maven 学习笔记</description>

  <!--项目url-->
  <url>https://github.com/dunwu/maven-notes</url>

  <!--项目开发年份-->
  <inceptionYear>2017</inceptionYear>

  <!--开源协议-->
  <licenses>
    <license>
      <name>Apache License, Version 2.0</name>
      <url>https://www.apache.org/licenses/LICENSE-2.0.txt</url>
      <distribution>repo</distribution>
      <comments>A business-friendly OSS license</comments>
    </license>
  </licenses>

  <!--组织信息(如公司、开源组织等)-->
  <organization>
    <name>...</name>
    <url>...</url>
  </organization>

  <!--开发者列表-->
  <developers>
    <developer>
      <id>victor</id>
      <name>Zhang Peng</name>
      <email>forbreak at 163.com</email>
      <url>https://github.com/dunwu</url>
      <organization>...</organization>
      <organizationUrl>...</organizationUrl>
      <roles>
        <role>architect</role>
        <role>developer</role>
      </roles>
      <timezone>+8</timezone>
      <properties>...</properties>
    </developer>
  </developers>

  <!--代码贡献者列表-->
   <contributors>
    <contributor>
      <!--标签内容和<developer>相同-->
    </contributor>
  </contributors>

  <!-- 项目信息 end -->

  ...
</project>
```

这部分标签都非常简单，基本都能做到顾名思义，且都属于可有可无的标签，所以这里仅简单介绍一下：

* **name** - 项目完整名称
* **description** - 项目描述
* **url** - 一般为项目仓库的 host
* **inceptionYear** - 开发年份
* **licenses** - 开源协议
* **organization** - 项目所属组织信息
* **developers** - 项目开发者列表
* **contributors** - 项目贡献者列表， 的子标签和 的完全相同。


## **环境配置**
**issueManagement**
这定义了所使用的缺陷跟踪系统（Bugzilla，TestTrack，ClearQuest 等）。虽然没有什么可以阻止插件使用这些信息的东西，但它主要用于生成项目文档。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <issueManagement>
    <system>Bugzilla</system>
    <url>http://127.0.0.1/bugzilla/</url>
  </issueManagement>
  ...
</project>
```

**ciManagement**
CI 构建系统配置，主要是指定通知机制以及被通知的邮箱。
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <ciManagement>
    <system>continuum</system>
    <url>http://127.0.0.1:8080/continuum</url>
    <notifiers>
      <notifier>
        <type>mail</type>
        <sendOnError>true</sendOnError>
        <sendOnFailure>true</sendOnFailure>
        <sendOnSuccess>false</sendOnSuccess>
        <sendOnWarning>false</sendOnWarning>
        <configuration><address>continuum@127.0.0.1</address></configuration>
      </notifier>
    </notifiers>
  </ciManagement>
  ...
</project>
```

**mailingLists**
邮件列表
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <mailingLists>
    <mailingList>
      <name>User List</name>
      <subscribe>user-subscribe@127.0.0.1</subscribe>
      <unsubscribe>user-unsubscribe@127.0.0.1</unsubscribe>
      <post>user@127.0.0.1</post>
      <archive>http://127.0.0.1/user/</archive>
      <otherArchives>
        <otherArchive>http://base.google.com/base/1/127.0.0.1</otherArchive>
      </otherArchives>
    </mailingList>
  </mailingLists>
  ...
</project>
```

**scm**
SCM（软件配置管理，也称为源代码/控制管理或简洁的版本控制）。常见的 scm 有 svn 和 git 。
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <scm>
    <connection>scm:svn:http://127.0.0.1/svn/my-project</connection>
    <developerConnection>scm:svn:https://127.0.0.1/svn/my-project</developerConnection>
    <tag>HEAD</tag>
    <url>http://127.0.0.1/websvn/my-project</url>
  </scm>
  ...
</project>
```

**prerequisites**
POM 执行的预设条件。
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <prerequisites>
    <maven>2.0.6</maven>
  </prerequisites>
  ...
</project>
```

**repositories**
repositories 是遵循 Maven 存储库目录布局的 artifacts 集合。默认的 Maven 中央存储库位于https://repo.maven.apache.org/maven2/上。
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <repositories>
    <repository>
      <releases>
        <enabled>false</enabled>
        <updatePolicy>always</updatePolicy>
        <checksumPolicy>warn</checksumPolicy>
      </releases>
      <snapshots>
        <enabled>true</enabled>
        <updatePolicy>never</updatePolicy>
        <checksumPolicy>fail</checksumPolicy>
      </snapshots>
      <id>codehausSnapshots</id>
      <name>Codehaus Snapshots</name>
      <url>http://snapshots.maven.codehaus.org/maven2</url>
      <layout>default</layout>
    </repository>
  </repositories>
  <pluginRepositories>
    ...
  </pluginRepositories>
  ...
</project>
```

**pluginRepositories**
与 repositories 差不多。
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <distributionManagement>
    ...
    <downloadUrl>http://mojo.codehaus.org/my-project</downloadUrl>
    <status>deployed</status>
  </distributionManagement>
  ...
</project>
```

**distributionManagement**
它管理在整个构建过程中生成的 artifact 和支持文件的分布。从最后的元素开始：
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <distributionManagement>
    ...
    <downloadUrl>http://mojo.codehaus.org/my-project</downloadUrl>
    <status>deployed</status>
  </distributionManagement>
  ...
</project>
```

repository - 与 repositories 相似
site - 站点信息
relocation - 项目迁移位置
profiles
activation 是一个 profile 的关键。配置文件的功能来自于在某些情况下仅修改基本 POM 的功能。这些情况通过 activation 元素指定。
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      https://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <profiles>
    <profile>
      <id>test</id>
      <activation>
        <activeByDefault>false</activeByDefault>
        <jdk>1.5</jdk>
        <os>
          <name>Windows XP</name>
          <family>Windows</family>
          <arch>x86</arch>
          <version>5.1.2600</version>
        </os>
        <property>
          <name>sparrow-type</name>
          <value>African</value>
        </property>
        <file>
          <exists>${basedir}/file2.properties</exists>
          <missing>${basedir}/file1.properties</missing>
        </file>
      </activation>
      ...
    </profile>
  </profiles>
</project>
```
