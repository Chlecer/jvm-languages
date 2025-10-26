Aqui tens o texto todo formatado corretamente em **Markdown**, com blocos de código, tabelas e títulos bem organizados:

---

# The Generic Try-Catch: The Practice That Turns Your Java Code Into a Swamp

*Based on the insights from Leonardo Ohashi on exception handling in Java.*

When you start coding in Java, the `try-catch` block feels like the magic bullet—a "safety net" promising to shield your code from unexpected errors. However, the excessive or incorrect use of generic `try-catch` (`catch (Exception e)`) is often a key indicator of weak code with poor maintainability.

This article is a **necessary provocation**: we will show why the rule to "wrap everything in a `try-catch`" is harmful and how adopting **Centralized Exception Management** (especially in Spring) is the key to elegance and robustness in Java applications.

---

## 1. The Problem: Catching Everything is the Same as Catching Nothing

The most destructive error for traceability is catching **everything** generically.

### The Code That Causes Operational Blindness

```java
// The issue: An error could be validation, sending, or database failure.
} catch (Exception e) {  
    System.out.println("An error occurred: " + e.getMessage());
    // PROBLEM: The log is generic. We don't know the origin of the failure.
}
```

### Why Do Engineers Abandon This Practice?

In a complex system, not knowing the exact source of an error is fatal.

#### E-commerce Example:

| Occurrence                  | Category        | Necessary Action                           |
| --------------------------- | --------------- | ------------------------------------------ |
| Empty Cart                  | BUSINESS ERROR  | Inform the customer (return status 400).   |
| Database Offline            | TECHNICAL ERROR | Alert the DevOps team (return status 500). |
| Payment Integration Failure | TECHNICAL ERROR | Attempt to call the payment service again. |

The `catch (Exception e)` treats all these scenarios the same, preventing the automation of fixes and making debugging slow.

---

## 2. The Clean Domain Solution: Separate Business vs. Technical Errors

The key to elegance is **Purpose**.
Your code should throw exceptions that communicate the **intent** of the error.

### The Principle

> Never handle technical exceptions (like `SQLException` or `IOException`) in your Business Layer (your Service).
> Let the Business Layer only throw Domain-specific exceptions.

### The First Step: Specific Exceptions

If an order fails:

Instead of throwing:

```java
throw new Exception("Invalid Order");
```

Throw:

```java
throw new InvalidOrderException("Stock does not have 5 units.");
```

This allows specific catch blocks to react correctly:

```java
} catch (InvalidOrderException e) { 
    // NOW I KNOW IT'S A 400 ERROR!
} catch (ShippingFailureException e) {
    // NOW I KNOW I CAN RETRY!
}
```

---

## 3. The Gold Standard: Centralizing Handling at the "System Edges"

Although Step 2 is better, it still requires repeating try-catch blocks.
The definitive solution, adopted in architectures like **Clean** and **Hexagonal**, is to **Delegate**.

The idea is that your Service layer (the pure business logic) should not know that it's running on a web server or worry about formatting JSON or HTTP status codes.

### The Spring Paradigm: `@ControllerAdvice`

Spring’s `@ControllerAdvice` acts as a **Global Control Point** at the "edges" of the application (where the HTTP request enters).

* **The Service Throws, But Doesn't Handle:**
  Your `OrderService` simply throws Domain exceptions (`InvalidOrderException`) or fails on database calls, allowing the exception to bubble up through the code.

* **The `@ControllerAdvice` Intercepts:**
  When the exception reaches the "edge," the global `ControllerAdvice` catches it and translates the error's meaning into a consistent HTTP Response.

### Example of Error Translation:

| Exception Thrown (In the Service)    | Intercepted by (`@ExceptionHandler`) | Response to Client        |
| ------------------------------------ | ------------------------------------ | ------------------------- |
| `InvalidOrderException` (Business)   | `InvalidOrderException.class`        | 400 Bad Request           |
| `DataAccessException` (Technical/DB) | `DataAccessException.class`          | 500 Internal Server Error |

The gain is monumental:
The **Domain** remains clean, focused only on *what* the application does, while the **Presentation Layer** (`@ControllerAdvice`) only handles *how* the application communicates failures to the outside world.

---

## Conclusion: Write Code That Communicates

The most valuable lesson is that **exceptions are communication tools**.

Spreading generic `try-catch` blocks may seem convenient, but it's a shortcut that harms the **traceability**, **maintenance**, and **security** of a corporate system.

By adopting the practice of **Specific Domain Exceptions** and **Centralizing Handling** at the system edges, we ensure:

✅ **Clear Logs** that immediately inform the true cause of the problem.
✅ **Controlled Flow** to react differently to business and technical errors.
✅ **Consistent and professional HTTP Status Codes.**

More than just avoiding errors, this is the most elegant and predictable way to prepare your code to handle them.

---

### Reference

This article is an analysis and elaboration of the ideas originally presented by Leonardo Ohashi in his Medium article:
**"Como não usar try-catch: Uma breve provocação."**

---

Queres que eu te converta isto também em **formato Markdown (.md)** descarregável?
