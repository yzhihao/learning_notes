#hibernate_学习笔记
>hibernate.show_sql （可以简写为：show_sql） : 是否将sql语句输出到控制台上; hibernate.format_sql ( format_sql )。hbm2ddl.auto: 帮助由Java代码生成数据库脚本，生成具体的表结构。create：删除表里面的数据，重新插入数据。update: 不删除表中的数据，更新数据
[各种查询的方式](http://www.cnblogs.com/shiyangxt/archive/2009/01/13/1375151.html)

##Hibernate执行流程
1. 创建Configuration配置对象（读取hibernate.cfg.xml文件）
2. 创建SessionFactory工厂会话对象（读取User.hbm.xml文件）
3. 创建Session对象（数据库连接，类似于JDBC中的Connection），可执行增删改查,一对多的关系。
4. 执行事务后，可提交，关闭事务


##transaction简介：
1. Hibernate推荐使用手工开启，提交事物的方式<br>
a.transaction=session.beginTransaction();<br>
b.transaction.commit();
2. 如果使用自动提交的方式，需要调用doWork()方法,并且要求刷新session.flush();

----
	session.doWork(new Work() {
	@Override
	public void execute(Connection connection) throws SQLException {
	connection.setAutoCommit(true);
	}
	});
	session.flush();
![](http://img.mukewang.com/5785e52a000187ac12800720.jpg)

##session
openSession 每次使用都是打开一个新的session，使用完需要调用close方法关闭session；<br>
getCurrentSession 是获取当前session对象，连续使用多次时，得到的session都是同一个对象，这就是与openSession的区别之一<br>
一般在实际开发中，往往使用getCurrentSession多，因为一般是处理同一个事务，所以在一般情况下比较少使用openSession；
如何获得session对象？？？
（1）openSessionion
（2）getCurrentSession


如果使用getCurrentSession需要在hibernate.cfg.xml文件中进行配置：
如果是本地事务（jdbc事务）
`<property name="hibernate.current_session_context_class">thread</property>`
如果是全局事务（jta事务）
`<property name="hibernate.current_session_context_class">jta</property>`

##penSession与getCurrentSession的区别<br>
1. getCurrentSession使用了到了单例模式使用现有的对象，虽然每一次运行都不一样，但同一个方法当中获取的session是一样的，无需手动关闭，commit的就立即关闭并且，在openSession之前就可获取，可见，Hibernate早早地准备好了session对象<br>
2. openSession每次创建新的session对象，必须要手动关闭，否则时间一长会照常连接池溢出<br>


##单表CRUD操作实例：
增加操作：save方法<br>
修改操作：update方法<br>
删除操作：delete方法<br>
查询操作：get/load方法（查询单个记录）<br>

#get和load区别：<br>
1、get不考虑缓存，在调用后立即向数据库发出sql语句。返回持久化对象。（持久化对象：本身）<br>
查询对象在数据库中不存在时，返回是你null。<br>
2、load方法会在调用后返回一个代理对象，该对象只保存了尸体对象的id，直到调用对象非主键属性时才会发出sql语句。（代理对象：替代品）<br>
查询对象在数据库中不存在时，返回是一个异常。<br>
<br>

##nverse属性：<br>
有时候我们会发现，如果使用了多对一关系，同时也使用一对多的关系的时候，会产生不必要的代码（例如update）<br>
inverse属性设置为true的时候代表的是只给某一方进行维护，而不是双方
。
就是在一对多的时候，在一方默认就会维护已存在数据库的多方（在有用set.add（student）时）。
<set name="students" table="student" inverse="true">
<key column="classId"></key>
<one-to-many class="com.pro.domain.Student"/
![](http://img.mukewang.com/572b333500011dc512800720.jpg)



##什么是缓存：
1. 并不是指计算机的内存或者CPU的一二级缓存；缓存是指为了降低应用程序对物理数据源访问的频次从而提高应用程序的运行性能的一种策略

###为什么使用缓存：
1. ORM框架访问数据库的效率直接影响应用程序的运行速度，提升和优化ORM框架的执行效率至关重要
2. Hibernate的缓存是提升和优化Hibernate执行效率的重要手段，所以学会Hibernate缓存的使用和配置是优化的关键
3. 评判一个ORM框架是否优秀，访问数据库的频次就一个重要的标准

###缓存的一般工作原理：
1. 缓存是在计算机内存当中
2. 如图
![](http://img.mukewang.com/5780d86100013d7212800720.jpg)

###Hibernate缓存：
1. Hibernate缓存与session相关，同一个session第二次访问同一个对象将使用缓存
2. 在不同的session中多次查询同一个对象时，会执行多次数据库查询
3. 在一级缓存当中，持久化类的每个实例都具有唯一的OID，也就是说同一个session两次查询同一个对象时，第二次是不会再将对象保存在缓存当中的

##介绍Hibernate一级缓存：
1. Hibernate一级缓存又称为"Session缓存","会话级缓存"
2. 通过Session从数据库查询实体时把实体在内存中存储起来，下一次查询同一实体时不再从数据库获取，而是从内存中获取，这就是缓存
3. 一级缓存的生命周期和Session相同;Session销毁，他也销毁
4. 一级缓存中的数据可适用范围在当前会话之内

##Hibernate一级缓存的API
###一级缓存无法取消，Hibernate默认使用一级缓存
2. 用两个方法管理一级缓存：

---
	a.evict(emp)：用于将某个对象从Session的一级缓存中清除
	b.clear():用于将一节缓存中的所有对象全部清除
3. 一级缓存也有些时候会对程序的性能产生影响，因为在对数据库进行增删改的时候同时也要更新缓存
![](http://img.mukewang.com/5709c058000196f712800720.jpg)
###一级缓存注意问题：
1.query.list()是不会使用一级缓存的（即用QHL时）
2.query.iterate()会使用一级缓存，当缓存中有数据的时候，query.iterate()将所有对象的id查询出来然后到缓存中将所有对象都查询出来，如果缓存中没有数据，query.iterate()则把对象从数据库中一条一条的将数据查出来

##二级缓存的配置步骤：
* 添加二级缓存对应的jar包ehcache.jar
* 在Hibernate的配置文件中添加Provider类的描述
* 添加二级缓存的属性配置文件
* 在需要被缓存的表所对应的映射文件中添加<cache/>标签
![](http://img.mukewang.com/56cd99120001d3f912800720.jpg)

##二级缓存的介绍：
1. 二级缓存又称为“全局缓存”，“应用级缓存”
2. 二级缓存中的数据可适用方位是当前应用的所有会话，不会虽然某一个session会话的关闭而关闭，而是随着整个sessionFactory的关闭而关闭
3. 二级缓存是可插拔式缓存，默认是EHCache,还支持其他二级缓存组件

##二级缓存的适用场景：
1. 很少被修改的数据
2. 不是很重要的数据，允许出现偶尔并发的数据
3. 不会被高并发访问的数据
4. 参考数据

##一二级缓存的对比：
如图
![](http://img.mukewang.com/5709cafd00018f5412800720.jpg)
总结：
1. Hibernate的缓存能提高检索效率
2. Hibernate的缓存分为一级缓存和二级缓存，一级缓存是会话级缓存，二级缓存是应用级缓存
3. Hibernate的缓存在提高检索的同时，也会增加服务器的消耗，所以要注意缓存的使用策略。
>**想看慕课的缓存策略**
