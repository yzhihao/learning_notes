#java_socket学习笔记
##获取本机信息   
###获取本机的InetAddress实例
	InetAddress address = InetAddress.getLocalHost();
	System.out.println("计算名：" + address.getHostName());
	System.out.println("IP地址：" + address.getHostAddress());
	byte[] bytes = address.getAddress();// 获取字节数组形式的IP地址
	System.out.println("字节数组形式的IP：" + Arrays.toString(bytes));
	System.out.println(address);// 直接输出InetAddress对象

### 根据机器名获取InetAddress实例
	// InetAddress address2=InetAddress.getByName("laurenyang");
	
	// 根据ip地址获取InetAddress实例
	InetAddress address2 = InetAddress.getByName("1.1.1.10");
	System.out.println("计算名：" + address2.getHostName());
	System.out.println("IP地址：" + address2.getHostAddress());

##//创建一个URL实例
	URL imooc=new URL("http://www.imooc.com");
	//?后面表示参数，#后面表示锚点
	URL url=new URL(imooc, "/index.html?username=tom#test");
	//主要方法：
	//getProtocol()//协议;getHost()//主机;getPort()getPath()//;文件路径;getFile()//文件名;getRef()//相对路径;getQuery();//字符串
	
	//可以读取网页内容
	//通过URL的openStream方法获取URL对象所表示的资源的字节输入流
	InputStream is = url.openStream();
openstream方法是返回字节流，inputsteam，再通过inputsteamreader把字节流转换成字符流，再通过bufferreader加入字符缓冲区，再去读取就很方便，同时由于编码的限制所以要使用utf-8,这个是在字节转换成字符流的时候指明：inputsteamreader=new inputsteamreader((new inputsteam(file)),"utf-8")