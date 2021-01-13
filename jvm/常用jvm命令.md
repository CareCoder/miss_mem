查看JVM使用的默认的垃圾收集器
java -XX:+PrintCommandLineFlags -version

JVM内存概览
jmap -heap 1

JVM栈信息
jstack -l 1
