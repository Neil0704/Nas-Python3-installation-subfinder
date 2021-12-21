# Nas-Python3-installation-subfinder
Synology movie subfinder

首先感谢原作者的文章，我研究了一下排了一些坑，更新了一下ptyhon和pip的安装地址和代码，作者在.sh代码中有一段错误命令，删除'zumuzu'字段后自动化文件才可以正确运行，否则会报错中断。

原地址如下：
https://www.jianshu.com/p/035fdf1bff4b?ivk_sa=1024320u

由于官方提供的 python3 没有自带 pip 这个库管理工具，而用 pip 安装 subfinder 又是最方便的，所以需要单独安装 pip。这一步稍微麻烦一些，需要用到 ssh 连接 NAS。首先在系统配置中打开 ssh 连接.

![image](https://user-images.githubusercontent.com/92854687/146916625-44686ee2-5f8b-4714-a66b-4ea430a0169a.png)

打开 ssh

接下来我们用 ssh 连接 NAS，windows 用户建议下载 PuTTY来进行连接，Mac 用户直接用终端就行,具体方式是terminal下输入: ssh -p 2233 用户名@Nas IP

![image](https://user-images.githubusercontent.com/92854687/146917160-00b8908c-c463-4b91-9bcc-424c574404a3.png)

连上后会让我们输入账号和密码。搞定以后，输入

sudo -i

输入密码后，获得 root 权限

![image](https://user-images.githubusercontent.com/92854687/146917234-e1fccee6-3870-41cc-ad9f-1b46063a530b.png)

接下来就是照着命令行敲就可以。先安装 setuptools：

wget https://pypi.python.org/packages/source/s/setuptools/setuptools-59.7.0.tar.gz#md5=030bf6b66b72d23bef44084e625e05bd

tar -zxvf setuptools-59.7.0.tar.gz

cd setuptools-59.7.0

python3 setup.py build

python3 setup.py install

接下来安装 pip：

wget   https://pypi.python.org/packages/source/p/pip/pip-21.3.1.tar.gz#md5=0d3f27f4b7fecb33fd573e4f46cc6788

tar -zxvf pip-21.3.1.tar.gz

cd pip-21.3.1

python3 setup.py build

python3 setup.py install

安装 subfinder 并配置计划任务：

python3 -m pip install subfinder

提示安装成功后，运行一下看能否成功

subfinder /volume4/MOVIES -m shooter zimuku

subfinder 之后的路径换成自己的视频的绝对路径。绝对路径可以在 File Station 中查看：

![image](https://user-images.githubusercontent.com/92854687/146917905-ee76ee84-0e0d-4350-aebe-bb948dc760aa.png)

成功运行的话会看到程序跑起来了：

![image](https://user-images.githubusercontent.com/92854687/146917962-4612b01d-3529-466f-b0af-5a27aebc17fa.png)

创建 sh 脚本便于计划任务管理:

可以使用VS code 创建 .sh格式的文件，代码如下：

#!/bin/bash


SUBFINDER_EXEC='/bin/subfinder'


VIDEO_PATH='/volume1/video/'


. /etc/profile


${SUBFINDER_EXEC} ${VIDEO_PATH} -m shooter zimuku >> /volume1/automatic/subfinder.log 2>&1

这里我创建的文件名及其位置为 volumes1/homes/subfinder.sh，其中路径名字大家都可以自由修改，只要自己能找得到就行。
/volume1/automatic/subfinder.log 2>&1 这一句是创建一个运行的log文件，路径可以自己替换'/volume1/automatic/'这一段字符，会出现在你指定的位置。

在群晖中配置计划任务
![image](https://user-images.githubusercontent.com/92854687/146918527-41f00f38-7660-49d8-83c9-94d0e6fb8741.png)


名字随便取

![image](https://user-images.githubusercontent.com/92854687/146918580-2fa18041-becc-4ad8-a937-864e50638474.png)

请根据自己的需求配置。

![image](https://user-images.githubusercontent.com/92854687/146918670-ef8905cb-36a9-43b8-ab34-e245101236a5.png)

此处注意替换自己脚本的路径和名字，记得要 bash 开头

![image](https://user-images.githubusercontent.com/92854687/146918621-93b2fdab-063f-401f-a5d7-1cd3f9ca77f2.png)


至此便全部配置完成，可以选择你的自动计划手动运行一次，大家可以到自己指定的路径去查看log日志。

路径名中最好不要出现中文和空格，如果一定想用空格，请用'_' 下划线 代替。linux 对中文处理相当不友好，经常莫明出错。
布置完成之后记得关闭SSH!!

