# mysql设置权限

>用root身份给其他用户设置权限

主要是`grant`命令

具体请参考手册




>grant all privileges on 库名.表名 to '用户名'@'IP地址' identified by '密码' with grant option;

>flush privileges;


>grant 权限1,权限2,…权限n on 数据库名称.表名称 to 用户名@用户地址 identified by ‘连接口令’;

```
库名:要远程访问的数据库名称,所有的数据库使用“*” 
表名:要远程访问的数据库下的表的名称，所有的表使用“*” 
用户名:要赋给远程访问权限的用户名称 
IP地址:可以远程访问的电脑的IP地址，所有的地址使用“%” 
密码:要赋给远程访问权限的用户对应使用的密码
```

最后：

mysql刷新权限命令：FLUSH PRIVILEGES;（一般用于数据库用户信息更新后）

还有一种方法，就是重启mysql服务器也可以
