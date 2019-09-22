---
title: 1.executor
date: 2018-10-18 22:52:39
urlname: executor
categories: [concurrency]
tags: [并发,concurrency,executor]
---

## 类翻译：
这是一个执行提交的任务（Runnable）的对象。这个接口提供了一种解决任务提交的方法，就是每个任务如何运行的机制，包括线程如何使用的详情，调度等等。executor经常被用来替代 显示的创建线程。比如，
对于每个线程，我们不用使用 
<!--more-->
```
new Thread(new(RunnableTask())).start()
```

我们可以使用

``` 
* <pre>
 * Executor executor = <em>anExecutor</em>;
 * executor.execute(new RunnableTask1());
 * executor.execute(new RunnableTask2());
 * ...
 * </pre>
```

然而，executor接口并不严格要求执行是异步的，在最简单的情景下，一个executor可以在调用者的线程中立即运行一个提交的任务。

```
 *  <pre> {@code
 * class DirectExecutor implements Executor {
 *   public void execute(Runnable r) {
 *     r.run();
 *   }
 * }}</pre>
```

更典型的是，任务可以在刨除当前线程，生成其他线程来执行。下面的这个executor就产生（spawn，产卵这个词比较形象）一个新的线程对于每个任务。

```
 *  <pre> {@code
 * class ThreadPerTaskExecutor implements Executor {
 *   public void execute(Runnable r) {
 *     new Thread(r).start();
 *   }
 * }}</pre>
```

很多executor的实现都“加上”了某种这些任务如何被调度的限制。下面的executor将任务序列化后提交到第二个executor，图解说明了复合executor。

```
<pre> {@code
 * class SerialExecutor implements Executor {
 *   final Queue<Runnable> tasks = new ArrayDeque<Runnable>();
 *   final Executor executor;
 *   Runnable active;
 *
 *   SerialExecutor(Executor executor) {
 *     this.executor = executor;
 *   }
 *
 *   public synchronized void execute(final Runnable r) {
 *     tasks.offer(new Runnable() {
 *       public void run() {
 *         try {
 *           r.run();
 *         } finally {
 *           scheduleNext();
 *         }
 *       }
 *     });
 *     if (active == null) {
 *       scheduleNext();
 *     }
 *   }
 *
 *   protected synchronized void scheduleNext() {
 *     if ((active = tasks.poll()) != null) {
 *       executor.execute(active);
 *     }
 *   }
 * }}</pre>
```
ps:定义了一个队列，然后添加runnable对象，然后使用另外的executor去执行队列中的对象。但是吧，我任务这个每次执行一个，不过只是demo，可以改造成一个执行运行对象的方法。

在当前包中，ExecutorService 继承了Executor，ExecutorService 是一个更广泛的接口。ThreadPoolExecutor 提供了一个可扩展的线程池实现方式。Executors 为executors提供了方便的工厂方法。

内存一致性效果：在“优先”提交一个runnable对象到executor的线程中，happens-before executor的执行操作，（这个执行操作）可能在另外的线程中；
ps:没太懂，这里讲这个的意思，虽然知道happens-before原则，但是不太理解这里，有了解的可以私信我，或者留言。

## 方法翻译
整个接口只有一个方法：

```
void execute(Runnable command);
```

ps:没有返回值，只有一个runnable对象。

在将来的某个时间点，执行赋予的runnable，这个runnable可能在一个新线程，在一个线程池中的线程，或者在一个被调用的线程执行，不过这个由实现自己决定。
会抛出两种异常 
* RejectedExecutionException，task不被executor接受
* NullPointerException runnable 为空异常




