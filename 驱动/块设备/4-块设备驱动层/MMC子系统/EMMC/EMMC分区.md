参考：  
https://blog.csdn.net/wxh0000mm/article/details/77864002  
https://www.cnblogs.com/chyl411/p/6739352.html  
https://wenku.baidu.com/view/d50196926bec0975f465e24e.html  

https://www.cnblogs.com/smartjourneys/articles/6653867.html

### 1、ＥＭＭＣ分区对应文件系统信息　　
```
/proc/mounts：查看所有挂载分区的信息

/sys/class/block/xxx：查看某一个分区的详细信息，比如只读设置（ro），读写情况（stat）
```

### 2、厂家EMMC原始分区  
> BOOT Area Partition 1（不能修改）  

> BOOT Area Partition 2（不能修改）  

```
1、嵌入式系统可以直接从boot分区启动
2、大小为128kb的整数倍
```



> RPMB（不能修改，Replay Protected Memory Block -- 回放保护存储块）  

```
1、大小为128KB的正数倍
2、对其中数据的访问需要签名认证
```



> General Purpose Partition 1～4  

```
1、在芯片出场时不存在，只有主动配置才会存在
2、最多有4个分区
3、是从UDA中分配的
```



> User Data Area  （用户数据区）

```
安卓系统会对此区域分区：boot、system、vendor等
```



> Vender private area  



> 用户不可见分区  


### 3、安卓设备（高通）EMMC分区  