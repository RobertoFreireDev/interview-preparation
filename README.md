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

# links/videos

- [WHY IS THE HEAP SO SLOW?](https://www.youtube.com/watch?v=ioJkA7Mw2-U)
- [Inside C#: Stack & Heap, Value Types, Boxing, stackalloc + More](https://www.youtube.com/watch?v=cCsVY0Ixx04)

## Async/await

* Async/await internals (Task vs ValueTask, thread pool)
* Exception handling best practices
* Dependency Injection (lifetimes, scopes, pitfalls)

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