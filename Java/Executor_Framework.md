# Chapter 6. Task Execution

## 6.1. Executing Tasks in Threads

### 6.1.1. Execute tasks sequentially in a single thread

```java
class SingleThreadWebServer {
    public static void main(String[] args) throws IOException {
        ServerSocket socket = new ServerSocket(80);
        while (true) {
            Socket connection = socket.accept();
            handleRequest(connection);
        } 
    }
}
```

### 6.1.2. Execute each task in its own thread

```java
class ThreadPerTaskWebServer {
    public static void main(String[] args) throws IOException {
        ServerSocket socket = new ServerSocket(80);
        while (true) {
            final Socket connection = socket.accept();
            Runnable task = new Runnable() {
                public void run() {
                    handleRequest(connection);
                } 
            };
            new Thread(task).start();
        } 
    }
}
```

The first step in organizing a program around task execution is identifying sensible task boundaries. Ideally, tasks are independent activities: work that doesn't depend on the state, result, or side effects of other tasks. Tasks are logical units of work, and threads are a mechanism by which tasks can run asynchronously. 

Processing a web request involves a mix of computation and I/O.

Creating a new thread for each request can consume significant computing resources.

---

## 6.2. The Executor Framework

It provides a standard means of decoupling task submission from task execution, describing tasks with Runnable.

Executor is based on the producer‐consumer pattern, where activities that submit tasks are the producers (producing units of work to be done) and the threads that execute tasks are the consumers (consuming those units of work).

```java
class TaskExecutionWebServer {
    private static final int NTHREADS = 100;
    private static final Executor exec
        = Executors.newFixedThreadPool(NTHREADS);
    
    public static void main(String[] args) throws IOException {
        ServerSocket socket = new ServerSocket(80);
        while (true) {
            final Socket connection = socket.accept();
            Runnable task = new Runnable() {
                public void run() {
                    handleRequest(connection);
                } 
            };
            exec.execute(task);
        } 
    }
}
```

### 6.2.2. Execution Policies

An execution policy specifies the "what, where, when, and how" of task execution, including:

- In what thread will tasks be executed?
- In what order should tasks be executed (FIFO, LIFO, priority order)?
- How many tasks may execute concurrently?
- How many tasks may be queued pending execution?
- If a task has to be rejected because the system is overloaded, which task should be selected as the victim, and how should the application be notified?
- What actions should be taken before or after executing a task?

### 6.2.3. Thread Pools

A thread pool manages a homogeneous pool of worker threads. A thread pool is tightly bound to a work queue holding tasks waiting to be executed. Worker threads have a simple life: request the next task from the work queue, execute it, and go back to waiting for another task.

### 6.2.4. Executor Lifecycle

An Executor implementation is likely to create threads for processing tasks. But the JVM can't exit until all the (non‐daemon) threads have terminated, so failing to shut down an Executor could prevent the JVM from exiting.

To address the issue of execution service lifecycle, the ExecutorService interface extends Executor, adding a number of methods for lifecycle management (as well as some convenience methods for task submission).

```java
public interface ExecutorService extends Executor {
    void shutdown();
    List<Runnable> shutdownNow();
    boolean isShutdown();
    boolean isTerminated();
    boolean awaitTermination(long timeout, TimeUnit unit)
        throws InterruptedException;
    //  ... additional convenience methods for task submission
}
```

The lifecycle implied by ExecutorService has three states ‐ running, shutting down, and terminated.

Tasks submitted to an ExecutorService after it has been shut down are handled by the rejected execution handler, which might silently discard the task or might cause execute to throw the unchecked RejectedExecutionException.

**ExecutorService APIs**:

- `invokeAny()`: accepts a collection of Callable tasks & returns a result of any one task that's completed successfully. Once it gets the result of the first successful operation, the rest of the tasks in the collection are cancelled.
- `invokeAll()`: accepts a collection of Callable tasks & returns a List of Futures, which holds the result returned by each task when all the asynchronous tasks are completed.

- `shutdown()`: no new tasks accepted, all previously submitted tasks are executed
- `shutdownNow()`: shuts down immediately
- `awaitTermination()`: waits until all tasks are completed after the shutdown request/time elapses/the current thread is interrupted. It returns true if the executor terminated, and false if the timeout elapsed before termination

---

## 6.3. Finding Exploitable Parallelism

The Executor framework makes it easy to specify an execution policy, but in order to use an Executor, you have to be able to describe your task as a Runnable.

### 6.3.2. Result Bearing Tasks: Callable and Future

Runnable's `run()` method cannot return a value or throw checked exceptions. For these types of tasks ‐ executing a database query, fetching a resource over the network, or computing a complicated function, Callable is a better abstraction: it expects that the main entry point, `call()`, will return a value and anticipates that it might throw an exception.

Future represents the lifecycle of a task and provides methods to test whether the task has completed or been cancelled, retrieve its result, and cancel the task.

The behavior of `get` varies depending on the task state (not yet started, running, completed). It returns immediately or throws an Exception if the task has already completed, but if not it blocks until the task completes.

#### Callable and Future Interfaces

```java
public interface Callable<V> {
    V call() throws Exception;
}

public interface Future<V> {
    boolean cancel(boolean mayInterruptIfRunning);
    boolean isCancelled();
    boolean isDone();
    V get() throws InterruptedException, ExecutionException,
                   CancellationException;
    V get(long timeout, TimeUnit unit)
        throws InterruptedException, ExecutionException,
               CancellationException, TimeoutException;
}
```

