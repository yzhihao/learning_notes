# java 容器
[容器的理解](http://blog.csdn.net/chenssy/article/category/1688799)
## 选取
Array 改慢读快——用数组实现的
Linked 改快读慢--链表实现的
Hash 两者之间

   		1、对于需要快速插入、删除元素，则需使用LinkedList。
        2、对于需要快速访问元素，则需使用ArrayList。
        3、对于“单线程环境”或者“多线程环境，但是List仅被一个线程操作”，需要考虑使用非同步的类，如果是“多线程环境，切List可能同时被多个线程操作”，考虑使用同步的类（如Vector）。

当用了Iterator（锁定）时用remove时，不能用原来的容器的remove的方法。

##map的遍历
1. map.entrySet()得到Set<Map.Entry<K, V>>，其实就是一个set的形式，之后再用foreach遍历既可。

		for(Map.Entry<k,v> entry:map.EntrySet()){
			system.out.println("结果是:"+entry.getKey+entry.getValue())
		}

2. 用迭代器遍历：最灵活
		
		Iterator<Map.Entey<K,V>> it=map.entrySet().iterator();
		while(a.hasNext()){
			Map.Entey<K,V> a=it.next();
			system.out.println("结果是："+a.getValue()+a.getKey())
		}
3. 最简单的一种方式：(普遍使用)灵活性强

		for(String key: map.keySet()){
			system.out.println（"结果是："+key+map,get(key)）
		}
		
##hashcode和equal
1. 首先hashcode相等成立equal不一定成立，反之则成立。应为hashcode是根据属性来计算散列表的，即属性的值相等时（可以不同类的实例）即相等，但equal表示两个对象必须是同一个类的实例。
###为什么hashcode和equal一定要同时覆写：
* 因为hashMap、hasoTable或hashSet是通过先比较hashcode在比较equal进行查找等操作的。若不同时覆写两个方法的话，就会到时不能正常的使用上述容器类来储存对象。
##HashTable与HashMap的区别
1. HashTable基于Dictionary类，而HashMap是基于AbstractMap
2. HashMap可以允许存在一个为null的key和任意个为null的value，但是HashTable中的key和value都不允许为null。HashTable直接抛异常，hashmap不会。
3. Hashtable的方法是同步的，而HashMap的方法不是。
       
>map的k，v都是一个泛型对象。、

##ConcurrentHashMap和Hashtable的区别
相同点： Hashtable 和 ConcurrentHashMap都是线程安全的，可以在多线程环境中运行； key跟value都不能是null<br>
区别：
*  两者主要是性能上的差异，Hashtable的所有操作都会锁住整个对象，虽然能够保证线程安全，但是性能较差；
* ConcurrentHashMap内部使用Segment数组，每个Segment类似于Hashtable，在“写”线程或者部分特殊的“读”线程中锁住的是某个Segment对象，其它的线程能够并发执行其它的Segment对象

#一、Vector简介
* Vector可以实现可增长的对象数组。与数组一样，它包含可以使用整数索引进行访问的组件。不过，Vector的大小是可以增加或者减小的，以便适应创建Vector后进行添加或者删除操作。<br>
* Vector实现List接口，继承AbstractList类，所以我们可以将其看做队列，支持相关的添加、删除、修改、遍历等功能。<br>
* Vector实现RandmoAccess接口，即提供了随机访问功能，提供提供快速访问功能。在Vector我们可以直接访问元素。<br>
* Vector 实现了Cloneable接口，支持clone()方法，可以被克隆。<br>

在Java中Stack类表示后进先出（LIFO）的对象堆栈。栈是一种非常常见的数据结构，它采用典型的先进后出的操作方式完成的。每一个栈都包含一个栈顶，每次出栈是将栈顶的数据取出.
源码基于Vector（继承vector）
