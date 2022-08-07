## Clang编译的例子看动态链接过程
### 基本clang操作,main.c引用random.c的函数，若不动态链接，编译报错

生成so
clang -o random.o -c random.c
clang -shared -o librandom.so random.o

clang -o main.o -c main.c //-c表示只编译，不链接
clang -o main main.o

![image-20220807221827884](C:\Users\WYX\AppData\Roaming\Typora\typora-user-images\image-20220807221827884.png)

clang -o main main.o -lrandom

![image-20220807222634109](C:\Users\WYX\AppData\Roaming\Typora\typora-user-images\image-20220807222634109.png)
因为没有指定链接so的路径出错
clang -o main mian.o -lrandom -L.  //-L指定查找路径

![image-20220807223038054](C:\Users\WYX\AppData\Roaming\Typora\typora-user-images\image-20220807223038054.png)

![image-20220807224237779](C:\Users\WYX\AppData\Roaming\Typora\typora-user-images\image-20220807224237779.png)

链接器查找路径的顺序：

>> ELF的rpath中规定的路径。
>> LD_LIBRARY_PATH环境变量中的路径。
>> ELF的runpath中规定的路径。
>> /etc/ld.so.conf文件中列出的路径。该文件可包含子文件，因此也包括子文件中列出的路径。
>> 默认的系统路径/lib和/usr/lib。

因此，编译命令指定rpath,clang -o main main.o -lrandom -L. -Wl,-rpath,.

![image-20220807224541519](C:\Users\WYX\AppData\Roaming\Typora\typora-user-images\image-20220807224541519.png)

