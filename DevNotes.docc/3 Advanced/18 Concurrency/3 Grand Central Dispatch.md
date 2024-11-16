# 3. Grand Central Dispatch (GCD)

## 3.1 Overview of GCD

Grand Central Dispatch (GCD) is Apple’s low-level concurrency framework 
that allows developers to manage tasks asynchronously. 
Introduced to help write efficient, responsive applications, 
GCD handles the complexity of managing threads and maximizes system resource use. 
While Swift has introduced a new concurrency model with `async`/`await`, 
GCD remains a powerful tool, especially for system-level and performance-critical tasks. 
GCD is often used alongside Swift’s structured concurrency tools 
in complex applications to handle low-level concurrency control.


## Why Use GCD in Modern Swift?

While Swift’s structured concurrency model with `async`/`await` 
simplifies handling asynchronous code, 
there are scenarios where GCD is particularly useful:

1. **Low-Level Control**:  
        For tasks requiring fine-grained control over system resources, 
        GCD’s dispatch queues, groups, and semaphores provide direct management capabilities.

2. **Performance Optimization**:  
        GCD enables developers to specify exact Quality of Service (QoS) for tasks, 
        allowing more control over CPU usage and optimizing system performance.

3. **Interoperability**:  
        In apps that combine Swift concurrency and legacy Objective-C code, GCD can help bridge these systems effectively,
        particularly in existing codebases that rely on dispatch queues.

4. **Specialized Concurrency Tasks**:  
        For operations that need unique concurrency patterns, 
        like throttling or timed operations, GCD’s capabilities, 
        including `DispatchWorkItem` and `DispatchSource`, allow precise task scheduling.

In sum, GCD is ideal for system-critical tasks, resource-sensitive work, 
and interoperability with legacy code. 
It complements Swift’s concurrency model, 
offering flexibility for advanced developers in performance-critical areas.


## Dispatch Queues and Quality of Service (QoS)

### What is a Dispatch Queue?

A **dispatch queue** is a lightweight object that manages the execution of tasks. 
Dispatch queues can be **serial** (one task at a time) 
or **concurrent** (multiple tasks at the same time). 

Queues are categorized into three main types:

- **Main Queue**:  
    Runs on the main thread and is used for updating the UI. 
    Only one task runs at a time in this queue.

- **Global Queues**:  
        Concurrent queues provided by the system 
        with varying QoS levels (quality of service).

- **Custom Queues**:  
        User-defined queues, allowing for specific configurations.

### Quality of Service (QoS)

QoS is used to prioritize tasks based on their importance.

In GCD, there are several QoS levels:

| QoS Class            | Type of Task                                                                                             | Duration of Task                                       |
|----------------------|----------------------------------------------------------------------------------------------------------|--------------------------------------------------------|
| **User-interactive** | Tasks that interact with the user immediately. For example, animations, changing the UI, etc.            | Task should be almost instantaneous.                   |
| **User-initiated**   | Tasks that the user initiated and require immediate feedback. The user is waiting for feedback to continue interaction. For example, anything that involves responding to a UI event. | Task should take a few seconds at the most.            |
| **Utility**          | Tasks that take time to complete and do not require immediate feedback. For example, downloading data, saving to the device, etc. | Task can take a few seconds to a few minutes.          |
| **Background**       | Tasks that are not time-sensitive and usually not visible to the user. For example, creating backups, synchronizing with a web service, etc. | Task can take a significant amount of time, on the order of minutes or hours. |


Setting the right QoS ensures that tasks are executed efficiently, balancing system performance and user experience.

```swift
let backgroundQueue = DispatchQueue.global(qos: .background)
backgroundQueue.async {
    print("Running a background task")
}
```


## Using DispatchQueue for Task Management

### Basic Syntax for DispatchQueue

DispatchQueue is used to manage tasks asynchronously or synchronously. 
With async, tasks are added to the queue and executed without blocking the caller. 
With sync, tasks are added to the queue and executed in the current thread, blocking the caller until the task completes.

