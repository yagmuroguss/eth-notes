A semaphore is a counter that controls how many threads can access a share resource at the same time. It supports two atomic operations:
- acquire(S) / P(S) / wait(S) : wait until S > 0, then do S = S-1.
- release(S) / V(S) / signal(S): do S=S+1. This potentially wakes up a thread.