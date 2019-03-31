排查 Java 程序占 CPU 问题，其实就分两个步骤：
1. 用 Linux 的 [top](http://man.linuxde.net/top) 命令找到对应线程的 pid；
2. 用 JDK 的小工具排查对应线程的 thread dump、heap dump；