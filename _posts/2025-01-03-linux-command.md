---
layout: post
title: 故障排查之 Linux 基础命令
categories: Linux
tags: ["故障排查"]
---

在 Linux 系统运维中，掌握常用的故障排查命令是非常重要的。本文将介绍一些最基础但很实用的故障排查命令，帮助你快速定位和解决系统问题。

### uptime
用于快速查看系统运行时间和负载

![uptime](/images/uptime.png)

系统运行了2天14小时18分钟，当前只有一个用户登录，系统负载平均值：最近1分钟、5分钟、15分钟的平均负载。
对于单核CPU来说：
- 负载值 < 1：系统很空闲
- 负载值 = 1：系统刚好满负荷
- 负载值 > 1：系统超负荷
对于多核CPU，用负载值除以CPU核心数来判断

### free
内存使用情况

`-h` 使用人类可读的格式

![free](/images/free.png)

- `total` 总内存大小
- `used` 已使用内存
- `free` 空闲内存
- `shared` 共享内存
- `buff/cache` 缓存和缓冲区使用的内存
- `available` 可用内存（包括可回收的缓存）

### df
Disk Free. 查看文件系统的磁盘空间使用情况

`-h` 使用人类可读的格式（KB、MB、GB）

![df](/images/df.png)

- `Filesystem` 文件系统设备名称
- `Size` 总容量
- `Used` 已使用容量
- `Avail` 可用容量
- `Use%` 使用百分比
- `Mounted on` 挂载点

### du
Disk Usage. 统计文件/目录的磁盘使用空间

重要参数说明
- `-h` 使用人类可读格式
- `-s` 只显示总计大小
- `-c` 显示所有文件总计大小
- `-a` 显示所有文件的大小，不只是目录
- `--max-depth=N` 显示目录层级，N 为层级数
- `--exclude=PATTERN` 排除匹配模式的文件
- `--time` 显示最后修改时间

常用选项
```bash
# 查看当前目录下所有文件和目录大小
du -h
# 只查看总大小
du -sh
# 查看当前目录下一级目录的大小
du -h --max-depth=1
# 按大小排序
du -h | sort -hr
# 查看特定类型文件占用空间
du -ch *.log
# 排除某些目录
du -h --exclude="*.log"
# 查看指定深度的目录大小
du -h --max-depth=2 /var/log/
```

### ps

![ps-ef](/images/ps-ef.png)

![psaux](/images/psaux.png)

重要参数说明
- `a` 显示所有用户的进程
- `u` 显示进程的用户和其他详细信息
- `x` 显示没有控制终端的进程
- `-e` 显示所有进程
- `-f` 显示完整格式的进程信息
- `-p pid` 指定进程ID

输出字段说明：
- `USER` 进程所有者
- `PID` 进程ID
- `%CPU` CPU使用率
- `%MEM` 内存使用率
- `VSZ` 虚拟内存大小（KB）
- `RSS` 实际内存使用（KB）
- `TTY` 终端名称
- `STAT` 进程状态
  - `R` 运行中
  - `S` 可中断睡眠
  - `D` 不可中断睡眠
  - `Z` 僵尸进程
  - `T` 停止或被追踪
- `START` 进程启动时间
- `TIME` 累计 CPU 时间
- `COMMAND` 执行的命令

常用选项
```bash
# 查看所有进程（进程资源使用情况）
ps aux --sort=-%cpu 
ps aux --sort=-%mem
# 查看所有进程（进程父子关系）PPID 是父进程ID
ps -ef
# 显示某个进程的子进程
ps --ppid <pid>
```

### netstat
用于显示网络连接、路由表和网络接口信息

参数说明
- `-a` 显示所有连接
- `-n` 不解析名字（显示数字）
- `-p` 显示进程信息
- `-l` 只显示监听中的连接
- `-t` TCP连接
- `-u` UDP连接
- `-r` 显示路由表
- `-s` 显示协议统计信息
- `-i` 显示网络接口信息

连接状态说明
- `LISTEN` 监听中
- `ESTABLISHED` 已建立连接
- `TIME_WAIT` 等待结束
- `CLOSE_WAIT` 等待关闭
- `SYN_SENT` 发送连接请求
- `SYN_RECV` 收到连接请求
- `FIN_WAIT1` 等待对方关闭
- `FIN_WAIT2` 从对方收到关闭确认
- `CLOSING` 双方同时尝试关闭
- `LAST_ACK` 等待最后确认

