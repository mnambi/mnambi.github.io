Thread Synchronization
====

1. Mutex
---

```C++
#include <boost/thread.hpp>
#include <boost/chrono.hpp>
#include <iostream>

void wait(int seconds)
{
	boost::this_thread::sleep_for(boost::chrono::seconds{seconds});
}

boost::mutex mutex;

void thread()
{
	wait(1);
	mutex.lock();
	for (int ii = 0; ii < 5; ++ii)
	{	
		std::cout << "Thread id: " << boost::this_thread::get_id() << ":  << ii << std::endl;
	}
	mutex.unlock();
}

void main()
{
	boost::thread thread1(thread);
	boost::thread thread2(thread);
	
	thread1.join(); 
	thread2.join();
}

```

2. boost::lock_guard
--- 

```C++
boost::lock_guard<boost::mutex> lock{mutex};
```

Locks the mutex in the constructor of the lock guard, and unlocks the mutex in the destructor of the lock guard. 

3. boost::unique_lock
--- 

```language
boost::timed_mutex mutex
boost::unique_lock<boost::timed_mutex> lock{mutex};
boost::unique_lock<boost::timed_mutex> lock{mutex, boost::try_to_lock}

```
boost::unique_lock works with boost::timed_mutex because it requires boost::try_to_lock which is only provided by boost::timed_mutex. The following calls can be used to check if a lock owns a mutex, and to wait for a particular period if it doesn't: 
```language
lock.owns_lock() || lock.try_lock_for(boost::chrono::seconds{1})
```
The destructor of unique_lock does not release the mutex. The following calls have to be used: 
```language
boost::time_mutex* m = lock.release(); 
m->unlock()
```

4. boost::shared_lock
Non-exclusive lock for read only threads. 

5. boost::condition_variable_any
Condition variables can be used to check a condition between different threads to ensure orderly execution of commands. 
```
boost::condition_variable_any condition;

condition.notify_all() // notify all threads that condition has been satisfied
condition.wait() // wait till notify_all is called by another thread
```

