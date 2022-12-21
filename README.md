# OpenMP 笔记 

omp_get_wtime()

OpenMP是由OpenMP Architecture Review Board牵头提出的，并已被广泛接受的，用于共享内存并行系统的多线程程序设计的一套指导性注释（Compiler Directive）.
要进行并行执行的代码片段需要进行相应的标记，用预编译指令使得在代码片段被执行前生成线程，每个线程会分配一个id，可以通过函数(called omp_get_thread_num())来获得该值，该值是一个整数，主线程的id为0。在并行化的代码运行结束后，子线程join到主线程中，并继续执行程序。

语法：
#pragma omp <directive> [clause[[,] clause] ...]

directive
其中，directive共11个：

atomic 内存位置将会原子更新（Specifies that a memory location that will be updated atomically.）
barrier 线程在此等待，直到所有的线程都执行到此barrier。用来同步所有线程。
critical 其后的代码块为临界区，任意时刻只能被一个线程执行。
flush 所有线程对所有共享对象具有相同的内存视图（view of memory）
for 用在for循环之前，把for循环并行化由多个线程执行。循环变量只能是整型
master 指定由主线程来执行接下来的程序。
ordered 指定在接下来的代码块中，被并行化的 for循环将依序执行（sequential loop）
parallel 代表接下来的代码块将被多个线程并行各执行一遍。
sections 将接下来的代码块包含将被并行执行的section块。
single 之后的程序将只会在一个线程（未必是主线程）中被执行，不会被并行执行。
threadprivate 指定一个变量是线程局部存储（thread local storage）

#pragma omp target


clause
共计13个clause：

copyin 让threadprivate的变量的值和主线程的值相同。
copyprivate 不同线程中的变量在所有线程中共享。
default Specifies the behavior of unscoped variables in a parallel region.
firstprivate 对于线程局部存储的变量，其初值是进入并行区之前的值。
if 判断条件，可用来决定是否要并行化。
lastprivate 在一个循环并行执行结束后，指定变量的值为循环体在顺序最后一次执行时获取的值，或者#pragma sections在中，按文本顺序最后一个section中执行获取的值。
nowait 忽略barrier的同步等待。
num_threads 设置线程数量的数量。默认值为当前计算机硬件支持的最大并发数。一般就是CPU的内核数目。超线程被操作系统视为独立的CPU内核。
ordered 使用于 for，可以在将循环并行化的时候，将程序中有标记 directive ordered 的部分依序执行。
private 指定变量为线程局部存储。将一个或多个变量声明为线程的私有变量
reduction Specifies that one or more variables that are private to each thread are the subject of a reduction operation at the end of the parallel region.
schedule 设置for循环的并行化方法；有 dynamic、guided、runtime、static 四种方法。
schedule(static, chunk_size) 把chunk_size数目的循环体的执行，静态依序指定给各线程。
schedule(dynamic, chunk_size) 把循环体的执行按照chunk_size（缺省值为1）分为若干组（即chunk），每个等待的线程获得当前一组去执行，执行完后重新等待分配新的组。
schedule(guided, chunk_size) 把循环体的执行分组，分配给等待执行的线程。最初的组中的循环体执行数目较大，然后逐渐按指数方式下降到chunk_size。
schedule(runtime) 循环的并行化方式不在编译时静态确定，而是推迟到程序执行时动态地根据环境变量OMP_SCHEDULE 来决定要使用的方法。
shared 指定变量为所有线程共享。
