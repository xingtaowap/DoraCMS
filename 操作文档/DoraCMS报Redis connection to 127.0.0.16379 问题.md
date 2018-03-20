DoraCMS报Redis connection to 127.0.0.1:6379 问题
##
生哥  • 原创教程   • 2 年前
近来有些同学在运行DoraCSM出现错误提示：

Error: Redis connection to 127.0.0.1:6379 failed - connect ECONNREFUSED
    at RedisClient.on_error (E:\myWeb\nodework\doracms1.0\node_modules\redis\index.js:196:24)
    at Socket.<anonymous> (E:\myWeb\nodework\doracms1.0\node_modules\redis\index.js:106:14)
    at Socket.emit (events.js:107:17)
    at net.js:459:14
    at process._tickCallback (node.js:355:11)
在此说明一下，DoraCMS 在V1.0.2版本中加入了redis缓存：



QQ截图20150917173608.jpg



**这里缓存了用户信息和站点地图，细心的同学可以在app.js 中找到配置：**
```

app.use(session({
    secret: settings.session_secret,
    store: new RedisStore({
        port: settings.redis_port,
        host: settings.redis_host,
        pass : settings.redis_psd,
        ttl: 1800 // 过期时间
    }),
    resave: true,
    saveUninitialized: true
}));
```
这里就设置了redis的相关信息（ip，端口号和密码），用过redis的同学一看应该就能明白，下面就介绍如何让DoraCMS不报这个错误。


##1、下载redis，根据您服务器的版本下载redis，您可以到官网上下载：http://redis.io/download



这里提供一个windows下的版本 

redis-2.4.5-win32-win64.zip



##2、解压后可以看到如下目录（我解压到了 D:\softbak\redis2.4.5\64bit 路径下）

QQ截图20150917174212.jpg



##3、执行 redis-server.exe，出现如下界面表示启动成功：

QQ截图20150917174534.jpg



##4、找到setting.js 文件（/doracms1.0/models/db/settings.js），配置redis相关参数：

redis_host: '127.0.0.1',
redis_port: 6379,
redis_psd : '',
redis_db: 0,


redis_port 默认为6379，不需要改，默认密码为空 （因为是直接启动的redis-server.exe，默认不需要密码）

redis_db 可以不用管



##5、在项目根目录下执行cmd，启动服务（npm start）,就不会报错了。



注意：本地调试redis可以不设置密码，放到服务器上必须设置密码，具体可以参考另一篇文章：