- renders the text -> CPU‐bound task  
- downloads all the images -> I/O‐bound task  

### 6.3.5. CompletionService: Executor Meets BlockingQueue

CompletionService combines the functionality of an Executor and a BlockingQueue.

- `executor.submit(task)` -> create a separate task for downloading each image  
- Multiple ExecutorCompletionServices can share a single Executor.  
- By remembering how many tasks were submitted and counting how many completed results are retrieved, you can know when all the results for a given batch have been retrieved, even if you use a shared Executor.  
- `timed Future.get` -> waits until time budget runs out. If timeout, cancels the task and uses fallback.  
- `executor.invokeAll()` -> fetch each result sequentially via its Future.  

---

# Chapter 8. Applying Thread Pools

Thread pools work best when tasks are homogeneous and independent. Mixing long‐running and short‐running tasks risks "clogging" the pool unless it is very large; submitting tasks that depend on other tasks risks deadlock unless the pool is unbounded.

## 8.1.1. Thread Starvation Deadlock

If tasks that depend on other tasks execute in a thread pool, they can deadlock. Even in larger thread pools, if all threads are executing tasks that are blocked waiting for other tasks still on the work queue. This is called thread starvation deadlock.

**NOTE**: Whenever you submit to an Executor tasks that are not independent, be aware of the possibility of thread starvation deadlock, and document any pool sizing or configuration constraints in the code or configuration file.

If your application uses a JDBC connection pool with ten connections and each task needs a database connection, it is as if your thread pool only has ten threads because tasks in excess of ten will block waiting for a connection.

## 8.2. Sizing Thread Pools

The ideal size for a thread pool depends on the types of tasks that will be submitted and the characteristics of the deployment system. Thread pool sizes should rarely be hard‐coded; instead pool sizes should be provided by a configuration mechanism or computed dynamically.

- For compute‐intensive tasks: `Ncpu + 1` threads  
- For I/O-bound: larger pool  
- Formula: `Nthreads = Ncpu * Ucpu * (1 + W / C)`  

```java
int Ncpu = Runtime.getRuntime().availableProcessors();
```

## 8.3. Configuring ThreadPoolExecutor

### 8.3.1. Thread Creation and Teardown

General constructor:

```java
public ThreadPoolExecutor(int corePoolSize,
                  int maximumPoolSize,
                  long keepAliveTime,
                  TimeUnit unit,
                  BlockingQueue<Runnable> workQueue,
                  ThreadFactory threadFactory,
                  RejectedExecutionHandler handler) { ... }
```

Example: Fixed-sized pool with bounded queue & Caller-runs policy:

```java
ThreadPoolExecutor executor
    = new ThreadPoolExecutor(N_THREADS, N_THREADS,
        0L, TimeUnit.MILLISECONDS,
        new LinkedBlockingQueue<Runnable>(CAPACITY));
executor.setRejectedExecutionHandler(
    new ThreadPoolExecutor.CallerRunsPolicy());
```

### 8.5. Parallelizing Recursive Algorithms

Transforming Sequential Execution into Parallel Execution.

```java
void processSequentially(List<Element> elements) {
    for (Element e : elements)
        process(e);
}

void processInParallel(Executor exec, List<Element> elements) {
    for (final Element e : elements)
        exec.execute(new Runnable() {
            public void run() { process(e); }
        }); 
}
```

---

## Executor Hierarchy

```
Executor
    ↑
ExecutorService
    ↑
ScheduledExecutorService
```

### ScheduledExecutorService

- Subinterface of ExecutorService  
- `schedule()`, `scheduleWithFixedDelay()`, `scheduleAtFixedRate()`  
- Returns `ScheduledFuture`  

### ThreadFactory API

- Default factory creates threads  
- Custom factories can set name, priority, daemon flag  

---

## CompletableFuture

- Implements `Future` + `CompletionStage`  
- Supports async callbacks & chaining  
- Key methods: `complete()`, `supplyAsync()`, `runAsync()`, `thenApply()`, `thenCompose()`, `handle()`, `exceptionally()`  

**Limitations of Future**:  
1. No manual completion  
2. No notification on completion  
3. No chaining  
4. No exception handling  

---

## Concurrency vs Parallelism

- **Concurrent**: multiple tasks "seemingly" at same time (context switching on single CPU)  
- **Parallel**: true simultaneous execution on multi-core CPUs  

**Parallelism**:  
- Based on task splitting into subtasks  
- Requires multi-core CPU  

---

## Fork/Join Framework

- Extension of ExecutorService  
- Supports divide-and-conquer parallelism  
- Used in parallel streams and `Arrays.parallelSort()`  

**ExecutorService vs ForkJoinPool**:  
- ExecutorService = general-purpose pool  
- ForkJoinPool = optimized for lightweight subtasks  

---

## Real-world Example

- 2-core CPU  
- Tasks take 1-2 seconds  
- Expected: 800/min (spike: 2500/min)  
- Task = object creation + HTTP call  

**Queue Sizing**:  
- If tasks must complete <100s  
- Limit queue to 50-100 tasks  
- Beyond this, reject new tasks  

---

## Little’s Law

```
Number of requests in system = (arrival rate) × (average service time)
```

If this number < pool size → pool can be reduced.  
