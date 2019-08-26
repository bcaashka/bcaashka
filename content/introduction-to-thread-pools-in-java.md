---
title: Introduction to Thread Pools in Java
seoTitle: Introduction to Thread Pools in Java. Ideal Thread Pool Size.
date: 2019-08-26T22:27:00+00:00
author: bcaashka
layout: post
permalink: /introduction-to-thread-pools-in-java/
categories:
  - Guides
tags:
  - guides
---
In Java, it's always been easy to run a method or piece of code asynchronous, that is if you have a `main` method which has a main thread, you can run your task in a separate thread, and you can do this very quickly using a code similar to this.<!--more-->

```java
    public static void main(String[] args) {
        Thread thread1 = new Thread(new Task());
        thread1.start();
        System.out.println("Thread name: " + Thread.currentThread().getName());
    }

    static class Task implements Runnable {
        public void run() {
            System.out.println("Thread name: " + Thread.currentThread().getName());
        }
    }
```

## Threads and Runnables
So, you can create a class, for example, `Task` which implements `Runnable` then override its run method and put the code that you want to run asynchronously. In this case, we are just doing the `System.out.println` which will print the name of the thread that it is running in. After that, you can create a new thread and, with the instance of this particular Task, start the thread. 

And here is the way to visualize it: 
{{< image title=\"\" alt=\"\" src=\"/images/2019/08/java-threads-and-runnable.jpg\" srcset=\"/images/2019/08/java-threads-and-runnable.jpg 960w, /images/2019/08/java-threads-and-runnable-768.jpg 768w, /images/2019/08/java-threads-and-runnable-640.jpg 640w\" >}}

There is a main thread which performs its operation from top to down, and at a certain point in time, it will do `thread.start()` and JVM will create a thread named Thread-0. This thread will do its operations. Once those operations are done JVM will kill that thread and the main thread will keep going. 

If you want to run ten tasks similarly, you can create the threads in a for loop. 

And each iteration it will create a new thread with a new instance of `Task` and will start that thread. And the way to visualize this is very similar:
{{< image title=\"\" alt=\"\" src=\"/images/2019/08/running-multiple-tasks-asynchronously.jpg\" srcset=\"/images/2019/08/running-multiple-tasks-asynchronously.jpg 960w, /images/2019/08/running-multiple-tasks-asynchronously-768.jpg 768w, /images/2019/08/running-multiple-tasks-asynchronously-640.jpg 640w\" >}}

JVM will create threads called Thread-0 till thread-9 denoted by t0 to t9 here and once the operations within the run method are complete JVM will kill those threads. 

## Executors
And what if you want to run 1000 tasks asynchronously?
{{< image title=\"\" alt=\"\" src=\"/images/2019/08/running-thousand-tasks-asynchronously.jpg\" srcset=\"/images/2019/08/running-thousand-tasks-asynchronously.jpg 960w, /images/2019/08/running-thousand-tasks-asynchronously-768.jpg 768w, /images/2019/08/running-thousand-tasks-asynchronously-640.jpg 640w\" >}}

The problem here is that one Java Thread corresponds to one operating system thread that means if you run the for loop thousand times, it will create one thousand threads. But creating a thread is an expensive operation so what you instead want is you want a fixed number of threads.

{{< image title=\"\" alt=\"\" src=\"/images/2019/08/java-thread-pool-example.jpg\" srcset=\"/images/2019/08/java-thread-pool-example.jpg 960w, /images/2019/08/java-thread-pool-example-768.jpg 768w, /images/2019/08/java-thread-pool-example-640.jpg 640w\" >}}

Let's have ten threads. Also, you want to create them upfront, let's call it a pool, a pool of threads, and let's submit a thousand tasks to it. After that, we want the threads to pick up those tasks and complete them. And the way to implement this is straightforward:
```java
    public static void main(String[] args) {
        int numberOfThreads = 10;
        ExecutorService executorService = Executors.newFixedThreadPool(numberOfThreads);
        for (int i = 0; i < 1000; i++) {
            executorService.execute(new Task());
        }
        System.out.println("Thread name: " + Thread.currentThread().getName());
    }

    static class Task implements Runnable {
        public void run() {
            System.out.println("Thread name: " + Thread.currentThread().getName());
        }
    }
```

You create a new **fixed thread pool** using the static method of `Executors` class. And in the same for loop that you used earlier but here instead of starting a new thread, you can submit or execute those tasks using the same service. And here is a visualization of that process:

{{< image title=\"\" alt=\"\" src=\"/images/2019/08/thread-pool-tasks-execution.jpg\" srcset=\"/images/2019/08/thread-pool-tasks-execution.jpg 960w, /images/2019/08/thread-pool-tasks-execution-768.jpg 768w, /images/2019/08/thread-pool-tasks-execution-640.jpg 640w\" >}}

Internally, that thread pool uses a blocking queue. The pool keeps storing all the tasks that you have submitted in that queue. And all the ten threads will perform the same two steps: fetch the next task from the queue and execute it. 

Since all ten threads attempt to take the task from the queue at the same time, you want the queue to be able to handle concurrent operations. So, you want a queue which is thread-safe, and that is why a **thread pool** uses the blocking queue.

## Ideal Thread Pool Size?
To be continued...