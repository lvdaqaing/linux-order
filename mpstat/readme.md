## 简介
```
mpstat是Multiprocessor Statistics的缩写，是实时系统监控工具。
其报告与CPU的一些统计信息，这些信息存放在/proc/stat文件中。
在多CPUs系统里，其不但能查看所有CPU的平均状况信息，
而且能够查看特定CPU的信息。mpstat最大的特点是：
可以查看多核心cpu中每个计算核心的统计数据；
而类似工具vmstat只能查看系统整体cpu情况。
```
![enter description here](./1.png)
```
字段的含义如下

%user      在internal时间段里，用户态的CPU时间(%)，不包含nice值为负进程  (usr/total)*100
%nice      在internal时间段里，nice值为负进程的CPU时间(%)   (nice/total)*100
%sys       在internal时间段里，内核时间(%)       (system/total)*100
%iowait    在internal时间段里，硬盘IO等待时间(%) (iowait/total)*100
%irq       在internal时间段里，硬中断时间(%)     (irq/total)*100
%soft      在internal时间段里，软中断时间(%)     (softirq/total)*100
%steal ：显示虚拟机管理器在服务另一个虚拟处理器时虚拟CPU处在非自愿等待下花费时间的百分比
%guest ：显示运行虚拟处理器时CPU花费时间的百分比
%idle      在internal时间段里，CPU除去等待磁盘IO操作外的因为任何原因而空闲的时间闲置时间(%) (idle/total)*100
```

## 语法
```
mpstat [-P {|ALL}] [internal [count]]
参数 解释
-P {|ALL} 表示监控哪个CPU， cpu在[0,cpu个数-1]中取值
internal 相邻的两次采样的间隔时间、
count 采样的次数，count只能和delay一起使用

当没有参数时，mpstat则显示系统启动以后所有信息的平均值。有interval时，
第一行的信息自系统启动以来的平均信息。从第二行开始，输出为前一个interval时间段的平均信息。
```

## 实例
### 查看多核CPU核心的当前运行状况信息， 每2秒更新一次
![enter description here](./2.png)

### 如果要看每个cpu核心的详细当前运行状况信息，输出如下：
![enter description here](./3.png)

## 计算公式如下
```
total_cur=user+system+nice+idle+iowait+irq+softirq
total_pre=pre_user+ pre_system+ pre_nice+ pre_idle+ pre_iowait+ pre_irq+ pre_softirq
user=user_cur – user_pre
total=total_cur-total_pre
其中_cur 表示当前值，_pre表示interval时间前的值。上表中的所有值可取到两位小数点。   
```