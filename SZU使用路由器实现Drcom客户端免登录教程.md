# SZU使用路由器实现Drcom客户端免登录教程

### 0 所需软件

##### 1 [Wireshark](https://www.wireshark.org/download.html)  —— 用来抓包

##### 2 dogcom

​	下载地址在[[2018-10-11\]【DrCOM/dr.com校园网路由器】恩山无线论坛)](https://www.right.com.cn/forum/thread-215978-1-1.html)里面的5楼，我下载的是第二个，下载完把文件名改为dogcom备用

![image-20210922162640965](image/SZU%E4%BD%BF%E7%94%A8%E8%B7%AF%E7%94%B1%E5%99%A8%E5%AE%9E%E7%8E%B0Drcom%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%85%8D%E7%99%BB%E5%BD%95%E6%95%99%E7%A8%8B.assets/image-20210922162640965.png)

##### 3 [WinSCP](https://winscp.net/eng/download.php)

##### 4 putty



### 1 抓包

1）电脑上安装drcom客户端和Wireshark

2）电脑用网线直接连接学校网口

3）打开Drcom客户端，先不要登录，并记住客户端的版本号（我这个是5.2.0（D））

<img src="image/SZU%E4%BD%BF%E7%94%A8%E8%B7%AF%E7%94%B1%E5%99%A8%E5%AE%9E%E7%8E%B0Drcom%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%85%8D%E7%99%BB%E5%BD%95%E6%95%99%E7%A8%8B.assets/image-20210922163312276.png" alt="image-20210922163312276" style="zoom: 80%;" />

4）打开Wireshark，并选择“以太网，然后点击左上角的“开始”，接点登录Drcom客户端，等待30s左右（如果抓包失败可以延长一下抓包时间）后，点击Wireshark左上角的红色按钮，并点击“文件-另存为dr.pcapng”

5）打开在线配置生成器https://drcoms.github.io/drcom-generic/，选择对应版本，然后点击open选择刚才生成的dr.pcapng文件，点击右边的SAVE将生成的配置文件保存为drcom.conf

6）打开drcom.conf文件，修改password = ''（添加你的密码）和ror_version = False（改为True）

```
server = '172.30.XX.XX'
username = '你的学号'
password = '这里输入你的密码'  ——修改这里
CONTROLCHECKSTATUS = '\x20'
ADAPTERNUM = '\x05'
host_ip = '172.30.98.52'
IPDOG = '\x01'
host_name = 'fuyumi'
PRIMARY_DNS = '202.96.134.133'
dhcp_server = '172.30.254.50'
AUTH_VERSION = '\x30\x00'
mac = 0x9069a5d3021e
host_os = 'Windows 10' 
KEEP_ALIVE_VERSION = '\xdc\x02'
ror_version = True  —— 修改为True
```

### 2 配置路由器

1）把电脑上的drcom客户端关掉，路由器WAN接学校网口，路由器LAN接电脑

2）登录WinSCP，按下图那样输入，点击登录连接路由器

![image-20210922164917462](image/SZU%E4%BD%BF%E7%94%A8%E8%B7%AF%E7%94%B1%E5%99%A8%E5%AE%9E%E7%8E%B0Drcom%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%85%8D%E7%99%BB%E5%BD%95%E6%95%99%E7%A8%8B.assets/image-20210922164917462.png)

3）进入到/usr文件夹，将前面准备的dogcom和drcom.conf放到该文件夹下，并右键dogcom，修改其属性为0777，确定

![image-20210922165104278](image/SZU%E4%BD%BF%E7%94%A8%E8%B7%AF%E7%94%B1%E5%99%A8%E5%AE%9E%E7%8E%B0Drcom%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%85%8D%E7%99%BB%E5%BD%95%E6%95%99%E7%A8%8B.assets/image-20210922165104278.png)

4）使用putty登录路由器，输入下列指令，测试网络是否连通

```
/usr/dogcom -m dhcp -c /usr/drcom.conf -v
```

5）确定可正常上网后，添加开机启动项

```
/usr/dogcom -m dhcp -c /usr/drcom.conf -d -e
```

​	将上述代码添加到exit 0之前

![image-20210922165454460](image/SZU%E4%BD%BF%E7%94%A8%E8%B7%AF%E7%94%B1%E5%99%A8%E5%AE%9E%E7%8E%B0Drcom%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%85%8D%E7%99%BB%E5%BD%95%E6%95%99%E7%A8%8B.assets/image-20210922165454460.png)

6）重启路由器，教程结束



### 3 参考

1 [[2018-10-11\]【DrCOM/dr.com校园网路由器】使用教程 c语言单文件 dogcom 不需python - 斐讯无线路由器以及其它斐迅网络设备 - 恩山无线论坛 - Powered by Discuz! (right.com.cn)](https://www.right.com.cn/forum/thread-215978-1-1.html)

2 [深圳大学使用路由器登陆校园网，openwrt登陆drcom，d版教程 - osc_2wznp7fr的个人空间 - OSCHINA - 中文开源技术交流社区](https://my.oschina.net/u/4256309/blog/4645346)

