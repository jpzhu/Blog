# 用vim给文本文件加个密
在vim的冒号模式下输入:X,会给文件设置一个密码。注意:是大写的X  
:X  
这是vim会提示你输入密码

>Enter encryption key:  
>Enter same key again:

退出vim前，再执行一下  
:w  
确保密码存下来了。

下次再用vim打开的时候，会提示你输入密码。用其它工具是打不开的:)

这个功能可以安全的用一个文件来保存你各种各样的密码.