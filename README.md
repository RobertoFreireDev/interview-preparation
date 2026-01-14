# 1. Core .NET & C#

## Value vs Reference types

Types:

- Value: int, double, char, bool, struct, enum,...
- Reference: object, string, class, array,...

Memory allocation:

- Value types are allocated either on the stack or the heap depending on where they are created.
- Reference types are always allocated on the heap.

Only the heap is garbage-collected.

Stack has automatic deallocation: when a function or method is called, a dedicated block of memory called a "stack frame" is pushed onto the top of the stack to hold its local variables, parameters, and return address. When the function exits, its entire stack frame is simply "popped off" the stack, and all the memory it used is instantly reclaimed.

```cs
public class Car 
{
    public int Price = 123; 

    public void SetPrice(int price) // argument value type is allocate in the stack 
    {
        Price = price
    }
}
...
public void CreateCar()
{
    int carPrice = 123; // carPrice is allocate in the stack 
    var car = new Car(); // car is allocated the heap
    car.SetPrice(10000) // car.Price is also allocated in the heap
}
```

Abstract representation:

```
┌──────────────────────────────┐
│            STACK             │
├──────────────────────────────┤
│ SetPrice(int price)          │  ← top of stack
│ ───────────────────────────  │
│ price = 10000                │  ← value type
├──────────────────────────────┤
│ CreateCar()                  │
│ ───────────────────────────  │
│ car = 0x01AF34               │  ← reference
│ carPrice = 123               │  ← value type
└──────────────────────────────┘
                │
                │ reference
                ▼
┌──────────────────────────────┐
│             HEAP             │
├──────────────────────────────┤
│ Car object @ 0x01AF34        │
│ ───────────────────────────  │
│ Price = 10000                │  ← field stored in heap
└──────────────────────────────┘
```

links/videos:

