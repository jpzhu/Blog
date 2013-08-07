#凌乱的重定向命令~
gcc -E -inlucde xxx.h – < /dev/null >& /dev/null  
这条命令，乍一看晕头转向不知道，要干啥

分开几个部分来看  

>< /dev/null 是指重定向输入部分，防止命令需要输入的时候，停止在那里不继续执行。  
>\>& /dev/null 则相当与 >/dev/null 2>&1,是一种简化形式

根据 $ info “(bash)Redirections” ,以下3种形式等价

    3.6.4 Redirecting Standard Output and Standard Error
    —————————————————-
    This construct allows both the standard output (file descriptor 1) and
    the standard error output (file descriptor 2) to be redirected to the
    file whose name is the expansion of WORD.
    
    There are two formats for redirecting standard output and standard
    error:
    &>WORD
    and
    >&WORD
    Of the two forms, the first is preferred. This is semantically
    equivalent to
    >WORD 2>&1

参考:  
http://stackoverflow.com/questions/8208033/what-does-dev-null-dev-null-at-the-end-of-a-command-do
