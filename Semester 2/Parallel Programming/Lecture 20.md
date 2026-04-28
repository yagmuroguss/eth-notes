## <font color="#95b3d7">Barriers</font>
A barrier is a synchronization tool that requires all threads to reach a specific point in execution before any of them can proceed. Important barrier terms:
- <font color="#95b3d7">Init (Initialization):</font> The values that the variables take before the execution of the program.
- <font color="#95b3d7">Pre (Pre-Barrier):</font> The work threads do before reaching to the barrier.
- <font color="#95b3d7">Post (Post-Barrier):</font> The work threads do after the barrier.
The two-phase barrier works as follows: the variables mutex, barrier1, barrier2, and count are all semaphores. A value of 0 means the semaphore is locked, while a value greater than 0 means the gate is open. The first thread acquires the mutex and increments count, then releases the mutex. After that, it reaches the acquire(barrier1) step. However, since barrier1 = 0, it is locked and cannot be acquired, so the thread blocks. The thread will wait until the nth thread opens the barrier. When barrier1 is released, its value becomes 1, allowing threads to pass through the barrier one by one.
```java
mutex=1; barrier1=0; barrier2=1; count=0
acquire(mutex)
	count++;
	if (count==n) {
		acquire(barrier2); release(barrier1)}
release(mutex)
acquire(barrier1); release(barrier1);

acquire(mutex)
	count--;
	if (count==0) {
		acquire(barrier1); release(barrier2) }
release(mutex)
//This is for the threads not to leave the group and start early to the loop
acquire(barrier2); release(barrier2)
```
## <font color="#95b3d7">Producer-Consumer Pattern</font>
This pattern can be used to build data-flow parallel programs. Each node is called a pipeline node. Producer nodes take work from a data queue, process it, and add it to a queue for consumer threads to use.
With semaphores, it is easy to introduce a deadlock, since they are unstructured. 

## <font color="#95b3d7">Monitors</font>
A monitor is a structure that brings together data + lock + waiting mechanism in one place, allowing only one thread to access shared data at a time. If necessary, threads wait inside and continue when notified.
With encapsulation, the data and the code that processes that data are in the same package. An external thread cannot reach the data directly. It must use the methods of the monitor. When using monitors, if a method is called, the monitor automatically acquires the lock. When the work is done, monitor automatically releases the lock (the lock that monitors use is the intrinsic lock). Monitors provide:
- Mutual exclusion
- A mechanism to check conditions
If a condition does not hold in a monitor, it releases the monitor lock, waits for the condition to become true and signals (the signaling mechanism prevents busy-loops (spinning), which eats up the CPU time).
 ![[Pasted image 20260428114835.png]]Monitors have wait() and signal() mechanisms. A Thread B goes to sleep waiting for a condition to be met, and another Thread A signals when the condition is fulfilled. As soon as Thread A signals, Thread A releases the monitor lock and goes to sleep (wait) (goes to Entry Queue). It passes the lock to Thread B. The woken thread has the highest priority. 
 When Thread B wakes up, we can be 100% sure that the condition is met, since no other thread has intervened.

### <font color="#95b3d7">Condition Interface</font>
In classic monitors (synchronized), there is only one waiting queue. Condition interface allows us to create multiple waiting queues. Conditions are always associated with a lock. Some methods (all called with the **lock held**):
- <font color="#95b3d7">.await():</font> Current thread waits until condition is signaled. Atomically releases the lock and waits until the thread is signaled. When returns, it is guaranteed to hold the lock.
- <font color="#95b3d7">.signal(): </font>Wakes up one thread waiting on this condition.
- <font color="#95b3d7">.signalAll():</font> Signals all threads waiting on this condition.
## <font color="#95b3d7">Sleeping Barber - Dijkstra's Version</font>
Normally, when a thread finishes its job, it blindly sends a signal. The problem is that if no thread is waiting, or if the waiting threads require additional conditions to be met, the signal is wasted. As a result, CPU cycles and system resources are used unnecessarily. Dijkstra proposed a solution to this problem. 
Instead of using merely locks, he proposed two counters m and n (these act like semaphores), such that m is for the producers and n is for the customers. Counter m (Available Slots / Producer Perspective) represents the available capacity within the buffer. Assume there are m initial seats. Each time a client (producer) arrives, the counter is decremented (m←m−1). When m=0, it indicates that all seats are occupied and the buffer is full. If m<0, the value has dropped below zero, meaning that ∣m∣ clients are currently waiting outside because they could not find a seat. Essentially, this counter determines whether a producer is permitted to enter the buffer or must block and wait.