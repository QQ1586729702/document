### 进程与线程

![[clipboard (21).png]]

**进程**：程序执行时的过程

**线程**：**一个进程**中可以包含**很多线程**，进程至少包含**一个**线程

多线程真正的是**多核**（多个CPU），一个CPU同一时间只能执行一个代码，现 在电脑多为多核

![[clipboard (22).png]]

波浪号指的是线程安全

### 线程创立的三种方式
![[clipboard (23).png]]

- 继承`Thread`类
![[clipboard (24).png]]

- 实现`Runnable`接口
![[clipboard (25).png]]

- 实现`Callable`接口


#java #java基础 