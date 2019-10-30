# QUEUE
We decided to make our queue from a doubly linked list in order to keep most operations to O(1) time complexity. Doubly linked lists allow easy enquing by having a tail pointer that inserts new data at the end without having to iterate through the whole queue.  They also allow fast dequing by simply moving the head pointer to the next node after the first node is grabbed. Double pointers also allow for a simpler implementation of delete compared to single linked lists. We needed to be able to delete any item in our queue, so we equipped our linked list  with a link to the previous and next pointer so that we could link the previous node to the next node.  

# TESTING_QUEUE
We tested the queue considering each function that the queue implemented.  To maintain the readability, we broke up the testing file into separate functions that would test one or two functions of the queue.  The testing was quite helpful as it enabled us to discover several errors in the edge cases of our queue.  We realized that we were not returning a -1 upon attempting to delete an element from an already empty queue, and it showed us that we were not updating the length variable in the delete queue.  

# UTHREAD
For our uthread code, we created a structure that represented a thread, that held its context, TID, stack pointer, status and any child threads it was waiting on. We initially tried storing our current thread, total number of threads and our queue pointer in a struct called library.  We soon discovered that this made our code difficult to read, and we had some malloc errors when creating an instance of this struct.  We took a step back and reconsidered the problem. We realized that we simply needed access to queues for ready, blocked and zombie threads, a thread pointer to the current thread, one int to keep track of the number of threads, and one main thread that we could update. Therefore, we decided to make each of these items global variables. For security, we changed these to static variables so that no program that implemented uthread.h could access them without calling uthread functions. We chose to hold the blocked and zombie threads in queues, though they effectively act as normal double linked lists.

# PREEMPT
To implement preempting in this project, we used a timer that sent a signal every 1/100th of a second. Our signal handler function would force the current running thread to yield. Our preempt would not start until the thread library was initialized. Preempt would also be disabled right before modifying the thread state and dequing the next thread to be run to prevent improper context switches.

# READABILITY
We decided to represent our four read running zombie and blocked states as 0, 1,2,3 and 4.  We initially thought of using strings for this, but this seemed to make the code unnecessarily complicated using string comparisons.   So we created several macros for this to improve readability and keep the code simple.  

# VOID POINTERS
To properly implement threads with queues, the queues need to be made with double void pointers. This initially caused confusion, especially with functions that required parameters to be passed as void double pointers. Eventually, it became clear to us that a double pointer is needed in order to switch the value pointed at by any single void pointer. Casting was also an issue at first, but we learned that a void pointer must first be cast to a type pointer before it can be dereferenced.

# TEST_PREEMPT
We decided to test preempt by first creating a thread that would run in an infinite loop.  We also created a second thread that would be later scheduled to run.  Because our first thread was caught in an infinite loop, our signal handler would would eventually call preempt after 1/100th of a second in order to allow the other threads to be called.  We were able to conclude that our preempt worked as all three threads successfully printed out their thread ID’s.  

# RESOURCES:
We used the links used in the homework prompt for the signal handling.  We also used the slides from lecture to help us with the thread create function.  We also went to computer science club’s tutoring hours to get help on the uthread data structures.
