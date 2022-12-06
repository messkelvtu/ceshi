## SpringBootTask定时任务

2022年10月8日

14:28

**一、简单应用（单线程）**

1. 启动类加@EnableScheduling（默认扫描启动类的同路径）
2. 创建一个类交给spring管理@Component
3. 该类的方法上@Scheduled(cron =     "*/5 * * * * ?")（每个使用到定时任务的方法）

 

**二、异步调用**

1. 启动类加@EnableScheduling（默认扫描启动类的同路径）
2. 创建一个类交给spring管理@Component
3. 该类的方法上@Scheduled(cron     = "*/5 * * * * ?")（每个使用到定时任务的方法）
4. 该类上加@EnableAsync
5. 方法上加@Async（每个使用到定时任务的方法）

 **效果：**

1. 同一方法（同一个任务）时间到达就会执行，不管前面的任务是否执行完（同时执行）。
2. 不同方法（不同任务），执行时机相同，同时执行。

 

**三、线程池配置**

1. 启动类加@EnableScheduling（默认扫描启动类的同路径）
2. 创建一个类交给spring管理@Component
3. 该类的方法上@Scheduled(cron     = "*/5 * * * * ?")（每个使用到定时任务的方法）
4. 配置线程池（并交给spring管理）

```java
  @Bean

  public ThreadPoolTaskScheduler testExecutor() {

​    ThreadPoolTaskScheduler threadPoolTaskScheduler = new ThreadPoolTaskScheduler();

​    // 配置线程池大小

​    threadPoolTaskScheduler.setPoolSize(20);

​    // 设置线程名

​    threadPoolTaskScheduler.setThreadNamePrefix("task-scheduling-");

​    // 设置等待任务在关机时完成

​    //  threadPoolTaskScheduler.setWaitForTasksToCompleteOnShutdown(true);

​    // 设置等待终止时间

​    // threadPoolTaskScheduler.setAwaitTerminationSeconds(60);

​    return threadPoolTaskScheduler;

  }
```

 

 **效果：**

1. 同一方法（同一个任务）时间到达，须排队等待上一次任务执行完毕，再执行下一次任务。（同一任务同步执行）
2. 不同方法（不同任务），执行时机相同，同时执行。（不同任务异步执行）

 

**注意：**

1. 如果多个不同任务操控同一资源，由于异步执行，肯定会导致线程问题，核心操作共享变量须加锁。
2. 任务间隔相同的情况下，为业务解耦，应归为同一定时任务。
3. 如果任务在某个时刻，同时执行，而又需保证A任务在B任务前执行，可以用redis来控制获取锁的顺序。
