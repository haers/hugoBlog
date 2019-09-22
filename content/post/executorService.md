---
title: 2.executorService
date: 2018-10-18 22:55:16
categories: [concurrency]
tags: [并发,concurrency,executorService]
---

## 类翻译
一个executor 可以提供方法来处理终止和产生一个 future 来记录一个或多个异步任务的进度。（mark，executor目前只是一个接口，目前还做不到。。。）
一个executorService可以被关闭，这将会导致它拒绝新的任务。ExecutorServie提供了两个关闭方法。shutdown 方法在终止之前允许先前提交的任务执行完。shutdownNow 方法阻止等待的任务启动 并且尝试停止当前正在执行的任务。一旦终止，Executor没有执行中的任务，没有等待中的任务，没有新任务可以提交。<!--more-->一个未被使用的ExecutorService应该被关闭来回收它的资源。
submit 方法扩展了 Executor的基本方法 execute，通过创建并返回Future，Future可以用来取消执行 和/或者 等待（执行）完成。invokeAny 和 invokeAll 方法 是执行最通用的，有用的集合执行形式。执行集合任务，然后等待至少一个或全部完成。ExecutorCompletionService可以被用来编写这些方法的定制变体。
Executors 为executor Service提供了方便的工厂方法。（mark：executor是也为executor提供了工厂）
使用示例：
这里有一个网络服务的草图，其中线程在线程池中来处理请求，他使用了预配置工厂方法（newFixedThreadPool）。

```
 *  <pre> {@code
 * class NetworkService implements Runnable {
 *   private final ServerSocket serverSocket;
 *   private final ExecutorService pool;
 *
 *   public NetworkService(int port, int poolSize)
 *       throws IOException {
 *     serverSocket = new ServerSocket(port);
 *     pool = Executors.newFixedThreadPool(poolSize);
 *   }
 *
 *   public void run() { // run the service
 *     try {
 *       for (;;) {
 *         pool.execute(new Handler(serverSocket.accept()));
 *       }
 *     } catch (IOException ex) {
 *       pool.shutdown();
 *     }
 *   }
 * }
 *
 * class Handler implements Runnable {
 *   private final Socket socket;
 *   Handler(Socket socket) { this.socket = socket; }
 *   public void run() {
 *     // read and service request on socket
 *   }
 * }}</pre>
```


以下方法分为两个阶段关闭 ExecutorService，第一步通过 shutdown 来拒绝传入的任务，然后调用 shutdownNow ，如果必要的话，可以用来取消任何延迟任务。

```
 *  <pre> {@code
 * void shutdownAndAwaitTermination(ExecutorService pool) {
 *   pool.shutdown(); // 禁止提交新任务
 *   try {
 *     // 等待现有任务终止
 *     if (!pool.awaitTermination(60, TimeUnit.SECONDS)) {
 *       pool.shutdownNow(); // 取消当前正在执行的任务
 *       // 等待一会让任务来反馈是否被取消了
 *       if (!pool.awaitTermination(60, TimeUnit.SECONDS))
 *           System.err.println("Pool did not terminate");
 *     }
 *   } catch (InterruptedException ie) {
 *     // （重新）取消如果当前线程被中断
 *     pool.shutdownNow();
 *     // 保留当前线程中断状态
 *     Thread.currentThread().interrupt();
 *   }
 * }}</pre>
```
内存一致性效果：和executor一样。

## 方法
todo 最近时间比较紧，尽力而为

