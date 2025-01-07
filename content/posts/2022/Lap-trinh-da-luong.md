---
title: "Lập Trình Đa Luồng Trong Java: Hướng Dẫn và Ví Dụ"
date: 2024-12-25
description: Khám phá lập trình đa luồng trong Java, bao gồm khái niệm, cách tạo luồng, và các kỹ thuật đồng bộ hóa.
image: images/java-multithreading.png
caption: Tối ưu hóa hiệu năng với Multithreading trong Java
categories:
  - programming
tags:
  - java
  - multithreading
  - programming
  - development
draft: false
---

## 1. Lập trình đa luồng là gì?

Lập trình đa luồng (Multithreading) là kỹ thuật thực thi nhiều luồng (threads) trong một tiến trình (process). Điều này cho phép chương trình thực hiện đồng thời nhiều tác vụ, cải thiện hiệu suất và tận dụng tối đa tài nguyên hệ thống.

### Các khái niệm cơ bản:
- **Thread**: Một đơn vị thực thi nhỏ nhất trong tiến trình.
- **Main Thread**: Luồng chính trong một ứng dụng Java.
- **Concurrency**: Thực thi đồng thời nhiều luồng.
- **Synchronization**: Đồng bộ hóa các luồng để tránh xung đột dữ liệu.

## 2. Tạo và chạy luồng trong Java

### 2.1 Sử dụng lớp `Thread`
Kế thừa lớp `Thread` và override phương thức `run()`.

```java
class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Luồng: " + getName() + " -> " + i);
        }
    }
}

public class ThreadExample {
    public static void main(String[] args) {
        MyThread thread1 = new MyThread();
        MyThread thread2 = new MyThread();
        thread1.start();
        thread2.start();
    }
}
```
### 2.2 Sử dụng giao diện Runnable
Triển khai giao diện Runnable và truyền vào đối tượng Thread.
```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println("Luồng đang chạy: " + i);
        }
    }
}

public class RunnableExample {
    public static void main(String[] args) {
        Thread thread = new Thread(new MyRunnable());
        thread.start();
    }
}
```
### 2.3 Sử dụng biểu thức Lambda
Dễ dàng tạo luồng với cú pháp gọn gàng hơn.
```java
public class LambdaThreadExample {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            for (int i = 1; i <= 5; i++) {
                System.out.println("Lambda Luồng: " + i);
            }
        });
        thread.start();
    }
}
```
## 3. Đồng bộ hóa luồng (Synchronization)
Khi nhiều luồng cùng truy cập và thay đổi dữ liệu chia sẻ, cần sử dụng đồng bộ hóa để tránh xung đột.
### 3.1 Sử dụng từ khóa synchronized
```java
class SharedResource {
    public synchronized void printNumbers(String threadName) {
        for (int i = 1; i <= 5; i++) {
            System.out.println(threadName + " -> " + i);
        }
    }
}

class MyThread extends Thread {
    private SharedResource resource;

    public MyThread(SharedResource resource) {
        this.resource = resource;
    }

    @Override
    public void run() {
        resource.printNumbers(Thread.currentThread().getName());
    }
}

public class SynchronizationExample {
    public static void main(String[] args) {
        SharedResource resource = new SharedResource();
        MyThread thread1 = new MyThread(resource);
        MyThread thread2 = new MyThread(resource);

        thread1.start();
        thread2.start();
    }
}
```
### 3.2 Sử dụng khối synchronized
```java
class SharedData {
    public void printData(String threadName) {
        synchronized (this) {
            for (int i = 1; i <= 5; i++) {
                System.out.println(threadName + " -> " + i);
            }
        }
    }
}

public class SynchronizedBlockExample {
    public static void main(String[] args) {
        SharedData data = new SharedData();
        Thread thread1 = new Thread(() -> data.printData("Thread 1"));
        Thread thread2 = new Thread(() -> data.printData("Thread 2"));

        thread1.start();
        thread2.start();
    }
}
```
## 4. Phối hợp giữa các luồng
### 4.1 Phương thức wait() và notify()
Được sử dụng để phối hợp luồng trong một tài nguyên chung.
```java
class SharedQueue {
    private boolean available = false;

    public synchronized void produce() throws InterruptedException {
        while (available) {
            wait();
        }
        System.out.println("Sản xuất dữ liệu");
        available = true;
        notify();
    }

    public synchronized void consume() throws InterruptedException {
        while (!available) {
            wait();
        }
        System.out.println("Tiêu thụ dữ liệu");
        available = false;
        notify();
    }
}

public class WaitNotifyExample {
    public static void main(String[] args) {
        SharedQueue queue = new SharedQueue();

        Thread producer = new Thread(() -> {
            try {
                for (int i = 0; i < 5; i++) {
                    queue.produce();
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        Thread consumer = new Thread(() -> {
            try {
                for (int i = 0; i < 5; i++) {
                    queue.consume();
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        producer.start();
        consumer.start();
    }
}
```
## 5. Executor Service: Quản lý luồng hiệu quả
Thay vì quản lý trực tiếp, bạn có thể sử dụng ExecutorService.
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ExecutorServiceExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(2);

        executor.execute(() -> {
            for (int i = 1; i <= 5; i++) {
                System.out.println("Task 1: " + i);
            }
        });

        executor.execute(() -> {
            for (int i = 1; i <= 5; i++) {
                System.out.println("Task 2: " + i);
            }
        });

        executor.shutdown();
    }
}
```
## 6. Kết luận
Lập trình đa luồng trong Java giúp tận dụng hiệu quả tài nguyên và nâng cao hiệu suất ứng dụng. Tuy nhiên, việc quản lý luồng và đồng bộ hóa là thách thức lớn, đòi hỏi sự cẩn thận để tránh các vấn đề như deadlock hoặc race condition.
