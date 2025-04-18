### 全文摘抄警告
**原文链接**：[ubuntu 永久关闭swap](https://juejin.cn/post/7033632940265308173)

网上大部分都只说了注释 ***/etc/fstab*** 最后一行 ***swap.img*** 注释掉

实际上还需要修改该文件的
~~~ shell
/dev/disk/by-uuid/5fea4562-481e-4bdc-9373-0a55b3420cc0 none swap sw 0 0
~~~
改成
~~~ shell
/dev/disk/by-uuid/5fea4562-481e-4bdc-9373-0a55b3420cc0 none swap sw,noauto 0 0
~~~