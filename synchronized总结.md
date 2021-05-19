## synchronized总结

> 日常工作用不到，只能是学了忘，忘了学！今天总结一下，日后直接看本文章！
> 根据白鹤翔架构师内部视频学习总结！我感觉这个老师讲的不错，是2016年的视频．

---

- 线程安全

> 当多个线程访问某一个类(对象或方法)时，这个类始终都能表现出正确的行为，那么这个类(对象或方法)就是线程安全的．
> Synchronized:可以在任意对象及方法上加锁，而加锁的这段代码称为"互斥区"或"临界区"．

```java
public class MyThread extends Thread {

    private int count = 5;

    public synchronized void run(){
        count--;
        System.out.println(this.currentThread().getName()+" count = "+count);
    }

    public static void main(String[] args) {
        //一个对象，多个线程竞争一把锁
        MyThread myThread = new MyThread();
        Thread t1 = new Thread(myThread,"t1");
        Thread t2 = new Thread(myThread,"t2");
        Thread t3 = new Thread(myThread,"t3");
        Thread t4 = new Thread(myThread,"t4");
        Thread t5 = new Thread(myThread,"t5");
        t1.start();
        t2.start();
        t3.start();
        t4.start();
        t5.start();
    }
}
```

- #### *类锁**

> synchronized取得的锁都是对象锁，而不是把一段代码(方法)当做锁，所以new出来的对象是各自拥有各自的锁．
>
> 有一种情况是相同的锁，即在静态方法上加上synchronized关键字，表示类锁(独占.class类)

```java
/**
 * 类锁
 * 对象锁
 */
public class MultiThread {
    //static
    private int num = 0;
    
    //static
    public synchronized void printNum(String tag){
        try {
            if(tag.equals("a")){
                num = 100;
                System.out.println("tag a set num ");
                Thread.sleep(1000);
            }else {
                num = 200;
                System.out.println("tag b set num");
            }
            System.out.println("tag "+tag +",num = "+num);
        }catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        //创建两个对象,不加static,synchronized就属于本(m1,m2)对象的锁
        //加static，表示是类锁
        final MultiThread m1 = new MultiThread();
        final MultiThread m2 = new MultiThread();

        Thread t1 = new Thread(new Runnable() {
            public void run() {
                m1.printNum("a");
            }
        });

        Thread t2 = new Thread(new Runnable() {
            public void run() {
                m2.printNum("b");
            }
        });
        t1.start();
        t2.start();
    }
}
```

- #### 对象锁

> 同一对象里的同步方法和不加锁的方法执行互不影响
>
> 同步的概念就是共享，如果不共享的资源，就没必要加锁

```java
public class MyObject {

    public synchronized void method1(){
        try {
            System.out.println(Thread.currentThread().getName());
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public void method2(){
        System.out.println(Thread.currentThread().getName());
    }

    public static void main(String[] args) {
        final MyObject myObject = new MyObject();
        Thread t1 = new Thread(new Runnable() {
            public void run() {
                myObject.method1();
            }
        });

        Thread t2 = new Thread(new Runnable() {
            public void run() {
                myObject.method2();
            }
        });

        t1.start();
        t2.start();
    }
}
```

- #### 脏读

```tex
对同一对象操作，要保证对象的一致性，在set时加锁，get上不加，会导致对象属性不一致．
```

```java
public class DirtyRead {

    private String name = "hello";
    private String pwd = "world";

    public synchronized void setValue(String name,String pwd){
        this.name = name;
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        this.pwd = pwd;
        System.out.println("setValue最终的结果，name="+name+" ,pwd = "+pwd);
    }

    public void getValue(){
        System.out.println("~~~getValue最终的结果，name="+name+" ,pwd = "+pwd);
    }
    public static void main(String[] args) throws InterruptedException {
        final DirtyRead dr = new DirtyRead();

        Thread t1 = new Thread(new Runnable() {
            public void run() {
                dr.setValue("thread","sync");
            }
        });
        t1.start();//t1线程执行
        Thread.sleep(1000);//主线程休眠

        dr.getValue();

    }
}
```

- #### 锁重入

```tex
synchronized拥有锁重入的功能，也就是在使用synchronized时，当一个线程得到一个对象的锁后，再次请求对象时是可以再次获得该对象的锁．
```

```java
public class SynDUbbo1 {

    public synchronized void method1(){
        System.out.println("method1 ~~~~~~~");
        method2();
    }
    public synchronized void method2(){
        System.out.println("method2 ~~~~~~~");
        method3();
    }
    public synchronized void method3(){
        System.out.println("method3 ~~~~~~~");
    }

    public static void main(String[] args) {
        final SynDUbbo1 synDUbbo1 = new SynDUbbo1();

        Thread t1 = new Thread(new Runnable() {
            public void run() {
                synDUbbo1.method1();
            }
        });
        t1.start();
    }
}
```

```java
package com.test.sync;


public class SynDubbo2 {

    static class Main{
        public int i = 10;
        public synchronized void opsup(){
            try {
                i--;
                System.out.println("main print i = "+i);
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    static class Sub extends Main{

        public synchronized void opsub() {
            try {
                while (i>0){
                    i--;
                    System.out.println("sub print i= "+i);
                    Thread.sleep(100);
                    this.opsup();
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        Thread t1 = new Thread(new Runnable() {
            public void run() {
                Sub sub = new Sub();
                sub.opsub();
            }
        });
        t1.start();
    }
}
```
- #### 锁异常

```tex
在发生异常时，synchronized锁会释放，如果不想释放锁，就要处理好异常．
```

```java
public class SynException {

    private int i =0 ;
    public synchronized void op() throws Exception {
        while (true){
            try {
                i++;
                Thread.sleep(100);
                System.out.println(Thread.currentThread().getName()+", i = "+i);
                if(i ==10){
                    Integer.parseInt("a");
                }
            } catch (Exception e) {
                e.printStackTrace();
                System.out.println(" exception log i = "+ i);
                throw e;
                //continue;
            }
        }
    }

    public static void main(String[] args) {
        final SynException s = new SynException();
        Thread t1 = new Thread(new Runnable() {
            public void run() {
                try {
                    s.op();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        },"t1");
        Thread t2 = new Thread(new Runnable() {
            public void run() {
                try {
                    s.op();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        },"t2");
        t1.start();
        t2.start();
    }
}
```





