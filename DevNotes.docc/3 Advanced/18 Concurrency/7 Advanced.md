# 7. Advanced Concurrency Techniques

In this article, we'll explore advanced concurrency techniques that help build high-performance and responsive Swift applications. Understanding these concepts will allow you to master fine-grained control over concurrency, making your code efficient and optimized for real-world scenarios, especially in large-scale desktop and mobile applications.

## 7.1 Throttling and Debouncing

### Overview

**Throttling** and **debouncing** are two techniques to control how often a function executes over time, both useful for optimizing performance and resource management.

- **Throttling**: Ensures that a function runs at most once in a specified time interval.
- **Debouncing**: Ensures that a function only runs after a certain period of inactivity.

These techniques are essential in situations where tasks are triggered by frequent events, such as UI interactions or network requests.

### Use Cases

- **Throttling**: Useful when continuously polling for updates, such as location tracking.
- **Debouncing**: Ideal for handling user input events, such as search queries as the user types.

### Implementing Throttling

```swift
class Throttler {
    private var lastRun = Date(timeIntervalSince1970: 0)
    private let queue = DispatchQueue.global(qos: .userInteractive)
    private let interval: TimeInterval
    
    init(interval: TimeInterval) {
        self.interval = interval
    }
    
    func throttle(action: @escaping () -> Void) {
        let now = Date()
        let delay = interval - now.timeIntervalSince(lastRun)
        
        if delay <= 0 {
            lastRun = now
            action()
        } else {
            queue.asyncAfter(deadline: .now() + delay) { [weak self] in
                self?.lastRun = Date()
                action()
            }
        }
    }
}
Implementing Debouncing
class Debouncer {
    private let interval: TimeInterval
    private var workItem: DispatchWorkItem?
    
    init(interval: TimeInterval) {
        self.interval = interval
    }
    
    func debounce(action: @escaping () -> Void) {
        workItem?.cancel()
        workItem = DispatchWorkItem { action() }
        DispatchQueue.main.asyncAfter(deadline: .now() + interval, execute: workItem!)
    }
}
7.2 Managing Large Task Sets with Operation Queues

Overview
Swift’s OperationQueue provides an advanced level of control for managing task dependencies, setting priorities, and handling task groups. Unlike GCD, OperationQueue is ideal for complex workflows where you need to track, cancel, or pause individual tasks and control dependencies between tasks.

Key Features
Task Prioritization: Adjust priorities of operations within the queue.
Dependencies: Define dependencies between operations to ensure correct execution order.
Pause and Cancel: Easily pause or cancel queued operations.
Example of Using OperationQueue
let queue = OperationQueue()
queue.maxConcurrentOperationCount = 3

let operation1 = BlockOperation {
    print("Task 1")
}

let operation2 = BlockOperation {
    print("Task 2")
}

operation2.addDependency(operation1) // Ensures Task 2 runs after Task 1

queue.addOperations([operation1, operation2], waitUntilFinished: false)
OperationQueue provides higher-level control over tasks, making it ideal for workflows where tasks have dependencies or require dynamic management.

7.3 Task Cancellation in Swift Concurrency

Overview
Task cancellation is essential in concurrency, allowing you to terminate long-running tasks that are no longer needed, such as a network request for data that the user has navigated away from.

Using Task.cancel() and checkCancellation()
In Swift’s concurrency model, cancellation is cooperative, meaning tasks check for cancellation periodically and exit gracefully if canceled.

Example of Task Cancellation
let task = Task {
    for i in 1...10 {
        try Task.checkCancellation()
        print("Processing \(i)")
        try await Task.sleep(nanoseconds: 1_000_000_000)
    }
}

task.cancel() // Cancels the task
In this example, Task.checkCancellation() throws an error if the task is canceled, allowing you to gracefully exit.

Use Case
Canceling Network Calls: If a user navigates away from a screen, cancel any ongoing data fetches.
Interruptible Processing: Break down tasks into smaller units, checking for cancellation regularly.
7.4 Combining Async and Sync Code

Overview
Combining asynchronous (async) and synchronous (sync) code is essential in Swift, allowing you to bridge older synchronous APIs with the newer Swift concurrency model. Using Task and TaskGroup within async functions helps you execute blocking tasks on background queues without blocking the main thread.

Bridging Sync Code in Async Context
If you need to perform a synchronous operation within an async function, wrap it in Task.detached or a background queue to avoid blocking the main thread.

func loadData() async {
    await Task {
        let data = fetchDataSync() // Synchronous function call
        print("Data fetched:", data)
    }
}
Practical Example of Bridging Async and Sync Code
func loadImageSynchronously() -> UIImage {
    // Simulate sync image loading
}

func loadImage() async -> UIImage {
    return await Task.detached {
        return loadImageSynchronously()
    }.value
}
Key Takeaways for Advanced Concurrency Techniques

Swift’s concurrency model and advanced techniques such as OperationQueue, task cancellation, and async bridging provide powerful tools to manage complex workflows in modern applications. By mastering these tools, you can design apps that are responsive, efficient, and ready for any level of concurrency challenge.
