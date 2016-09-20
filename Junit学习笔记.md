1. 测试方法上必须使用@Test进行修饰
 
2. 测试方法必须使用public void 进行修饰，不能带任何的参数
 
3. 新建一个源代码目录，专门存放Test测试类

4. 测试类的包应该和被测试类保持一致

5. 测试单元中的每个方法必须可以独立测试，测试方法间不能有任何的依赖

6. 测试类使用Test作为类名的后缀
 
7. 测试方法使用test作为方法名的前缀

8. Failure 一般由单元测试使用的方法判断失败所引起的，这表示测试点发现了问题，就是说问题输出的结果和我们预期的不一样。
 
9. error是有代码异常引起的，它可以产生于测试代码本身的错误，也可以是测试代码中一个隐藏的bug
 
10. 测试用力不是用来证明你是对的，而是用来证明你没有错。

//在 该类 （不是所有类）加载以前执行，所以必须是静态方法才能执行
this is @BeforeClass

//before和after会在所有test前后执行
	
	this is @Before
	this is @Test1
	this is @After
	
	this is @Before
	this is @Test2
	this is @After
	 
//通常测试对资源的管理，例如数据库的连接和关闭
this is @AfterClass

@Test (timeout=毫秒) 测试代码运行效率，也能避免死循环
@Ignore 所修饰的测试方法会被测试运行器忽略

@RunWith: 可以修改测试运行器 org.junit.runner.Runner

JUnit测试套件：


参数化设置

参数测试类开头要有RunWith(Parameterized.class)

要有一个构造方法放参数

要有一个参数数组方法