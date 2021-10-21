1. 这两个方法来自不同的类，sleep来自Thread类，wait来自Object类。sleep谁使用谁去休息。
2. 最主要的sleep方法没有释放锁，而wait方法释放了锁，使得其他线程可以使用同步控制块或者方法。

sleep不出让系统资源；wait是进入线程池等待，出让系统资源，其他线程可以占用CPU。	一般wait不会加时间限制，因为如果wait线程的运行资源不够，再出来也没用，要等待其他线程调用notify/notifyall唤醒等待池中的所有线程，才会进入就绪队列等待OS系统分配资源，sleep可以用时间指定她自动唤醒过来，如果时间不到只能用interrrupt强行打断。

Thread.Sleep（0）的作用是“触发操作系统立刻进行一次CPU竞争”。

\3. 使用范围：wait，notify和notifyall只能在同步控制方法或者同步控制块里面使用，而sleep可以在任何地方使用。

\4. sleep必须捕获异常，而wait，notify和notifyall不需要捕获异常。