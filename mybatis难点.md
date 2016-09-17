## mybatis难点
## 容易混淆的概念：
1. resultMap和resultType：
都是表示查询结果集的类型，
resultMap需要手动配置映射关系，
而resultType是直接指定java类型，查询结果集的列名必须和实体属性名称一致
2. parameterMap和patameterType：
表示传入参数的对应关系，前者不推荐使用，只是mybatis为了适应以前的版本
3. #{}和${}：
都是用来作为占位符的，
`#{}在预编译的时候会呗替换为？`
而${}在预编译的时候直接将变量的值替换进去，而且没有引号，
故一般都是用前者，个别情况会使用后者：如在 需进行排序olderby，且排序字段为参数时可以使用${}
4. #{}和ognl：
在#{}中如果是基本类型，其中的名称可以随便写，但一般都用_parameter，因为值唯一
而ognl中必须写成_parameter的方式


![](http://img.mukewang.com/56fc98c400016e0812800720.jpg)

[延迟加载](http://www.cnblogs.com/selene/p/4631244.html#_label1)