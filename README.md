# hive
日常使用hive中遇到的一些问题的解决
### hive执行到中间存储空间不足或者卡住怎么办？
1. 把占空间小的表放到前面join，因先join大表会产生一个很大的虚拟表，导致后续需要的存储空间越来越大，最后因为无法存下虚拟表导致出错失败。
2. 申请更多的内存来运行，利用
set mapreduce.reduce.memory.mb=36000;
set mapreduce.reduce.java.opts=-Xmx36000m;
set mapreduce.map.memory.mb=36000;
set HADOOP_CLIENT_OPTS=Xmx36000m;
这些语句申请更多的运行内存
3. 针对卡在某个进度上一般是由于数据倾斜比较大，利用设置
set hive.optimize.skewjoin=true;
set hive.skewjoin.key=100000;
第一个设置为true表示数据有倾斜，需要处理，第二个设置当有多少key相同时，需要把它拆解成两个job来处理
4. 在select语句前加explain可以知道每个Stage在取用什么表，进行什么操作，利用这个可以优化HQL语句，也可以知道是在处理哪个表的时候速度比较慢。
