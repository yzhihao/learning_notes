#Sping学习笔记1
[1000行代码读懂spring](http://my.oschina.net/flashsword/blog/192551)<br>
[自己写spring](http://blog.csdn.net/it_man/article/details/4402245#comments)
##spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器框架
1. 轻量：大小、开销轻量
2. 通过IoC达到松耦合
3. 提供了面向切面编程的丰富支持，允许通过分离应用的业务逻辑与系统级服务进行内聚性的开发
4. 包含并管理应用对象的配置和生命周期，是一种容器
5. 将简单的组件配置、组合成为复杂的应用，是框架

##spring——bean的配置及作用域#
###bean的常用配置项：（理论上只有class是必须的）
	id：唯一标识
	class：具体是哪一个类
	scope：范围
	constructor arguments：构造器的参数
	properties：属性
	Autowiring mode：自动装配模式
	lazy-initialization mode：懒加载模式
	initialization/destruction method：初始化/销毁的方法

###bean的作用域
	singleton 单例 bean容器只有唯一的对象（默认模式）
	prototype 每次请求会创建新的实例，destory方式不生效
	request 对于request创建新的实例，只在当前request内有效
	session 对于session创建新的实例，只在当前session内有效
	global session 基于portlet（例如单点登录的范围）的web中有效，如果在web中同session
##spring——bean的生命周期
生命周期：定义，初始化，使用，销毁
###初始化：
1. 方法1.实现org.springframework.beans.foctory.InitializingBean接口，覆盖afterPropertiesSet方法。系统会自动查找afterPropertiesSet方法，执行其中的初始化操作
2. 方法2.配置init-method（这是一定要有方法的实现）
例如设置bean中init-method="init"那么在初始化过程中就会调用相应class指定类的init()方法进行初始化工作
## bean配置的继承
[可借鉴--多个bean的属性相同](http://www.cnblogs.com/rollenholt/archive/2012/12/27/2835130.html)<br>
[可借鉴--重复应用某个bean](http://blog.csdn.net/zhangzeyuaaa/article/details/22583681)
###销毁（与初始化类似）
1. 方法1.实现org.springframework.beans.foctory.DisposableBean接口，覆盖destory方法。
2. 方法2.配置destory-method（这是一定要有方法的实现）

###配置全局初始化、销毁方法（属于默认配置，参考截图）
**注意：**
当三种方式同时使用时，全局（默认的）初始化销毁方法会被覆盖。
即使没有以上三种初始化方法也是可以编译执行的.

另外实现接口的初始化/销毁方式会先于配置文件中的初始化/销毁方式执行。<br>

![](http://img.mukewang.com/5784b3810001a11912800720.jpg)

##Aware
1. Spring中提供了一些以Aware结尾的接口，实现了Aware接口的bean在被初始化之后可以获取相应资源
2. 通过Aware接口，可以对Spring相应资源进行操作（一定要慎重）
3. 对为Spring进行简单的扩展提供了方便的入口

##Bean 的自动装配（Autowiring）

	default-autowire="no/byName/byType/constructor"
	
	no：不做任何操作
	
	byName：根据属性 名 自动装配，设值注入
	<bean id="xxx" class="xxx" ></bean>
	
	byType：根据属性 类型 自动装配，相同类型多个会抛出异常，设值注入
	<bean class="xxx" ></bean>
	
	constructor：与 byType 方式类似，不同之处是构造注入
	<bean class="xxx" ></bean>

###ResourceLoader：资源加载器，所有的applicationContext都实现该接口。
Resources资源接口。
如果资源路径不指定前缀，则依赖于applicationContext实例的创建方式。如视频所述，当前的applicationContext实例是通过测试类创建的，也是classpath方式，故资源路径也是根据classpath来查找资源。
示例
	(1)先实现ApplicationContextAware接口取得ApplicationContext实例；
	(2)通过ApplicationContext实例的getResourse()方法获得资源。

##·AOP基本概念及其特点
###什么是AOP
* AOP：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。
* 主要的功能是：日志记录，性能统计，安全控制，事务处理，异常处理等等。

##对切面的理解
* 程序中的每一个模块或者说功能，任何一个模块中都要记录它的日志、事务、安全验证等等，给我们带来的工作量非常大。
* 当程序到达某种规模时，尤其是格式调整之类的，这种改动量是非常大的。如果通过切面方式，对开发人员是不可见的，默认地会对每一个子模块记录日志等这些工作。通过预编译或者动态代理的方式来执行这个功能，
对开发人员是透明，他不需要知道。
* 切面是和功能垂直的，也就是切面是横切与各个功能之上的

##AOP实现方式
1. 预编译-AspectJ
2. 运行期动态代理（JDK动态代理、CGLib动态代理）-SpringAOP、JbossAOP

##AOP几个相关概念
1. 切面---一个关注点的模块化，这个关注点可能会横切多个对象
2. 连接点--程序执行过程中的某个特定的点
3. 通知----在切面的某个特定的连接点上执行的动作advice
4. 切入点--匹配连接点的断言，在AOP中通知和一个切入点表达式关联pointcut
5. 引入----在不修改类代码的前提下，为类添加新的方法和属性
6. 目标对象-被一个或者多个切面所通知的对象
7. AOP代理--AOP框架创建的对象，用来实现切面契约（包括通知方法执行等功能）
8. 织入---把切面连接到其他的应用程序类型或者对象上，并且创建一个被通知的对象，分为：编译时织入，执行时织入
![](http://img.mukewang.com/5787311f0001b30412800720.jpg)
##Spring的AOP实现
* 纯java实现，无需特殊的编译过程·，不需要控制类加载器层次
* 目前只支持方法执行连接点（通知Spring Bean的方法执行）
* 不是为了提供最完整的AOP实现（尽管它非常强大）；而是侧重于提供一种AOP实现和Spring IOC容器之间的整合，用于帮助解决企业应用中的常见问题
* Spring AOP不会与AspectJ竞争，从而提供综合全面的AOP解决方案。

##有接口和无接口的Spring AOP实现区别
* Spring AOP默认使用标准的JAVASE动态代理作为AOP代理，使得任何接口（或者接口集）都可以被代理
* Spring AOP中也可以使用CGLIB代理（如果一个业务对象并没有实现一个接口）
##配置AOP
Spring所有的切面和通知器都必须放在一个<aop:config>内（可以配置包含多个<aop:config>元素），每一个<aop:config>可以包含pointcut，advisor和aspect元素<br>
（它们必须按照这个顺序进行声明）

##通知

##pointcut
![](http://img.mukewang.com/5742a3180001aba412800720.jpg)
解释一下(* com.evan.crm.service.*.*(..))中几个通配符的含义：

第一个 * —— 通配 任意返回值类型<br>
第二个 * —— 通配 包com.evan.crm.service下的任意class<br>
第三个 * —— 通配 包com.evan.crm.service下的任意class的任意方法<br>
第四个 .. —— 通配 方法可以有0个或多个参数<br>

##切片
源码spring-aop-schema-advice

##introductions 引入
###对于declare-parents的作用：
在这里，declare-parents 为types-matching中的类（用proxy表示）指定了一个父类，然后又在指定了此父类为接口interface，并指出此父类的一个默认实现类impl。
###这个运用的是：
属于代理模式中的静态代理。作用就是通过proxy代理了impl。实现并可以加强imple中的功能！假如说impl中只有一个方法a（），那么proxy就可以代理a（）并对a添加附加功能/设定访问权限等等
所有基于配置文件的aspect，都只支持单例模式singlation model