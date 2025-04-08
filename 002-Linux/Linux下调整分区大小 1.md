## Linux下调整分区大小

### 常用命令

- `df -h`：查看分区
- `lsblk`：查看分区结构和可分配空间

### 调整步骤（以home分区分配给root为例）

#### 1. 查看并备份
```bash
df -h
tar cvf /tmp/home.tar /home
```

#### 2. 卸载home
```bash
fuser -km /home/
umount /home
```

#### 3. 删除并扩展分区
```bash
lvremove /dev/mapper/centos-home
lvextend -L +20G /dev/mapper/centos-root
```

#### 4. 扩展文件系统
```bash
# CentOS 6
resize2fs /dev/mapper/centos-root
# CentOS 7
xfs_growfs /dev/mapper/centos-root
```

#### 5. 重建home分区
```bash
lvcreate -L 20G -n home centos
```

#### 6. 创建文件系统并挂载
```bash
# CentOS 6
mke2fs /dev/mapper/centos-home
# CentOS 7
mkfs.xfs /dev/mapper/centos-home

mount /dev/mapper/centos-home /home
```

#### 7. 恢复数据
```bash
tar xvf /tmp/home.tar -C /
cd /home/home/
mv * ../
```
