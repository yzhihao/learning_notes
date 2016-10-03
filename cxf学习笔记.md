#WebService
####1：什么是 WebService?
*Web service*是一个平台独立的，低耦合的，自包含的、基于可编程的web的应用程序，可使用开放的XML（标准通用标记语言下的一个子集）标准来描述、发布、发现、协调和配置这些应用程序，用于开发分布式的互操作的应用程序。
Web Service技术， 能使得运行在不同机器上的不同应用无须借助附加的、专门的第三方软件或硬件， 就可相互交换数据或集成。
#####Web service 就是一个应用程序，它向外界暴露出一个能够通过Web进行调用的API
 
####2: WebService 的实现方式
        1）基于jdk实现的WebService
            ->无需引入其他jar包就可实现
        2) 基于cxf实现的WebService
            ->需要引入相关的jar包
 
一些常用的英文缩写：
#####SOAP
简单对象访问协议(Simple Object Access Protocol) 它是用于交换XML（标准通用标记语言下的一个子集）编码信息的轻量级协议。
#####WSDL
Web Service描述语言WSDL　就是用机器能阅读的方式提供的一个正式描述文档而基于XML（标准通用标记语言下的一个子集）的语言，用于描述Web Service及其函数、参数和返回值。因为是基于XML的，所以WSDL既是机器可阅读的，又是人可阅读的。
 
#####你会怎样向别人介绍你的Web service有什么功能，以及每个函数调用时的参数呢？你可能会自己写一套文档，你甚至可能会口头上告诉需要使用你的Web service的人。这些非正式的方法至少都有一个严重的问题：当程序员坐到电脑前，想要使用你的Web service的时候，他们的工具(如Visual Studio)无法给他们提供任何帮助，因为这些工具根本就不了解你的Web service。
解决方法是：用机器能阅读的方式提供一个正式的描述文档。Web service描述语言(WSDL)就是这样一个基于XML（标准通用标记语言下的一个子集）的语言，用于描述Web service及其函数、参数和返回值。WSDL既是机器可阅读的，又是人可阅读的，这将是一个很大的好处。一些最新的开发工具既能根据你的Web service生成WSDL文档，又能导入WSDL文档，生成调用相应Web service的代码。
 
 
 
 
 
 
------
 
文档的格式
###
    <definitions> 相当于java的 package   
    <type></type>
    <message></message>
    <portType>
        <operation></operation> 用来指定SEI中的方法  N个operation子元素，每个operation代表一个ws操作（即方法），包含请求与回复2次
        <input></input>    指定客户端应用传过来的数据，会引用上面定义的<message>
        <output></output>指定服务器端返回给客户端的数据，会引用上面定义的<message>
    </portType> 用来定义服务器端的SEI
 
 
    <binding> 是用于定义SEI的实现类  指定ws的函数风格，并详细的定义了接口中的操作即 operation 。函数风格，现在一般为 document 文档风格。
    <binding>
        <type></type>    引用上面的<portType>
        <operation></operation> 用来定义实现的方法
            <soap:operation style="document"/>     传输的是document（XML）
            input :指定客户端引用传过来的数据
                <soap:body use="literal"/>使用文本类型
            output: 指定服务器端返回给客户端的数据
 
 
    </binding>
    <service>
        <name></name> 它用于指定客户端的容器类
        <port></port>用来指定一个服务器端处理请求的入口（SEI的实现）
    </service>  一个webService的容器
</definitions>
 

 
 
4、本质：一个ws，其实并不是方法的调用，而是发送soap消息（即xml文档片段）
    1）客户端把调用的方法参数，转换生成xml文档片段（soap消息）??必须符合wsdl规定的格式
 
    2）客户端通过网络，把xml文档片段传给服务器。
 
    3）服务器接收到xml文档片段。
 
    4）服务器解析xml文档片段，提取其中的数据。返回xml文档。
 