- [WHY IS THE HEAP SO SLOW?](https://www.youtube.com/watch?v=ioJkA7Mw2-U)
- [Inside C#: Stack & Heap, Value Types, Boxing, stackalloc + More](https://www.youtube.com/watch?v=cCsVY0Ixx04)

## Flaws in structs

Not everything should be allocated on the stack:

- the stack is limited in size; excessive usage can lead to a stack overflow.
- when a function returns, its stack frame is destroyed, and all local stack data is released.
- structs are value types, so they are copied by value, which can be inefficient for large structs.

Note: struct can be passed by reference using ref, in or out

# Boxing and Unboxing

Boxing is the process of converting a C# value type (like int, char, or struct) to a reference type (object or an interface)

```cs
int x = 10;
object o = x; // boxing

void Log(object value) { }
Log(5); // boxing occurs
```

- Heap allocation → slower than stack allocation
- GC pressure → more frequent garbage collections
- Extra copy → defeats the purpose of value types

Unboxing is the reverse process, where a boxed object is explicitly converted back to its original value type

```cs
int y = (int)o; // unboxing
```

- Extra copy
- Runtime cost (type checking)
- InvalidCastException risk


To avoid boxing/unboxing, use generics

```cs
var list = new ArrayList();
list.Add(10); // boxing
```

```cs
var list = new List<int>();
list.Add(10);
```

## Process

- Process is a program in execution.
- Every process has at least one thread (Main Thread)

## Threads

- Threads are a way to inform the OS that specific parts of a program can be executed concurrently (Multiple threads can point to same function)
- Thread is a lightweight version of a process, easier and faster to create
- Threads points to code/function in memory.

## Multithreading

True Multithreading: The kernel can schedule multiple threads across different processors in a multi-core system.

If one thread blocks (e.g., waiting for I/O), the kernel can switch to another thread.

links/videos:

- [Multithreading in C#](https://medium.com/@yashdesai281/multithreading-in-c-b80332b777b1)

## Context switching

Thread context switching is the CPU's process of pausing one thread, saving its execution state (like registers and program counter), and loading the state of another thread to continue execution, enabling multitasking within a single process.

## Task and thread pool

Async/await internals (Task vs ValueTask, thread pool)

## Blocking and non-blocking thread

Blocking means the thread is waiting, not that the CPU is idle. The CPU will run other threads or processes. 

If the blocked thread is the UI thread, the application becomes unresponsive, because input handling and rendering depend on that thread.

Also, blocking threads can lead to thread starvation in systems that rely on a limited thread pool.

When many threads are blocked waiting for I/O (such as database or network calls), available threads are exhausted, preventing new requests from being processed and significantly reducing application throughput.

Examples:

```cs
// Synchronous (blocking)
var user = _db.Users.First();

// Asynchronous (non-blocking) - recommended
var user = await _db.Users.FirstAsync();
```

```cs
// Synchronous (blocking)
var users = GetUsersAsync().Result;

// Asynchronous (non-blocking) - recommended
var users = await GetUsersAsync();
```

## Thread Safety: 

Threads share the same memory space as the parent process, including its code, data, and resources. However, threads have their own execution context, including a program counter, registers, and stack. Each thread has its own dedicated stack, while all threads within a process share the same heap memory

The shared nature of the heap is why synchronization mechanisms (like lock, Monitor, Mutex, and Semaphore) are necessary in C#. Without them, multiple threads accessing and modifying shared heap data can lead to race conditions and data corruption. 

## Synchronization mechanisms

## Exception handling best practices

##  Dependency Injection (lifetimes, scopes, pitfalls)

---

# 1a. Object-Oriented Programming (OOP)

* Encapsulation, inheritance, polymorphism, abstraction
* Composition over inheritance
* Interface vs abstract class tradeoffs
* Immutability and side-effect control
* Object lifetime and ownership
* Domain modeling with OOP

---

# 2. ASP.NET Core & Web APIs

* Request pipeline & middleware
* Minimal APIs vs MVC Controllers
* REST principles & API design
* API versioning strategies
* Model binding & validation
* Filters (action, resource, exception)
* Authentication & Authorization (JWT, OAuth2, OpenID Connect)
* Rate limiting & throttling
* CORS & security headers

---

# 2a. Algorithms & Data Structures

* Big-O notation and complexity analysis
* Arrays, Lists, Stacks, Queues
* Dictionaries / HashMaps
* Sorting algorithms (quick, merge, heap – concepts)

---

# 2b. Design Patterns

* Creational patterns (Factory, Builder, Singleton)
* Structural patterns (Adapter, Decorator, Facade)
* Behavioral patterns (Strategy, Observer, Command)
* Dependency Injection pattern
* Anti-patterns and overengineering
* Applying patterns pragmatically

---

# 3. Architecture & Design

* SOLID principles (with real-world tradeoffs)
* Clean Architecture / Onion / Hexagonal
* Monolith vs Modular Monolith vs Microservices
* Bounded contexts & domain-driven design basics
* Event-driven architecture
* CQRS (when to use / when not to)
* Saga pattern & eventual consistency
* Idempotency strategies
* API Gateway patterns

---

# 4. Data & Persistence

* Entity Framework Core internals
* Tracking vs No-Tracking queries
* Dapper vs EF Core tradeoffs
* SQL performance basics
* Indexing strategies
* Transactions & isolation levels
* Caching strategies & cache invalidation

---

# 5. Security Foundations

* Cryptography basics (hashing vs encryption)
* Symmetric vs asymmetric keys
* Password storage best practices
* JWT structure & common pitfalls
* Token expiration & refresh tokens
* Secrets management (Key Vault, env vars)
* OWASP Top 10
* Secure API design principles

---

# 6. Performance & Scalability

* Profiling tools (dotnet-counters, dotnet-trace)
* Thread pool starvation
* Async bottlenecks & deadlocks
* Load balancing strategies
* Horizontal vs vertical scaling
* Caching & CDN usage
* Background processing (Hosted Services)
* Messaging systems (RabbitMQ and Service Bus)

---

# 7. Observability & Reliability

* Structured logging (Serilog)
* Logs vs metrics vs traces
* Health checks
* Distributed tracing (OpenTelemetry)
* Alerting strategies
* Resiliency patterns (Polly – retries, circuit breakers)

---

# 8. Testing Strategy

* Unit testing principles (fast, isolated, deterministic)
* Writing testable code
* Mocking vs fakes vs stubs
* Mocking pitfalls
* Testing async code
* Code coverage strategy (what matters, what doesn’t)

---

# 8a. Integration Testing

* Integration testing goals and scope
* ASP.NET Core WebApplicationFactory
* TestContainers (databases, queues)
* In-memory vs real infrastructure

---

# 9. Code Review Excellence

* Goals of code review (quality, learning, risk reduction)
* What to review vs what to ignore
* Reviewing for readability, maintainability, and correctness
* Identifying hidden bugs and edge cases
* Performance and security review basics
* Consistency with architecture and standards
* Giving constructive, actionable feedback
* Handling disagreements and pushing back respectfully
* Scaling code reviews across teams

---

# 11. Interview Readiness

* Prepare system design examples from real projects
* Practice explaining architecture verbally
* Prepare "why we chose X over Y" stories
* Prepare failure & incident stories
* Review common Tech Lead interview questions
* Prepare thoughtful questions for interviewers

---