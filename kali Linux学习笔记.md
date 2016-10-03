#kali Linux
##Linux的常用命令：
* clear清空终端命令
* 终端停止命令ctrl+c
##kali中主要为2种卸载方法：
* 使用apt的方式有：
apt-get remove [package]
apt-get remove --purge # ------(package 删除包，包括删除配置文件等)
apt-get autoremove --purge # ----(package 删除包及其依赖的软件包+配置文件等

*　使用dpkg
dpkg -r     #移除一个已安装的包。
dpkg -P    #完全清除一个已安装的包。和 remove 不同的是，remove 只是删掉数据和可执行文件，purge 另外还删除所有的配制文件。
##w3af
是一个完整的Web 应用攻击和漏洞分析环境。该环境为Web 漏洞的分析和渗透测

#关于密码破解-密码破解基本策略
1. 识别加密类型
2. 对较短的密码直接实施暴力破解
3. 尝试常用密码
4. 组合常用密码/单词/拼音与数字
5. 混合暴力攻击
6. 如果还失败了。。Gpu，僵尸网络，集成电路，分布式
7. 不行就算了，一个密码而已

##sqlmap
[简单用法](http://blog.csdn.net/gsl68/article/details/9046301)

	sqlmap -u “注入地址” -v 1 –dbs   // 列举数据库（-v 1指的是等级）
	sqlmap -u “注入地址” -v 1 –current-db   // 当前数据库
	sqlmap -u “注入地址” -v 1 –users    // 列数据库用户
	sqlmap -u “注入地址” -v 1 –current-user  // 当前用户
	sqlmap -u “注入地址” -v 1 -D “数据库” --tables    // 列举数据库的表名
	sqlmap -u “注入地址” -v 1 -D “数据库”  -T “表名” --columns // 获取表的列名
	sqlmap -u “注入地址” -v 1 -D “数据库” -T “表名”  -C “字段,字段” --dump  // 获取表中的数据，包含列
	增加等级（–v 2）就可
###用法：
* 开始可以调节等级和风险
* 当看到有is  vulnerale的字样说明可以被入侵。
* 之后加上所要得到的数据库名之类的，因为sqlmap已经有缓存，所以不会第二次去测试可否入侵
##arpspoof 用法
* 首先是配置
`echo 1 >> /proc/sys/net/ipv4/ip_forward`
* 监听命令
`arpspoof -i eth0 -t iP1 iP2`
>在这里，IP1是要欺骗的ip（比如路由器网关），ip2是欺骗ip（比如攻击者的ip）
##查看图片
driftnet -i ech0