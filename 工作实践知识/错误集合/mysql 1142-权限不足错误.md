报错信息
![[截图.png]]
问题分析：
用户权限不足，需要分配其一定的权限。
解决办法

（1）找到`my.ini`并打开，在`mysqld`下面加上一行`skip-grant-tables`(部分操作会受到限制)（方法二：打开cmd命令提示符，进入mysql.exe所在的文件夹。输入命令 `mysqld --skip-grant-tables` 回车，此时就跳过了mysql的用户验证。）

![[截图 (1).png]]

（2）重启mysql服务

（3）修改权限

①直接输入mysql，不需要带任何登录参数直接回车就可以登陆上数据库。
![[截图 (2).png]]
②输入show databases; 可以看到所有数据库说明成功登陆。
![[截图 (3).png]]

③其中mysql库就是保存用户名的地方。输入 use mysql; 选择mysql数据库。show tables; 查看所有表，会发现有个user表，这里存放的就是用户名，密码，权限等等账户信息。
![[截图 (4).png]]
④输入
`select * from user where user="root" and host="localhost"`
来查看用户信息。在显示的列表中显示：root用户的localhost的权限都是’N’,表示root用户本地登陆不具有权限

⑤输入以下命令，修改权限
``` mysql
update user set Select_priv = 'Y',Insert_priv = 'Y',Update_priv = 'Y',Delete_priv = 'Y',Create_priv = 'Y',Drop_priv = 'Y', Reload_priv = 'Y',Shutdown_priv = 'Y',Process_priv = 'Y',File_priv = 'Y', Grant_priv = 'Y', References_priv = 'Y',Index_priv = 'Y',Alter_priv = 'Y', Super_priv = 'Y',Show_db_priv = 'Y',Create_tmp_table_priv = 'Y',Lock_tables_priv = 'Y',Execute_priv = 'Y',Repl_slave_priv = 'Y', Repl_client_priv = 'Y',Create_view_priv = 'Y', Show_view_priv = 'Y', Create_routine_priv = 'Y',Alter_routine_priv = 'Y',Create_user_priv = 'Y', Event_priv = 'Y',Trigger_priv = 'Y',Create_tablespace_priv = 'Y' where user='root' and host='localhost';
```


结果如下
![[截图 (5).png]]
OK，问题解决！（记得将在my.ini加的一行代码去掉，然后重启mysql才算完成！）

版权声明：本文为CSDN博主「慕斯-ing」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。

原文链接：[https://blog.csdn.net/blbyu/article/details/118642175](https://blog.csdn.net/blbyu/article/details/118642175)

#mysql  #错误集 
