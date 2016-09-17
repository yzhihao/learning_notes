#spring注解
scan可以自动扫描类和成员变量的注解<br>
config 可以自动扫描成员变量的注解<br>
scan的范围大于config的范围<br>
注册了 scan 就不用再考虑 config方法了<br>
![](http://img.mukewang.com/572034e80001193612800720.jpg)
###配置扫描
![](http://img.mukewang.com/5720357800015e2212800720.jpg)
[springmvc和spring的配置](http://www.imooc.com/article/6331)

----
![](http://img.mukewang.com/579a29a9000130c312800720.jpg)
*bean名默认是首字母小写的类名。*

---
>关于bean的作用域，用hashcode的方法来测试
![](http://img.mukewang.com/57830b05000191d812800720.jpg)

##
* 在用@Autowired时，可以设置required=false，这样容器在找不到这个bean时，也不会报错。
* 每个类中，只能有一个构造器被标记为requid=true。

###@Autowired注解标注的List和Map：
spring会将@Component注解标注的实现了BeanInterface的Bean的实例加载进去
@Order注解只对List有效，对Map无效

###@Autowired更新的用法
.可以通过添加注解给需要该类的数组的字段或方法，以提供ApplicationContext中的所有特定类型的bean
* .可以用于装配Key为String的Map
* .如果希望数组有序，可以让bean实现org.springframework.core.Ordered接口或使用的@Order注解.

###使用@Autowired注意的事项
.@Autowired是由Spring BeanPostProcessor处理的，所以不能在自己的BeanPostProcessor或BeanFactoryPostProcessor类型应用这些注解，这些类必须通过XML或者Spring的@Bean注解加载

###@Autowired
.可以使用@Autowired注解那些众所周知的解析依赖性接口，比如：BeanFactory,ApplicationContext,Environment,ResouceLoader,ApplicationEventPublisher,and MessageSouce

##
@qualifier注释可以指定具体注入的是哪一个对应实例（例如多个类实现同一个接口，而接口注入实例（向上转型时）时，可以指定注入的是哪一个继承的类实例）

##基于java的容器注解（001）
@Bean标识一个用于配置和初始化一个有SpringIOC容器管理的新对象的方法，类似于XML配置文件的<bean/>

可以在Spring的@Component注解的类中使用@Bean注解任何方法（仅仅是可以）

上一点中，通常使用的是@Configuration和@Bean配合使用

例子中左侧@Bean标识的方法是让IOC容器实例化MyService对象
跟右侧的<bean>标签的方式是一样效果

##引入资源文件和赋值的两种方式：
(1)注解方式：@ImportResource和@Value；<br>
(2)xml配置方式：<context:property-placeholder location="***"></context:property>和<proeprty>标签。
注：properties文件的username最好加前缀，以保证命名的唯一性，不会与系统冲突。


## @bean(name="类名") 默认是单例的；
@scope 指定bean的作用域 值有singleton(默认),prototype（只存活于当前注解范围，每次调用都重新创建）,session,request,global session
##
CustomAutowireConfigurer<br>
.CustomAutowireConfigurer是BeanFactoryPostProcessor的子类，通过它可以注册自己的qualifier注解类型（即使没有使用Spring的@Qualifier 注解）<br>
.该AutowireCandidateResolver决定自动装配的候选者：<br>
--每个bean定义autowired-candidate值<br>
--任何<bean/>中的default-autowire-candidates<br>
--@Qualifier注解及使用CustomAutowireConfigurer的自定义类型

##@autowired，@resources，@inject
**执行顺序：**
###@Autowired and @Inject

1. Matches by Type
2. Restricts by Qualifiers
3. Matches by Name
###@Resource

1. Matches by Name
2. Matches by Type
3. Restricts by Qualifiers (ignored if match is found by name)

[三者详解](http://my.oschina.net/swearyd7/blog/297681)