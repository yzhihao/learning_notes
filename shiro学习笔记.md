#shiro
##注解
[详见](https://my.oschina.net/xinxingegeya/blog/211730)

##权限
* 单个权限 query
* 单个资源多个权限 user:query user:add 多值 user:query,add<br>
* 单个资源所有权限 user:query,add,update,delete user:<br>
* 所有资源某个权限 *:view<br><br>
* 实例级别的权限控制<br>
* 单个实例的单个权限 printer:query:lp7200 printer:print:epsoncolor
* 所有实例的单个权限 printer:print:*
* 所有实例的所有权限 printer:*:*
* 单个实例的所有权限 printer:*:lp7200
* 单个实例的多个权限 printer:query,print:lp7200

##注解
### RequiresGuest注解
RequiresGuest注解要求当前Subject是一个“访客”，也就是，在访问或调用被注解的类/实例/方法时，他们没有被认证或者在被前一个Session记住。

* 可以是在注册或登录的url上使用。

##RequiresRoles 注解
注解要求当前Subject在执行被注解的方法时具备所有的角色，否则将抛出AuthorizationException异常。

##shiro加密
* MD5+salt是比较安全的一种。详见example
##LifecycleBeanPostProcessor的作用
LifecycleBeanPostProcessor用于在实现了Initializable接口的Shiro bean初始化时调用Initializable接口回调，在实现了Destroyable接口的Shiro bean销毁时调用 Destroyable接口回调。如UserRealm就实现了Initializable，而DefaultSecurityManager实现了Destroyable。具体可以查看它们的继承关系。
##配置aop的AuthorizationAttributeSourceAdvisor
AuthorizationAttributeSourceAdvisor匹配所有类，匹配所有加了认证注解的方法