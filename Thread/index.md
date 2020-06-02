#### Thread ####
线程的状态：

* new 。线程创建成功，但是没有运行。
* runnable。线程正在运行。线程创建成功后，执行start。就会处于运行中。
* terminated。线程结束。当线程运行完成，被终止、被打断都会从runable 到 该状态。
* block。线程阻塞。当线程正在等待获取锁时，比如进入 synchronized修饰的代码。会从runnable状态到该状态。
* waiting 和  timed_waiting. 等待和带超时的等待。在遇到Object#wait 或 Thread#join  或 Thread#sleep 等方法时，线程等待另外一个线程执行完成后，才能继续向下执行。

以上6种状态并不是线程的所有状态。

守护线程。

默认创建的线程都是非守护线程。守护线程的优先级很低。JVM退出时，不关心有无守护线程。


线程的相关操作

* join：当前线程等待另一个线程的执行完成，才继续操作。

* yield: 一个native方法。当前线程做出让步，放弃当前cpu，重新竞争。避免线程过度使用cpu。

* sleep：线程休眠。但是不会释放锁。

* interrupt: 打算正在运行的线程。如果线程处于 waiting/timed_waiting状态，则会抛出异常。


#### Future ####
FutureTask 有两个构造方法，一个是接受Callable接口，一个是接受Runnable、result的接口。

Callable是可以有返回值。所以使用前者线程可以有返回值。FatureTask适配了两种方式。
