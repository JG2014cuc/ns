# DNS域传送漏洞及DNS信息收集实例讲解

### 域名是zonetransfer.me和两个名称服务器nsztm1.digi.ninja和nsztm2.digi.ninja



### 实验名词解释


     SOA record:

### 概要

域名系统 (DNS) 区域中的任何文件中的第一个资源记录应起始授权机构 (SOA) 资源记录。SOA 资源记录表明该 DNS 名称服务器是此 DNS 域内的数据信息的最佳来源。

### SOA记录

SOA 资源记录包含以下信息︰

1.源主机-在其中创建文件的主机。

2.电子邮件-负责管理域的区域文件的人的电子邮件地址，请与联系。请注意，"."而不是使用"@"中的电子邮件名称。



3.序列号 – 此区域文件的修订号。每次更改区域文件时，增加这个数字。很重要，以便所做的更改将分发给任何辅 DNS 服务器进行更改时，每次增加这个值。



4.刷新时间-的时间，以秒为单位，查询要检查更改的主 DNS 服务器的 SOA 记录之前等待的辅助 DNS 服务器。当刷新时间到期时，辅助 DNS 服务器将请求从主当前 SOA 记录的副本。主 DNS 服务器符合此请求。

5.辅助 DNS 服务器比较主 DNS 服务器的当前 SOA 记录的序列号和其自己的 SOA 记录中的序列号。如果它们不同，则辅助 DNS 服务器将从主 DNS 服务器请求区域传送。默认值为 3600。



6.重试时间-的时间，以秒为单位，辅助服务器等待重试失败的区域传送。通常情况下，重试时间小于刷新时间。默认值为 600。



7.过期时间的时间，以秒为单位，不断尝试完成区域传送的辅助服务器。如果此时间到期之前成功的区域传送，辅助服务器将过期它的区域文件。这意味着次镜像将停止应答查询，因为它认为它太旧，无法进行可靠的数据。默认值为 86400。

8.最小 TTL-最小生存时间值适用于区域文件中的所有资源记录中。通知其他服务器多长时间他们应在缓存中保留数据的查询响应中提供此值。默认值为 3600。

下面是一个 Microsoft DNS 服务器生成默认 SOA 资源记录的示例︰

@   IN  SOA     nameserver.place.dom.  postmaster.place.dom. (

                               1            ; serial number

                               3600         ; refresh   [1h]

                               600          ; retry     [10m]

                               86400        ; expire    [1d]

                               3600 )       ; min TTL   [1h]



括号将允许换行到多行的 SOA 记录。

在上面的示例︰

源主机 = nameserver.place.dom。

联系人的电子邮件 = postmaster.place.dom。  

来源：https://support.microsoft.com/zh-cn/kb/163971

}


{

    MX records：mail exchanger record 邮件交换记录。用于将以该域名为结尾的电子邮件指向对应的邮件服务器以进行处理

      来源：https://help.dreamhost.com/hc/en-us/articles/215032408-What-is-an-MX-record-
### MX记录
1.MX记录是由两部分组成：优先权和域名。

2.前面的数字为优先权

3.越低的数字意味着一个更高的优先级。

4.ASPMX.L.GOOGLE.COM是它连接的邮件服务器

5.发送邮件服务器连接到MX服务器的优先顺序由优先级决定

6.如果两个服务器有相同的优先级，它随机选择一个。（这实际上是负载平衡了连接。）

    PTR record：PTR记录是邮件服务器通常需要。每一个邮件服务器有自己的DNS记录，它指向一个IP地址，但并不是所有的IP地址反向PTR记录。
如果你没有PTR记录，可能是您的邮件将被拒绝从其他邮件服务器。通常你不能自己创建PTR记录，因为你不是一个拥有IP地址，只是域名。
但是可以问服务提供商创建PTR记录

最好的办法就是做一个反向的DNS PTR记录相同的DNS主机邮件服务器记录。请向下滚动页面看到一些例子如何检查DNS PTR记录手动。

你可以使用此页检查PTR记录。您的邮件服务器将被自动检测到。这个工具将不检查单个IP地址的PTR记录。要做到这一，请使用此网页。 来源：http://www.ptrrecord.net/

### SRV记录
   SRV记录是一个服务记录，它是用来显示协议识别服务、主机和端口上运行它。这是经常使用的VoIP设置显示SIP服务器的位置，但可用于任何类型的服务。

SRV记录可以显示一些什么服务目标的公司正在运行的非常有用的信息

SRV 记录：一般是为Microsoft的活动目录设置时的应用。DNS可以独立于活动目录，但是活动目录必须有DNS的帮助才能工作。

为了活动目录能够正常的工作，DNS服务器必须支持服务定位（SRV）资源记录，资源记录把服务名字映射为提供服务的服务器名字。活动目录客户和域控制器使用SRV资源记录决定域控制器的IP地址。

_sip._tcp.zonetransfer.me -  服务的名称，包括协议和TCP / UDP的名字，在这里它是运行在TCP协议

}



