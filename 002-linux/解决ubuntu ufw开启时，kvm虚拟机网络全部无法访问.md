> 原文地址 [blog.csdn.net](https://blog.csdn.net/tanqing24520/article/details/51557985)

博客备份一下解决方案，只要在`/etc/sysctl.conf`添加以下三行内容就行了：

```
net.bridge.bridge-nf-call-ip6tables = 0
net.bridge.bridge-nf-call-iptables = 0
net.bridge.bridge-nf-call-arptables = 0
```

然后`sudo sysctl -p`生效即可。

参考方案: http://my.oschina.net/abcfy2/blog/539420