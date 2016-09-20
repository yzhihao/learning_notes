# maven
## 常用命令
* mvn -v :查看mvn版本信息<br>
* mvn compile :对项目源文件编译<br>
* mvn test :测试项目<br>
* mvn package : 项目打包,存放在target 目录下
* mvn clean :删除target目录下项目打包的文件
* mvn install:安装jar包到本地仓库中.
* install安装jar到本地仓库 配置依赖 *<dependencies>* install通过配置的依赖坐标先在本地仓库查找对应的jar包，找不到到maven的线上中央仓库中查找下载，然后添加到ClassPath中
* 先看pox.xml中依赖的坐标，然后上仓库中找有没有该jar包，若有则将该jar包引入到classpath中，若没有则到网上的maven仓库中查找是否有该jar包并下载，并放到本地仓库，供项目使用

## 自动创建目录
1. archetype:gennerate
-DgroupId=组织名，公司网址的反写+项目名
-DartifactId=项目名-模块名
-Dversion=版本号
-Dpackage=代码所存在的包名
2. archetype:generate 按照提示进行选择
## 生命周期
[常见的插件](http://my.oschina.net/u/1387007/blog/519581)<br>
[插件讲解](http://blog.csdn.net/tounaobun/article/details/8960964)<br>
* MAVEN生命周期：<br>
>clean	清理项目<br>
default	构建项目<br>
site	生成项目站点<br>

* clean->compile->test->package->install<br>
>clean 清理项目：<br>
pre-clean:执行清理前的工作<br>
clean:清理上一次构建生成的所有文件<br>
post-clean:执行清理后的文件<br>

[可看生命周期详解](http://www.cnblogs.com/holbrook/archive/2012/12/24/2830519.html#sec-1)

* default 构建项目：
>compile test package install<br>
site 生成项目站点：<br>
pre-size:在生成项目站点前要完成的工作<br>
site:生成项目的站点文档<br>
post-size:在生成项目站点后要完成的工作<br>
site-deploy:发布生成的站点到服务器上<br>

* maven plugins站点：<br>
https://maven.apache.org/plugins/index.html<br>
**例如**：*要在项目中增加source（项目源代码打包）*
maven-source-plugin:<br>
https://maven.apache.org/plugins/maven-source-plugin/<br>
* 在项目pom.xml中添加如下xml配置信息（在打包时）：
	<build>
	<plugins>
	<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-source-plugin</artifactId>
	<version>2.4</version>
	<executions>
	<execution>
	<phase>package</phase>
	<goals>
	<goal>jar-no-fork</goal>
	</goals>
	</execution>
	</executions>
	</plugin>
	</plugins>
	</build>
* 配置jdk的版本：
	<build>  
      <plugins>  
        <plugin>  
            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-compiler-plugin</artifactId>  
            <version>2.3.2</version>  
            <configuration>  
                <source>1.6</source>  
                <target>1.6</target>  
            </configuration>  
        </plugin>  
      </plugins>  
    </build>


## pom.xml解析
* `<modelVersion>4.0.0</modelVersion>`
* `<groupId></groupId><!--主项目标识：反写公司网址+项目名-->`
* `<artifactId></artifactId><!--项目名+模块名-->`
* <version>0.0.1-SNAPSHOT</version><!--当前版本项目本：大版本 号.分支版本号.小版本号;SNAPSHOT:快照，ALPHA:内部测试,BETA:公 测,RELEASE:稳定版,GA:正式发布-->
* <packaging>jar</packaging><!--打包方式，默认为jar。 其他.war,zip,pom-->
* <!--项目描述名-->
<name>hi</name>
* <!--项目地址-->
<url></url>
* <!--项目描述-->
<description></description>
* <!--开发人员信息-->
<developers></developers>
* <!--许可证信息-->
<licenses></licenses>
<!--组织信息-->
* **<!-- 依赖列表 -->**
<dependencies>
<dependency>
<groupId>junit</groupId>
<artifactId>junit</artifactId>
<version>3.8.1</version>
<type></type>
<scope>test（只在test中有用）</scope>
* `<!-- 设置依赖是否可选，默认为false, -->
<optional></optional>`
* `<!-- 排除依赖传递列表 -->
	<exclusions>
		<exclusion>
		坐标
		</exclusion>
	</exclusions>

</dependency>
</dependencies>`
* <!-- 插件列表 -->
<plugins></plugins>
</build>
* <!-- 在子模块中对父模块POM的继承 -->
<parent></parent>
* <!-- 多模块 -->
<modules></modules>