{

   TXT记录的文本信息和应该检查的有价值的信息。第一个泄露了一个与系统管理有关的人的电话号码和电子邮件地址。

第二个显示该网站已被验证在谷歌应用程序帐户中使用。

三号是一个方式，GoDaddy以检查申请一个SSL证书拥有域名，这种信息可能是有用的如果它泄漏服务正在使用或联系信息

}



{

SRV 记录：一般是为Microsoft的活动目录设置时的应用。DNS可以独立于活动目录，但是活动目录必须有DNS的帮助才能工作。

为了活动目录能够正常的工作，DNS服务器必须支持服务定位（SRV）资源记录，资源记录把服务名字映射为提供服务的服务器名字。活动目录客户和域控制器使用SRV资源记录决定域控制器的IP地址。

_sip._tcp.zonetransfer.me -  服务的名称，包括协议和TCP / UDP的名字，在这里它是运行在TCP协议



## 实验步骤：



在linux中进行实验操作，输入dig axfr @nsztm1.digi.ninja zonetransfer.me

输入命令得到soa记录：dig nsztml.digi.nninja zonetransfer.me SOA +noall +answer （截图在ppt里面）

此条目显示有关主名称服务器的信息、管理员的联系方式和其他关键信息，这是它如何分解的信息：


     nsztm1.digi.ninja.  源主机

robin.digi.ninja. 电子邮件-负责管理域的区域文件的人的电子邮件地址，请与联系。请注意，"."而不是使用"@"中的电子邮件名称。

    2014101601   
此区域文件的修订号。每次更改区域文件时，增加这个数字。增加这个值是每一次的变化是重要的，这样的变化将分发到任何辅助DNS服务器。

    172800  
二级名称服务器应在请求更改请求之间等待的时间

    900 
重试时间 如果它未能正确刷新，则主名称服务器应等待重试时间（秒）

    1209600  
过期时间 Expire time

    3600 
最小生存时间。

负责人领域提供了一个电子邮件地址，可作为其他攻击的一部分，
并从目前的序列号，如果它是基于日期和定期检查，它的改变可能表明该公司的一些活动。



    zonetransfer.me.        7200    IN      NS      nsztm1.digi.ninja.

    zonetransfer.me.        7200    IN      NS      nsztm2.digi.ninja.





MX记录：
       dig nsztml.digi.nninja zonetransfer.me MX +noall +answer

       zonetransfer.me.	7200	IN	MX	0 ASPMX.L.GOOGLE.COM.

       zonetransfer.me.	7200	IN	MX	10 ALT1.ASPMX.L.GOOGLE.COM.

       zonetransfer.me.	7200	IN	MX	10 ALT2.ASPMX.L.GOOGLE.COM.

       zonetransfer.me.	7200	IN	MX	20 ASPMX2.GOOGLEMAIL.COM.

       zonetransfer.me.	7200	IN	MX	20 ASPMX3.GOOGLEMAIL.COM.

       zonetransfer.me.	7200	IN	MX	20 ASPMX4.GOOGLEMAIL.COM.

       zonetransfer.me.	7200	IN	MX	20 ASPMX5.GOOGLEMAIL.COM.

MX记录表明在邮件要发送，这些都是标准的邮件服务器，谷歌表示该公司使用Gmail和谷歌应用程序来处理他们的电子邮件。



     dr.zonetransfer.me.	300	IN	LOC	53 20 56.557 N 1 38 33.525 W 0.00m 1m 10000m 10m

LOC是短暂的位置，可以用来记录一个纬度/经度值。值被存储在度/分钟/秒，



     onetransfer.me.	301	IN	TXT	"Remember to call or email Pippa on +44 123 4567890 or pippa@zonetransfer.me when making DNS changes"

     zonetransfer.me.	301	IN	TXT	"google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA"

     dzc.zonetransfer.me.	7200	IN	TXT	"AbCdEfG"

     info.zonetransfer.me.	7200	IN	TXT	"ZoneTransfer.me service provided by Robin Wood - robin@digininja.org. See www.digininja.org/blog/zonetransferme.php for more information

TXT记录的文本信息和应该检查的有价值的信息。第一个泄露了一个与系统管理有关的人的电话号码和电子邮件地址。

第二个显示该网站已被验证在谷歌应用程序帐户中使用。

三，GoDaddy以检查申请一个SSL证书拥有域名，这种信息可能是有用的如果它泄漏服务正在使用或联系信息



     testing.zonetransfer.me. 301	IN	CNAME	www.zonetransfer.me.

     staging.zonetransfer.me. 7200	IN	CNAME	www.sydneyoperahouse.com.

公司有一个测试网站，它位于同一个服务器上的主要网站。

测试网站往往是不太安全的比主要网站，所以是一个更好的攻击向量。



     164.180.147.217.in-addr.arpa.zonetransfer.me. 7200 IN PTR www.zonetransfer.me.



     _sip._tcp.zonetransfer.me. 14000 IN	SRV	0 0 5060 www.zonetransfer.me.

SRV记录是一个服务记录，它是用来显示协议识别服务、主机和端口上运行它。这是经常使用的VoIP设置显示SIP服务器的位置，但可用于任何类型的服务。

SRV记录可以显示一些什么服务目标的公司正在运行的非常有用的信息

    _sip._tcp.zonetransfer.me - 
 服务的名称，包括协议和TCP / UDP的名字，在这里它是运行在TCP协议

14000 IN SRV   标准的TTL，DNS值，DNS和类型 

0 服务的优先级，如果有多个服务，这表明先尝试

0 重量，当两个服务有相同的优先级，这表明这是首选

5060 服务正在监听的端口

       www.zonetransfer.me. 
主机提供服务



     zonetransfer.me.	7200	IN	A	217.147.177.157

     www.zonetransfer.me.	7200	IN	A	217.147.177.157

     vpn.zonetransfer.me.	4000	IN	A	174.36.59.154

     owa.zonetransfer.me.	7200	IN	A	207.46.197.32

     office.zonetransfer.me.	7200	IN	A	4.23.39.254

     canberra_office.zonetransfer.me. 7200 IN A	202.14.81.230

    dc_office.zonetransfer.me. 7200	IN	A	143.228.181.132



个记录是DNS记录的主要类型，常常把大多数的记录，他们的名字到IP地址的映射。









# 课程大作业

## IANA与互联网？ 

### o域名管理机制

### oIP地址管理机制

### o协议管理机制

### o中文域名如何注册和解析？



##### IANA：(The Internet Assigned Numbers Authority，互联网数字分配机构)是负责协调一些使Internet正常运作的机构。同时，由于Internet已经成为一个全球范围的不受集权控制的全球网络，为了使网络在全球范围内协调，存在对互联网一些关键的部分达成技术共识的需要，而这就是IANA的任务。


#### 简介：

IANA是全球最早的Internet机构之一，其历史可以追溯到1970年。今天，IANA被负责协调IANA责任范围的非营利机构ICANN(Internet Corporation for Assigned Names and Numbers，互联网名称与数字地址分配机构)掌管。

IANA分配和维护在互联网技术标准（或者称为协议）中的唯一编码和数值系统。

IANA的所有任务可以大致分为三个类型：

#### 一、域名。IANA管理DNS域名根和.int，.arpa域名以及IDN（国际化域名）资源。

#### 二、数字资源。IANA协调全球IP和AS（自治系统）号并将它们提供给各区域Internet注册机构。 

#### 三、协议分配。IANA与各标准化组织一同管理协议编号系统。



#### 职能：

IANA是INTERNET域名系统的最高权威机构，掌握着INTERNET域名系统的设计、维护及地址资源分配等方面的绝对权力。

在IANA之下另有3个分支机构分别负责欧洲、亚太地区、美国与其他地区的IP地址资源分配与管理。这3个机构是： RIPE（即设在比利时的Réseaux IP Européens)，负责整个欧洲地区的IP地址资源分配与管理； 

APNIC（即设在澳大利亚的Asia Pacific Network Information Center），负责亚洲与太平洋地区的IP地址资源分配与管理；

ARIN（即设在美国的American Registry for Internet Numbers) ，负责美国与其他地区的IP地址资源分配与管理。另外，许多国家和地区都成立了自己的域名系统管理机构，负责从前述3个机构获取IP地址资源后在本国或本地区的分配与管理事务。

这些国家和地区的域名系统管理机构大多属于半官方或准官方机构。但在实际运作过程中，相关国家或地区的政府至少在业务上对其不加干预，使其成为前述3个机构之一在各该国家或地区的附属机构。如日本的JPNIC和中国的CNNIC均属此种机构。

IANA还可以查询全球各类顶级域名的具体信息，无论知名还是不知名的域名后缀，你都可以找到它的详细信息以及管理机构所在国家、地址信息、运营公司、注册局网址等。

数据库总网址

AfriNIC非洲地区

APNIC亚太地区

ARIN 北美地区

LACNIC 拉丁美洲和一些加勒比群岛

RIPE NCC 欧洲、中东和中亚

顶级域名信息数据库