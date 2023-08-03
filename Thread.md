# [Thread](https://zhuanlan.zhihu.com/p/632718569)

>任何一个对象都有一个 Monitor 与之关联，当且一个 Monitor 被持有后，它将处于锁定状态。Synchronized 在 JVM 里的实现都是 基于进入和退出 Monitor 对象来实现方法同步和代码块同步。


```
/**
 * 描述：     展示wait和notify的基本用法 1. 研究代码执行顺序 2. 证明wait释放锁
 */
public class Wait {
​
    public static Object object = new Object();
​
    static class Thread1 extends Thread {
​
        @Override
        public void run() {
            synchronized (object) {
                System.out.println(Thread.currentThread().getName() + "开始执行了");
                try {
                    object.wait(); //进入阻塞状态会释放锁
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("线程" + Thread.currentThread().getName() + "获取到了锁。");
            }
        }
    }
​
    static class Thread2 extends Thread {
​
        @Override
        public void run() {
            synchronized (object) {
                object.notify(); //唤醒正在等待这把锁的单个线程
                System.out.println("线程" + Thread.currentThread().getName() + "调用了notify()");
            }
        }
    }
​
    public static void main(String[] args) throws InterruptedException {
        Thread1 thread1 = new Thread1();
        Thread2 thread2 = new Thread2();
        thread1.start();
        Thread.sleep(200);
        thread2.start();
    }
}
```

***上面的代码表示，线程 A 在使用 wait() 方法后会进入阻塞状态并释放object的锁，然后另一个线程 B 会获取到该object的锁，在执行 notify() 方法后，会唤醒等待这把锁的 线程 A ，然后线程 A 继续执行!***