常用选项
```bash
# 查看所有监听端口
netstat -lntp
# 查看占用指定端口的进程
netstat -lnp | grep :80
# 查看指定进程的网络连接
netstat -anp | grep nginx
```

### vmstat
Virtual Memory Statistics，用于展示系统的虚拟内存、进程、CPU等信息

语法：`vmstat [间隔时间] [采样次数]`

![vmstat](/images/vmstat.png)

- procs（进程）：
   - `r`: 等待运行的进程数（运行队列）
   - `b`: 处于不可中断睡眠状态的进程数

- memory（内存，单位KB）：
   - `swpd`: 使用的虚拟内存大小
   - `free`: 空闲的物理内存大小
   - `buff`: 用作缓冲的内存大小
   - `cache`: 用作缓存的内存大小

- swap（交换分区，单位KB/s）：
   - `si`: 从交换分区读入内存的数据量
   - `so`: 从内存写入交换分区的数据量

- io（块设备IO，单位块/s）：
   - `bi`: 从块设备读取的块数
   - `bo`: 写入块设备的块数

- system（系统）：
   - `in`: 每秒中断数，包括时钟中断
   - `cs`: 每秒上下文切换次数

- cpu（CPU使用率百分比）：
   - `us`: 用户空间占用 CPU 时间百分比
   - `sy`: 内核空间占用 CPU 时间百分比
   - `id`: CPU空闲时间百分比
   - `wa`: 等待IO的 CPU 时间百分比
   - `st`: 虚拟机被迫等待虚拟 CPU 的时间百分比

常用选项
```bash
# 每秒采样一次
vmstat 1
# 每秒采样一次，共采样5次
vmstat 1 5
# 显示详细内存统计
vmstat -s
# 显示磁盘统计信息
vmstat -d
```

### top
实时显示系统中各个进程的资源占用情况

![top command](/images/top-command.png)

- 顶部各行信息概览
   - 第一行：当前系统时间、系统运行时间、当前登录用户数、平均负载值
   - 第二行（进程信息）：系统总进程数、正在运行的进程数、睡眠中的进程数、停止的进程数、僵尸进程数
   - 第三行（CPU信息）：
     - `us` 用户空间占用 CPU 时间百分比
     - `sy` 内核空间占用 CPU 时间百分比
     - `ni` 改变过优先级的用户进程占用 CPU 时间百分比
     - `id` 空闲 CPU 时间百分比
     - `wa` 等待输入输出 CPU 时间百分比
     - `hi` 硬件中断占用 CPU 时间百分比
     - `si` 软件中断占用 CPU 时间百分比
     - `st` 虚拟机偷取 CPU 时间百分比
   - 第四行（内存信息,默认单位是 MiB）：物理内存总量、空闲内存量、使用中的内存量、缓冲区/缓存的内存量
   - 第五行（交互空间信息）：交换区总量、空闲交换区两、使用的交换区量、可用于进程的内存量
- 列含义
   - `PID` 进程ID
   - `USER` 进程所有者用户名
   - `PR` 进程优先级
   - `NI` nice值（负值表示高优先级，正值表示低优先级）
   - `VIRT` 进程使用的虚拟内存总量
   - `RES` 进程使用的物理内存大小
   - `SHR` 共享内存大小
   - `S` 进程状态（R=运行，S=睡眠，D=不可中断睡眠，Z=僵尸）
   - `%CPU` CPU使用率
   - `%MEM` 内存使用率
   - `TIME+` 进程使用的CPU时间总计
   - `COMMAND` 进程名称/命令行
- 常用命令交互
   - `h` 显示帮助
   - `q` 退出
   - `空格` 立刻刷新显示
   - `c` 完整命令行
   - `t` 切换进程和 CPU 状态信息
   - `m` 切换内存显示
   - `e` 切换进程内存显示单位
   - `P` 按 CPU 使用率排序（默认）
   - `M` 按内存使用率排序
   - `E` 切换内存显示单位（KiB、MiB、GiB、TiB、PiB、EiB）
