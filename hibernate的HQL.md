# hibernate的HQL
**分页和模糊查询见项目hqlBasic**
##几个常用的数据查询子句：
1. 检索对象：from子句
2. 选择：select子句
3. 限制：where子句
4. 排序：order by子句

##@HQL语句形式：
select子句：用来指定查询结果中的对象和属性，并指定以何种数据类型返回 （在最前面）<br>
from子句：用来指定hql语句的查询目标，即映射配置的持久化类及其属性<br>
where子句：逻辑表达式，用来设置查询条件，限制返回结果和范围<br>
group by子句：分组查询语句<br>
having子句：对分组进行限制条件设置<br>
order by子句：用来指定查询结果中的实例对象的排序<br>
Ps：最简单的HQL语句形式只要有from就可以了，其他的都可以省略。这是与SQL不同之处。

##注意
1. HQL是面向对象的查询语句，对Java类与属性大小写敏感
2. HQL对关键字不区分大小写，习惯上一律小写

##Query接口简介
org.hibernate.Query接口
1.	Query接口定义有执行查询的方法
2.	Query接口支持方法链编程风格，使得程序代码更为简洁
Query实例的创建
1、Session的createQuery（）方法创建Query实例
2、createQuery方法包含一个HQL语句参数，createQuery（hql）
Query执行查询
1．Query接口的list（）方法执行HQL查询
2．list方法返回结果数据类型为java.util.List，List集合中存放符合查询条件的持久化对象

>HQL的持久化的引用过程中，直接引用类名即可（当然也可用全限定名），因为Hibernate通过映射，会自动导入缺省的权限类名。

##HQL子句中别名的引用：
1. 为被查询的类指定别名
2. 在HQL语句其他部分通过别名引用该类
3. 别名命名习惯，参考Java变量的命名习惯<br>
为类指定别名：from Seller as s/from Seller s，要保留代码的可读性

##选择-select子句
1. 以Object[]形式返回选择的属性
2. 以List形式返回选择的属性
3. 以map形式返回选择的属性
4. 以自定义类型返回选择的属性
5. 获取独特的结果-distinct关键字

##以自定义类型返回
1. 持久化类中定义对应的构造器
2. select子句中调用定义的构造器