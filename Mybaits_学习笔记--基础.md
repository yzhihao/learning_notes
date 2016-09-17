# Mybaits_学习笔记
## 原理
[Mybaits——原理](http://www.iteye.com/blogs/subjects/mybatis_internals)<br>
[动态sql语句](http://www.jb51.net/article/71528.htm)见彪哥demo
##使用注解
[注解开发1](http://www.cnblogs.com/gossip/p/5198338.html)<br>
[注解开发2](http://blog.csdn.net/luanlouis/article/details/35780175#comments)

## dao层的作用
1. 对象能与数据库交互；
2. 能执行sql语句。


> 配置文件的详细路径：D:\Mybatis\mybatis-3-code_sources-3.4.1\src\test\java\org\apache\ibatis\submitted\complex_property
   
---------
## Mybatis之SqlSession
* SqlSession的作用：<br>
<1>向SQL语句传入参数<br>
<2>执行SQL语句<br>
<3>获取执行SQL语句的结果<br>
<4>事物的控制<br>
* 如何得到SqlSession：<br>
<1>通过配置文件获取数据库连接相关信息:<br>`Reader reader = Resources.getResourceAsReader("com/yezhihao/www/config/Configuration.xml")`;<br>
<2>通过配置信息构建SqlSessionFactory<br>
`SqlSessionFactory sqlSessionFactory = new SqlSessioFactoryBuilder.build(reader)`<br>
<3>通过SqlSessionFactory打开数据库会话<br>
`SqlSession sqlSession = sqlSeesionFactory.openSession()`<br>

## 常用的jdbc对于的type
	JDBC Type：           Java Type：  
	CHAR                String  
	VARCHAR             String  
	LONGVARCHAR         String  
	NUMERIC             java.math.BigDecimal  
	DECIMAL             java.math.BigDecimal  
	BIT             boolean  
	BOOLEAN             boolean  
	TINYINT             byte  
	SMALLINT            short  
	INTEGER             int  
	BIGINT              long  
	REAL                float  
	FLOAT               double  
	DOUBLE              double  
	BINARY              byte[]  
	VARBINARY           byte[]  
	LONGVARBINARY               byte[]  
	DATE                java.sql.Date  
	TIME                java.sql.Time  
	TIMESTAMP           java.sql.Timestamp  
	CLOB                Clob  
	BLOB                Blob  
	ARRAY               Array  
	DISTINCT            mapping of underlying type  
	STRUCT              Struct  
	REF                         Ref  
	DATALINK            java.net.URL[color=red][/color]

## 自增时插入
* 在Mybatis Mapper文件中添加属性“useGeneratedKeys”和“keyProperty”，其中keyProperty是Java对象的属性名，而不是表格的字段名。<br>

-------
   	<id="insert" parameterType="Spares"     
    useGeneratedKeys="true" keyProperty="id">    
    insert into system(name) values(#{name})   

* Mybatis执行完插入语句后，自动将自增长值赋值给对象systemBean的属性id。因此，可通过systemBean对应的getter方法获取！

----
	count = systemService.insert(systemBean);    
	int id = systemBean.getId(); //获取到的即为新插入记录的ID

##模糊查询
	select * from d_user where 
		<if test='name != "%null%"'>
			 name like #{name} and 
		</if>
		age between #{minAge} and #{maxAge}
## 缓存
>**详细看暑假学习的文档**<br>
一级缓存：<br>
执行方法 1. 一级缓存 : session级的缓存
> 缓存清空 <br>
 	

	1. 执行了session.clearCache();
 	2. 执行CUD操作
 	3. 不是同一个Session工厂对象
 * 二级缓存: 是一个映射文件级的缓存
 * 　1. 映射语句文件中的所有select语句将会被缓存。

2. 映射语句文件中的所有insert，update和delete语句会刷新缓存。

3. 缓存会使用Least Recently Used（LRU，最近最少使用的）算法来收回。

4. 缓存会根据指定的时间间隔来刷新。

5. 缓存会存储1024个对象

cache标签常用属性：

	<cache> 
	eviction="FIFO"  <!--回收策略为先进先出-->
	flushInterval="60000" <!--自动刷新时间60s-->
	size="512" <!--最多缓存512个引用对象-->
	readOnly="true"/> <!--只读-->
	</cache> 


## mybatis的大致原理--对JDBC的优化和封装：
1. 使用数据库连接池对连接进行管理
2. SQL语句统一存放到配置文件
3. SQL语句变量和传入参数的映射以及动态SQL
4. 动态SQL语句的处理
5. 对数据库操作结果的映射和结果缓存
6. SQL语句的重复
