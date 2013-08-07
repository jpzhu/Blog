# 在Android开发Qt应用
将Qt移植到Android平台的项目名称叫necessitas，可以在sourceforge上找到，项目网址是necessitas.sf.net

目前的项目状态是对ARM支持的比较好，在ARM平台上开发，只需下载necessitas的SDK，然后利用SDK中的QtCreator向导即可生成for Android平台的Qt应用.apk。

####下载necessitas SDK
可以到
http://sourceforge.net/projects/necessitas/files/去下载。  
因为SDK是在线安装的，所以需要到http://files.kde.org/necessitas/sdk_master/ 上去下载软件包和数据，在公司的网络条件会比较费时。我已经下载了一份，放在内部服务器上。所以你也可以使用内部服务器，需要改动一下files.kde.org的域名指向. 只需在/etc/hosts里加入下面一行  
128.224.176.14 files.kde.org

####安装Qt应用到Android
安装Qt应用到Android手机上和安装普通Android程序是一样的。

####运行Qt应用
此时Qt程序还不能运行，因为你的Android系统里还没有安装Qt库。
你还需要安装一个叫Ministro service的一个安装包，它的作用是可以检查你Qt应用的库依赖，可以自动从网上下载更新Qt库。该软件的下载地址在
http://sourceforge.net/projects/ministro.necessitas.p/files/
选择Ministro II.apk下载即可。

好了，现在你可以运行你的Qt for Android程序了。

####编译自己的SDK ？
Necessitas是一个开源项目，你可以将之部署到不同的Android平台上。 详细的编译流程可以在wiki上看到。
http://sourceforge.net/p/necessitas/wiki/Home/

Qt for Android的基本工作原理是，对硬件资源的请求从传统的向X11请求，转而向Android请求。而中间的所有运行，包括图形的渲染等还是Qt的C++实现。不存在性能上的损失。
