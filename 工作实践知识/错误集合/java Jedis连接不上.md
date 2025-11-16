### 出现错误：

出现以下错误：
```java
Failed to connect to any host resolved for DNS name.

Suppressed: java.net.SocketTimeoutException: connect timed out
```

### 错误原因：

1. 未打开`redis-server`

2. 防火墙未开`6379`

3. `redis.config`未修改

### 解决方法：

#### 1. 首先检查redis-server是否启动
`ps-ef | grep redis`

出现如下则启动了`redis-server`
![[clipboard (3).png]]
未出现则启动`redis-server`，若启动了仍然连接不上则看解决办法2，3

#### 2. 修改`redis.config`
![[clipboard (4).png]]

#### 3. 确认防火墙开启6379端口
```Linux
firewall-cmd --query-port=6379/tcp #查看端口6379是否开启 #如果返回yes则代表已开启 
firewall-cmd --zone=public --add-port=6379/tcp --permanent #开启6379端口 
firewall-cmd --reload #重载防火墙 
firewall-cmd --query-port=6379/tcp #再次查看端口6379是否开启 
firewall-cmd --list-ports #查看已经开启的端口，应该会返回3306/tcp 6379/tcp
```

#java #错误集 