查看JVM使用的默认的垃圾收集器
java -XX:+PrintCommandLineFlags -version

JVM内存概览
jmap -heap 1

JVM栈信息
jstack -l 1

JVM GC信息
jstat -gc 1

JVM导出内存文件
jmap -dump:format=b,file=/heap.hprof 1
