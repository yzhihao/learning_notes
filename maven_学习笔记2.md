#maven_学习笔记2
##scope依赖范围：
* compile:编译，默认的范围，编译测试运行都有效
* provided:测试和编译时有效运行期由容器提供，如servlet-api包<br>
* runtime:测试和运行时有效
* test:测试，只在测试时有效,如junit包<br>
* system:测试和编译时有效,可移植性差，与本机想关联
* import：导入的范围，只使用在dependencyManagement中，表示从其他的pom中导入dependency
* 的配置
 

## 排除依赖
* 传递依赖：简单讲就是间接依赖关系，比如：B依赖A，C依赖B，那么C也就依赖A了，C和A的依赖关系就是传递依赖。<br>
* Maven对于依赖的管理是这样的，当在POM.XML文件中发现配置了，某个依赖，就先去自己本地的依赖仓库中去找对应的依赖，如果没找到，就去Maven的中央依赖仓库中去找，如果还是没找到，就会生气报错。<br>
* 对于项目而言，比如上面的例子A/B/C我们需要在B的POM.XML依赖关系中配置上A的坐标，并且需要对A进行编译、打包、安装到本地仓库等工作，B才能实现对A的依赖。C依赖与B，并且B依赖与A，C的依赖库里会自动的将Ａ项目的jar包也导进来的。如果我们不想这样，那么就需要用到排除依赖这个标签了。
* <exclusion></exclusion>——此标签就是排除对传递依赖的依赖关系的一种方式。
>**例如**
	<dependency>
	     <groupId>com.topview.webtest</groupId>
  		 <artifactId>maven_test</artifactId>
	     <version>0.0.1-SNAPSHOT</version>
	       <exclusions>
		   <exclusion>
			   <groupId>com.topview.maven_test</groupId>
	  		   <artifactId>maven_test</artifactId>
		   </exclusion>
		   </exclusions>
    </dependency>

-------
## 依赖冲突：
>是指间接依赖关系中依赖同一个依赖，或者同一个依赖的不同版本的情况，此时我们就需要判断到底依赖那一个依赖，如下是选择的两条原则<br>

-------------------------------
* 如果A通过依赖传递的关系通过不同的路径依赖同一个依赖Ｂ。<br>
①：短路优先：<br>
会优先解析路径短的版本，比如：<br>
A -> B -> C -> X(jar)<br>
A -> D -> X(jar) 优先解析短的<br>
* 如果路径长度相同，则谁先声明，先解析谁——根据在依赖文件ＰＯＭ．ＸＭＬ中声明的先后顺序来选择依赖
②：先声明先优先<br>
## 聚合继承

* 聚合：如果项目D依赖项目C，项目C依赖项目B，项目B依赖项目A，我们需要一个个安装这项项目，在Maven中有一种方式可以将多个项目一次性安装，这就是聚合的概念。简单讲就是，需要人工多次操作的，只要MAVEN能理解，一次性告诉他，他就能帮我们做这件单调烦人的事情了。——使用
<modules><module>../(项目名)</module></modules>这个标签<br>
* *将一系列小的模块聚合成整个产品。*
* 通过聚合后的工程可以同时管理每个相关模块的构建、清理、文档等工作。 聚合关系通过在子工程中指定一个pom类型的project作为父project来定义。

-------
* 继承：多次使用到的依赖，比如：单元测试，没有必要在所有的项目中都引用一下，此时就可以采用继承的方式来实现，先来一个父级的POM.XML然后再继承此POM.XML
* 对于继承父类的时候，需要引用 <relativePath>../maven-parent/pom.xml</relativePath>
否则会报:
Non-resolvable parent POM: Could not find artifact

## **其他注意**
* 依赖多个其他工程，必须通过工程的唯一标识（groupId+artifactId+version)<br>