如getCatsByUser(User user) 方法，传入传出的是这样的xml片段：
    <!--传入的参数-->
    <getCatsByUser>
        <arg0>
            <address></address>
            <id></id>
            <name></name>
            <pass></pass>
        </arg0>
    </getCatsByUser>
 
    <!--返回的参数-->
    <getCatsByUserResponse>
        <return>
            <color></color>
            <id></id>
            <name></name>
        </return>
        <return>
            <color></color>
            <id></id>
            <name></name>
        </return>
    </getCatsByUserResponse>
 
 
 
**soap的语法**
 
    1、根元素：Envelope。
 
    2、Header元素：:不是强制出现，由程序员控制，主要用于携带一些额外的信息，比如用户名、密码
 
    3、Body:
 
    1）调用正确，body元素内容应该遵守WSDL要求的格式。
        <soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
            <soap:Body>
                <ns2:sayHiResponse xmlns:ns2="http://ws.com/">
                    <return>SAM，您好现在的时间是：Thu Jan 09 10:19:57 CST 2014</return>
                </ns2:sayHiResponse>
            </soap:Body>
        </soap:Envelope>
    2）调用错误，body元素内容显示Faulty元素。
        <soap:Fault>
        <faultcode>soap:Server</faultcode>
        <faultstring>
            No binding operation info while invoking unknown method with params unknown.
        </faultstring>
        </soap:Fault>
        </soap:Body>
 
    4、return：返回信息。
 
 
 
 
 
（一）通过使用CXF的日志拦截器，我们可以在控制台输出拦截器的soap消息。服务端拦截如下：
 
1、sayHi()操作的soap消息：
 
 
    传入：
    <?xml version="1.0" ?>
        <S:Envelope xmlns:S="http://schemas.xmlsoap.org/soap/envelope/">
            <S:Body>
                <ns2:sayHi xmlns:ns2="http://ws.com/">
                    <arg0>SAM-SHO</arg0>
                </ns2:sayHi>
            </S:Body>
        </S:Envelope>
 
    传出：
    <soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
            <soap:Body>
                <ns2:sayHiResponse xmlns:ns2="http://ws.com/">
                    <return>SAM-SHO，您好！您现在访问的是简单的WS服务端，时间为：14-11-20 下午1:28</return>
                </ns2:sayHiResponse>
            </soap:Body>
    </soap:Envelope>
 
 
2、getCatsByUser()操作的soap消息：与上面wsdl分析的消息应该是一致的。
 
    传入：
 
    <?xml version="1.0" ?>
    <S:Envelope xmlns:S="http://schemas.xmlsoap.org/soap/envelope/">
        <S:Body>
            <ns2:getCatsByUser xmlns:ns2="http://ws.com/">
                <arg0>
                    <address>soochow</address>
                    <id>1</id>
                    <name>Sam-Sho</name>
                    <pass>1234</pass>
                </arg0>
            </ns2:getCatsByUser>
        </S:Body>
    </S:Envelope>
 
    传出：
    <soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
        <soap:Body>
            <ns2:getCatsByUserResponse xmlns:ns2="http://ws.com/">
                <return>
                    <color>黄色</color>
                    <id>1</id>
                    <name>加菲猫</name>
                </return>
                <return>
                    <color>蓝色</color>
                    <id>2</id>
                    <name>蓝胖子</name>
                </return>
                <return>
                    <color>粉色</color>
                    <id>3</id>
                    <name>hello kitty</name>
                </return>
                <return>
                    <color>黑白色</color>
                    <id>4</id>
                    <name>熊猫</name>
                </return>
            </ns2:getCatsByUserResponse>
        </soap:Body>
    </soap:Envelope>

##客户端和服务端
* 在生成客户端的时候要添加相应的jar包，并在cmd中输入类似下面的命令
`F:\myworkspace\Cxf_Client\src>       wsdl2java -frontend jaxws21 http://localhost:9999/onyasWS?wsdl`

 