#Sping学习笔记2
##advisor
* advisor就像一个小的自包含的方面，只有一个advice
* 切面自身通过一个bean表示，并且必须实现某个advice接口，同时，advisor也可以很好的利用AspectJ的切入表达式
* Spring通过配置文件中<aop:advisor>元素支持advisor实际使用中，大多数情况下它会和transactional advice配合使用
* 为了定义一个advisor的优先级以便让advice可以有序，可以使用order属性来定义advisor的顺序

advisors的使用场景：
通常会用在一个环绕通知中，
我们可以统计方法的调用次数或者调用频率，或是某种情况下需要对调用次数进行控制。

比如这里我们可以在函数尝试调用4次的时候抛出一个异常实现

#Spring事务
##什么是事务?<br>
事务指逻辑上的一组操作,这组操作要么全部成功,要么全部失败.<br>
事物的特性:<br>
1. 原子性Atomic:事务是一个不可分割的工作单位,事务中的操作要么都发生,要么都不发生.<br>
2. 一致性Consistent:事务处理前后数据的完整性必须保持一致.<br>
3. 隔离性Isolated:多个用户并发访问数据库时,一个用户的事务不能被其他用户的事务所干扰,多个并发事务之间数据要相互隔离.<br>
4. 持久性Durable:一个事务一旦被提交,它对数据库中数据的改变就是永久性的,即使数据库发生故障也不应该对其有任何影响

###Spring高层事务管理抽象主要有3个接口：

	platform TransactionManager事务管理
	TransactionDefinition事务定义信息（隔离、传播、超时、只读）
	TransactionStatus事务具体运行状态

##TransactionManager的选择
![](http://img.mukewang.com/57554bc70001b45e12800720.jpg)

第一类共同点：如果 A 方法中有事务，则调用 B 方法时就用该事务，即：A和B方法在同一个事务中。

	PROPAGATION_REQUIRED：如果 A 方法中没有事务，则调用 B 方法时就创建一个新的事务，即：A和B方法在同一个事务中。
	PROPAGATION_SUPPORTS：如果 A 方法中没有事务，则调用 B 方法时就不使用该事务。
	PROPAGATION_MANDATORY：如果 A 方法中没有事务，则调用 B 方法时就抛出异常。

第二类共同点：A方法和B方法没有在同一个事务里面。

	PROPAGATION_REQUIRES_NEW：如果 A 方法中有事务，则挂起并新建一个事务给 B 方法。
	PROPAGATION_NOT_SUPPORTED：如果 A 方法中有事务，则挂起。
	PROPAGATION_NEVER：如果 A 方法中有事务，则报异常。

第三类：如果 A 方法有的事务执行完，设置一个保存点，如果 B 方法中事务执行失败，可以滚回保存点或初始状态。

    重点的三种:PROPAGATION_REQUIRED, PROPAGATION_REQUIRES_NEW, PROPAGATION_NESTED