```swift
// Asynchronous task
DispatchQueue.global().async {
    print("Running asynchronously")
}

// Synchronous task
DispatchQueue.global().sync {
    print("Running synchronously")
}
```


### Creating Custom Dispatch Queues

You can create your own serial or concurrent queues to manage specific groups of tasks. 
**Serial queues** execute tasks in order, one at a time, while **concurrent queues** allow tasks to run in parallel.

```swift
// Creating a custom serial queue
let serialQueue = DispatchQueue(label: "com.example.serialQueue")

serialQueue.async {
    print("Task 1")
}
serialQueue.async {
    print("Task 2")
}

// Creating a custom concurrent queue
let concurrentQueue = DispatchQueue(label: "com.example.concurrentQueue", attributes: .concurrent)

concurrentQueue.async {
    print("Concurrent Task 1")
}
concurrentQueue.async {
    print("Concurrent Task 2")
}
```

## Dispatch Group for Task Coordination

A DispatchGroup allows you to coordinate a set of tasks 
and be notified when all of them have completed. 
This is useful when you need to execute multiple tasks concurrently 
but want to perform an action only after all tasks are done.

### Key Use Cases

- Waiting for multiple tasks to complete before proceeding.
- Synchronizing concurrent tasks without blocking the main thread.
- Using DispatchGroup

To use a DispatchGroup, you add tasks to it using the enter and leave methods, 
or directly call asynchronous tasks with DispatchQueue.async(group:). 
When all tasks within the group are complete, the group can notify a completion handler.

let dispatchGroup = DispatchGroup()

```swift
// Adding tasks to the group
dispatchGroup.enter()
DispatchQueue.global().async {
    print("Task 1 started")
    sleep(2)
    print("Task 1 completed")
    dispatchGroup.leave()
}

dispatchGroup.enter()
DispatchQueue.global().async {
    print("Task 2 started")
    sleep(3)
    print("Task 2 completed")
    dispatchGroup.leave()
}

// Notify when all tasks in the group are complete
dispatchGroup.notify(queue: .main) {
    print("All tasks are complete")
}
```

## Practical Example with DispatchGroup

```swift
func fetchDataFromMultipleSources() {
    let dispatchGroup = DispatchGroup()
    
    dispatchGroup.enter()
    DispatchQueue.global().async {
        // Fetch data from source 1
        sleep(1)
        print("Data from source 1 fetched")
        dispatchGroup.leave()
    }
    
    dispatchGroup.enter()
    DispatchQueue.global().async {
        // Fetch data from source 2
        sleep(2)
        print("Data from source 2 fetched")
        dispatchGroup.leave()
    }
    
    dispatchGroup.notify(queue: .main) {
        print("All data fetched and ready to process")
    }
}
```

In this example:

Two asynchronous data-fetching tasks are added to a DispatchGroup.
dispatchGroup.notify is called on the main queue, 
and only executes after both tasks have completed, 
ensuring that we have all the data before proceeding.


## Summary: The Role of GCD in Modern Swift Concurrency

While Swift’s structured concurrency provides an easier, 
more structured approach to asynchronous programming with async/await, 
GCD remains an essential tool for certain cases:

- **System-Level Control**:  
        For performance-critical apps requiring low-level control, 
        GCD’s dispatch queues, groups, and semaphores offer precision and customization.

- **Legacy Code Support**:  
        Many existing Objective-C-based codebases rely on GCD. 
        Developers maintaining these codebases will still encounter 
        and use GCD to manage asynchronous work.

- **Fine-Grained Task Control**:  
        For tasks requiring exact timing, synchronization, or throttling, 
        GCD’s tools such as DispatchGroup, DispatchSemaphore, and DispatchSource are invaluable.

- **Flexible Concurrency Combinations**:  
        GCD can complement Swift’s concurrency model, 
        offering flexible solutions where async/await alone may not suffice.

Using GCD effectively allows developers to create responsive, high-performance apps, 
especially for tasks needing precise resource management. 
GCD and Swift concurrency can work together, 
with GCD handling specific low-level tasks, 
while async/await takes care of high-level asynchronous flows.
