# 2 Swift Concurrency Model

Using Swift's Concurrency Model for Asynchronous Programming

## 2.1 What is Structured Concurrency?

Structured concurrency organizes tasks hierarchically, 
helping manage task lifecycles and dependencies in a predictable way. 
In Swift, structured concurrency allows tasks to be nested, 
ensuring that they complete within a defined scope, 
making it easier to track and handle errors.

#### Key Benefits
- **Automatic Task Management**: 
Tasks are managed hierarchically, 
making it easier to control their execution and completion.

- **Error Handling**: 
Errors are propagated in a structured way, allowing for safer code.

- **Scoped Resource Management**: 
Tasks finish within the scope they were created, 
minimizing resource leaks and simplifying cleanup.

Structured concurrency in Swift is a step forward from older concurrency approaches, 
such as GCD, providing clearer, more maintainable code.

---

## 2.2 Tasks in Swift Concurrency

### What is a Task?

A **Task** represents a unit of asynchronous work, like a network request or a calculation.
Tasks allow you to perform work in the background without blocking the main thread, 
improving app responsiveness. Tasks in Swift are cancellable, 
meaning they can be stopped if they are no longer needed.

#### Creating and Using Tasks

Tasks can be created in two main ways:

@Row{
    @Column{
        **Directly with Task**: 
        
        Suitable for work tied to the current async context.
        
    }
    @Column{
        **DetachedTask**: 
                
        Unlike regular tasks, which inherit priority and execution context, 
        detached tasks don’t carry over any of this information.
    }
}

@Row{
    @Column{
        ```swift
        // Creating a basic Task
        Task {
        let data = await fetchData()
        print("Fetched data:", data)
        }
        ```
    }
    @Column{
        
        ```swift
        // Creating a detached Task
        Task.detached {
        let data = await fetchData()
        print("Fetched data:", data)
        }
        ```
        
    }
}


## 2.3 Async and Await in Swift

The async and await keywords simplify asynchronous programming 
by allowing functions to perform work without blocking other tasks. 
When a function is marked with async, it can be suspended, 
meaning it can "pause" without blocking the thread, using **await**

**async functions:** Define work that doesn’t need to complete immediately.
**await:** Temporarily pauses the current function until the awaited function completes.

```swift
func someAsyncFunction() async {
let data = await fetchData()
print("Data fetched:", data)
}
```

## 2.4 TaskGroup for Concurrent Work

What is a TaskGroup?
TaskGroup allows you to manage collections of tasks that can run concurrently. 
It’s ideal for cases where multiple independent tasks must complete, 
and you want to handle their results as they finish.

Key Benefits

Parallel Execution: Each task in a group can run in parallel if resources allow.
Structured Completion: Waits until all tasks in the group are complete, 
making it easy to gather results.
func fetchAllData() async {
await withTaskGroup(of: String.self) { group in
group.addTask { await fetchDataFromSource1() }
group.addTask { await fetchDataFromSource2() }

for await result in group {
print("Fetched result:", result)
}
}
}

## 2.5 async let

### What is async let?
async let is a shorthand for creating concurrent tasks within an async function. 
It’s particularly useful for lightweight, 
independent tasks and simplifies code by avoiding the need for a TaskGroup.

### Key Benefits

- **Automatic Concurrency**:  
        Swift runs async let tasks concurrently if resources allow.
- **Cleaner Syntax**:  
        No need for an explicit TaskGroup.

```swift
func fetchMultipleData() async {
    async let data1 = fetchDataFromSource1()
    async let data2 = fetchDataFromSource2()

    let (result1, result2) = await (data1, data2)
    print("Fetched data:", result1, result2)
}
```

## 2.6 Actors and MainActor

### What is an Actor?

An Actor is a reference type that isolates its state 
to prevent race conditions in concurrent environments. 
Only one task at a time can access an actor’s data, 
ensuring thread safety and eliminating the need for manual locking mechanisms.

#### Example:

```swift
actor DataManager {
    private var data = [String]()

    func addData(_ newData: String) {
        data.append(newData)
        }
}
```

### MainActor
The @MainActor attribute ensures a function or type runs on the main thread, 
making it essential for UI work. 
Using @MainActor ensures that code interacts with UI elements in a thread-safe manner.

#### Example:

```swift
@MainActor
func updateUI() {
// This code runs on the main thread
}
```

## 2.7 AsyncSequence and AsyncStream

Swift’s AsyncSequence and AsyncStream handle data streams 
that don’t arrive all at once but over time. These tools are ideal for real-time data, 
sensor readings, or user interactions.

### AsyncSequence
An AsyncSequence is like a regular sequence but provides values asynchronously, 
allowing you to process items as they become available.

```swift
struct NumberSequence: AsyncSequence {
    typealias Element = Int

    struct AsyncIterator: AsyncIteratorProtocol {
        private var current = 0
    
        mutating func next() async -> Int? {
            current += 1
            await Task.sleep(1_000_000_000)
            return current <= 5 ? current : nil
        }
    }

    func makeAsyncIterator() -> AsyncIterator {
        return AsyncIterator()
    }
}
```

### AsyncStream

AsyncStream allows you to create an async sequence on demand, 
which is particularly useful for dynamic events like user interactions, 
network requests, or sensor data.

```swift
func createDataStream() -> AsyncStream<String> {
    AsyncStream { continuation in
        continuation.yield("Data 1")
        continuation.yield("Data 2")

        Task {
            await Task.sleep(1_000_000_000)
            continuation.yield("Data 3")
            continuation.finish()
        }
    }
}
```

## 2.8 Task Cancellation

Task cancellation in Swift allows you to stop long-running tasks that are no longer needed,
freeing up resources and improving app responsiveness. 
This is especially useful in cases where tasks may be user-initiated and can be canceled 
if the user navigates away or no longer requires the operation to finish.

### Cancelling a Task

Tasks can be canceled with `Task.cancel()` 
and can check for cancellation with `Task.isCancelled`. 
A task that detects it has been canceled can gracefully stop its work, 
allowing your app to reallocate resources where they are needed.

### Example: Cancelling a Task

In this example, we create a task that processes items in a loop. 
Each iteration checks if the task has been canceled by using `Task.isCancelled`. 
If the task is canceled, it breaks out of the loop and stops processing.

```swift
let task = Task {
    for i in 0..<10 {
        if Task.isCancelled {
            print("Task was canceled. Exiting...")
            break
        }
        print("Processing item \(i)")
        await Task.sleep(500_000_000) // Simulate work with a 0.5 second delay
    }
    print("Task completed")
}

// Simulate canceling the task after a delay
Task {
    await Task.sleep(1_000_000_000) // Wait for 1 second before canceling
    task.cancel()
    print("Task cancellation requested")
}
```

In this example:

The task begins processing items in a loop.

Each iteration checks Task.isCancelled. 
If the task is canceled, it exits the loop early, stopping further work.

After a 1-second delay, we simulate canceling the task by calling task.cancel().
The output shows that once the task is canceled, 
it stops processing additional items and exits gracefully.

### Why Task Cancellation Matters
Task cancellation is important for optimizing performance, 
particularly in user-driven scenarios where tasks may need to be stopped 
if the user navigates away from the current screen or the work is no longer relevant. 
By monitoring Task.isCancelled in long-running tasks, 
you ensure that your app remains responsive and conserves resources.
