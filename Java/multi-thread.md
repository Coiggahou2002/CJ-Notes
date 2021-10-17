# Java 多线程

Process 进程：执行程序的一次执行过程，是系统资源分配的单位.

Thread 线程：线程是 CPU 调度和执行的单位，一个进程可以包含一到多个线程.

多核 CPU 才是真正的多线程，单核 CPU 上多线程都是模拟的.

+ 程序运行时，`main()` 是主线程，此外还有垃圾回收器线程
+ 线程的调度由操作系统完成，不能人为干预
+ 对同一份资源操作时，会存在资源抢夺的问题，需要加入并发控制

## 创建方式

### 继承 Thread 类

创建一个新的类，让他继承 Thread 类，然后重写 `run()` 方法，把想要使用新线程执行的代码放在 `run()` 方法中，最后在主线程中开启新线程即可.

```java
public class NewTread extends Thread {
    @Override
    public void run(){
        for (int i = 0; i < 20; i ++) {
            System.out.println("副线程--" + i);
        }
    }
}

public static void main(String[] args) {
    NewThread t = new NewThread();
    t.start();
    for (int i = 0; i < 200; i++) {
        System.out.println("主线程--" + i);
    }
}

// 在系统的调度下，两条线程同时执行，主线程副线程的输出会交替出现
```



### 实现 Runnable 接口

### 实现 Callable 接口

