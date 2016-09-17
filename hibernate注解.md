#hibernate注解
##@Entity注解：<br>
@Entity：映射实体类<br>
@Entity（name="tableName"）<br>
name:可选，对应数据库中的一个表。若表名与实体类名相同，则可以省略
注意：使用@Entity时必须指定实体类的主键属性。

##@Table<br>
1.@Table(name="",catalog="",schema="")<br>
2.必须与@Entity配合使用，只能标注在实体的class定义处，表示实体对应的数据库表的信息。<br>
3.name:可选，与@Entity一样为数据库表名<br>
4.catalog:可选，目录名，默认为空<br>
5.schema:可选，模式名，默认为空

##@Embeddable
1.@Embeddable表示一个非Entity类可以嵌入到另一个Entity类中作为属性而存在<br>
2.@embeddable不生成独立的表，可以理解@Embeddable注解类为属性集，在表中也是属性。
##属性注解
![](http://img.mukewang.com/56e167e20001682812800720.jpg)
**放在属性或get方法上**

##@id
![](http://img.mukewang.com/577f64dc00019b3c12800720.jpg)
>将string类型的属性设置成主键是一定要指定该属性的长度，可以用Column注解来指定，不然mysql会默认让其长度为255，而mysql主键的长度不允许太长<br>
**序列化，serializable接口（声明式接口，不需要实现任何方法）**
## 属性级别注解@GeneratedValue:
@GeneratedValue(strategy=GenerationType,generator="")
该属性可选，用于定义主键生成策略。
![](http://img.mukewang.com/5782f45d000124ab12800720.jpg)

##若主键是字符串
>主键是字符串，主键生成策略就为手工赋值，不能用JPA提供的主键生成策略，要用Hibernate提供的主键生成器，其中GenericGenerator(name)与generaotr中的值对应，assigned就是手工赋值，参考配置文件的是一样的。
>![](http://img.mukewang.com/56e57814000164c412800720.jpg)


##属性级别注解@Column
@Column-可将属性映射到列，使用该注解来覆盖默认值，@Column描述了数据库表中该字段的详细定义，这对于根据JPA注解生成数据库表结构的工具非常有作用。
![](http://img.mukewang.com/5782f807000131f512800720.jpg)

##属性级别注解：@Embedded
@Embedded是注释属性的，表示该属性的类是嵌入类。<br>
注意：同时嵌入类也必须标准@Embeddedable注解。
就是在嵌入属性上添加和在要嵌入的类的上面添加。

##属性级别注解：@EmbeddedId
@EmbeddedId使用嵌入式主键类（写@Embedded）实现复合主键。<br>
注意：嵌入式主键类必须实现Serializable接口、必须有默认的public无参数的构造方法、必须覆盖equals和hashCode方法。
这里

##属性级别注解@Transient
可选，表示该属性并非一个到数据库表的字段的映射，ORM框架将忽略该属性，如果一个属性并非数据库表的字段映射，就务必将其标示为@Transient，否则ORM框架默认其注解为@Basic(就是在表的字段)

##一对一的外键
![](http://img.mukewang.com/56e170ee0001c20f12800720.jpg)

表示关系由对方维护，自己将不再维护，就算在自己这端设置值，保存到数据库后外键依然是 null

a） 只有OneToOne,OneToMany,ManyToMany上才有mappedBy属性，ManyToOne不存在该属性；
b） mappedBy标签一定是定义在the owned side（被拥有方的），他指向theowning side（拥有方）；
c） 关系的拥有方负责关系的维护，在拥有方建立外键。所以用到@JoinColumn
d）mappedBy跟JoinColumn/JoinTable总是处于互斥的一方