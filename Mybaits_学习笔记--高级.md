# mybatis_学习笔记2
##接口式编程


#一对一，一对多，多对多的查询，
> [该博客有详解](http://www.cnblogs.com/selene/p/4627446.html?utm_source=tuicool&utm_medium=referral#_label1)

###使用原始的方法开发dao时。
> 在具体的实现类中，调用sqlSession.selectList()时需要传递两个参数，根据第一个参数在配置文件中找具体的sql语句，这里需要注意两个风险。


1. 编码中的namespace 与配置文件中的namespace 一致，
2. 编码中引用的id与配置文件中的id也要一致，手写就会有很大的风险
3. 就是第二个参数的问题，selectList方法中传入的参数是object，而配置文件中是一个pojo对象，那么在编写代码的时候，传递的参数是其他的类型，编译器也不会提示错误，只有程序运行起来的时候，才会出错、。
4.  还有返回值，编码中是通过泛型来约束list的，至于list中是什么泛型，编译器都是可以通过的，在配置文件中的返回类型使用resultMap来约束的。
###规避以上四个风险的手段，mybatis称之为接口式编程，需要遵循以下规范：
1. 	类的映射：使用接口类的全限定名(包括包名)作为配置文件的namespace，完成类与配置文件的对应关系。
2. 	方法名：接口方法名与配置文件中将要执行的sql语句的id一样，这样就完成了方法调用的映射。
3.  参数类型：接口方法的输入参数类型和配置文件中sql的parameterType的类型相同
4. 	返回值类型：接口方法的返回值类型和配置文件的resultMap的类型相同
###实例：
接口的调用：

	IMessage imessage = sqlSession.getMapper(IMessage.class);
	messageList = imessage.queryMessageList(message);
![](http://img.mukewang.com/5780d0b9000157b012800720.jpg)
![](http://img.mukewang.com/5722e2770001161f12800720.jpg "接口编写")
## 接口编程的原理
* 动态代理,接口没有实现类.Mybatis为接口提供实现类,即用Proxy.newProxyInstance()创建代理实例,返回类型为Object,利用泛型强制转换<br>
`IMessage imessage = sqlSession.getMapper(IMessage.class);`<br>
代理实例调用接口方法时,并不会执行,而是触发 `MapperProxy.invoke()`,其中包含`sqlSession.selectList(namespace.id,parameter)`
至于为什么会包含,因为接口方法与(加载Mybatis的)配置信息对应得上,即 接口名.方法=namespace.id
messageList = imessage.queryMessageList(message);

## 动态代理的源码
	IMessage imessage=sqlSession.getMapper(IMessage.class);//获取到的就是代理实例
	messageList =imessage.queryMessageList(parameter);//代理实例执行接口方法时，就会触发调用处理程序，也就是第三个参数对象的invoke（）方法，MapperProxy是实现了InvocationHandler接口的
	MapperProxyFactory.newInstance(MapperProxy<T> mapperProxy){
	--return (T) Proxy.newProxyInstance(mapperInterface.getClassLoader()//通过接口获取类加载器,new Class[]{mapperInterface}//代理类实现的接口数组,mapperProxy//调用代理实例的处理程序)
	--}
###解决了三个问题：
1. 为什么只定义了一个接口，没有具体实现的情况下，接口方法可以被调用，因为动态代理。
2. 为什么sqlSession.getMapper(.class)可以根据传入的参数，返回一个对应的类型，因为泛型。
3. Mybatis加载文件时，利用namespace加载了一个class,然后把这个class与代码中传入接口的class进行匹配，方法执行所需要的信息就是来自于已经匹配成功的配置文件中，当结果与配置文件对应上后，调用接口的方法执行sql语句。

用动态代理实现拦截器
![](http://img.mukewang.com/5723107b0001f5c412800720.jpg)


###mybatis的拦截器实现分页（动态代理）
拦截sql语句来实现分页:

1. 拦截什么样的对象（以page作为参数传入；page对象）
2. 拦截对象什么行为 
3. 什么时候拦截 （在prepareStatement的时候拦截）
（源码）
###具体过程
1. RoutingStatementHandler
2. 通过RoutingStatementHandler对象的属性delegate找到statement实现类BaseStatementHandler
3. 通过BaseStatementHandler类的反射得到对象的MappedStatement对象
4. 通过MappedStatement的属性getID得到配置文件sql语句的ID
5. 通过BaseStatementHandler属性的到原始sql语句
6. 拼接分页sql（
1. 需要查询总数的sql
2. 通过拦截Connection对象得到PrepareStatement对象	
3. 得到对应的参数
4. 把参数设到prepareStatement对象里的？（该？号在配置文件以#{}形式存在，mybatis会把它转为？号）
5. 执行改sql语句
6. 得到总数
7. 把属性值为新的sql

##批量新增
![](http://img.mukewang.com/552cda8a000170b112000530.jpg)

##批量删除，详见天河项目。