# redis-rce

# 漏洞利用
##  Redis 4.x/5.x RCE
在GitHub找到了不需要msf的利用脚本
```
https://github.com/Ridter/redis-rce
```
需要下载exp.so
>exp.so 是一个恶意的 redis 模块，我们将在目标 redis 服务器上加载它。下载地址

```
https://github.com/n0b0dyCN/redis-rogue-server/blob/master/exp.so
```
不过我总是报错
![image.png](https://cdn.jsdelivr.net/gh/dustblessnotdust/imgpic@main/img/202502230222013.png)
我当时认为是exp.so模块版本不符合的原因，然后就自己从新编译了一下
```
https://github.com/n0b0dyCN/RedisModules-ExecuteCommand
```
报错了
![image.png](https://cdn.jsdelivr.net/gh/dustblessnotdust/imgpic@main/img/202502230224661.png)
搜索了一下，往`module.c`文件添加了一些文件头，可以编译成功了
```
#include <string.h>  // 修复 strlen 和 strcat 的问题
#include <arpa/inet.h>  // 修复 inet_addr 的问题
#include <unistd.h>  // 修复 execve 的问题
```
![image.png](https://cdn.jsdelivr.net/gh/dustblessnotdust/imgpic@main/img/202502230226012.png)
然后我将反弹shell的端口设为80，还是运行失败，改为6379才成功，之后就都不行了，所以我也不能判断是端口的问题还是攻击模块的问题
![image.png](https://cdn.jsdelivr.net/gh/dustblessnotdust/imgpic@main/img/202502230217111.png)

反弹shell成功
![image.png](https://cdn.jsdelivr.net/gh/dustblessnotdust/imgpic@main/img/202502230218100.png)
获取root权限
