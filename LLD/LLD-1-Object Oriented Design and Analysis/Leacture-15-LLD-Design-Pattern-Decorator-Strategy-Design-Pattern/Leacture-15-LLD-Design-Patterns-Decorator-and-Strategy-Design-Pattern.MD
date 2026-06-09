## =========== 1. Decorator Design Pattern ==========
## 🎁 Decorator Design Pattern — Full Guide (with Real Examples)

---

## 📌 What is Decorator Pattern?

The **Decorator Pattern** is a **structural design pattern** that lets you **add new behavior to objects dynamically** (at runtime) **without modifying their code**.

---

## 🎯 Simple Definition (Interview Ready)

> Decorator pattern wraps an object to add new functionality while keeping the same interface.

---

## 🧠 Why Do We Need It?

Problem:

* You have a class
* You want to add features (logging, encryption, extra behavior)
* But you don’t want:

  * ❌ Modify original class
  * ❌ Create too many subclasses

👉 Solution = **Decorator**

---

## ☕ Real-Life Analogy (Best One)

Think of coffee:

* Base: ☕ Coffee
* Add-ons:

  * Milk 🥛
  * Sugar 🍬
  * Chocolate 🍫

👉 You **wrap coffee with add-ons dynamically**

---

## 🏗️ Structure

### Components:

1. **Component (Interface)**
2. **Concrete Component (Base class)**
3. **Decorator (Abstract wrapper)**
4. **Concrete Decorators (Add features)**

---

## 💻 Full Java Example (Coffee System)

---

### 1️⃣ Component Interface

```java
public interface Coffee {
    String getDescription();
    double cost();
}
```

---

### 2️⃣ Concrete Component

```java
public class SimpleCoffee implements Coffee {

    public String getDescription() {
        return "Simple Coffee";
    }

    public double cost() {
        return 50;
    }
}
```

---

### 3️⃣ Decorator Base Class

```java
public abstract class CoffeeDecorator implements Coffee {

    protected Coffee coffee;

    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }
}
```

---

### 4️⃣ Concrete Decorators

#### Milk Decorator

```java
public class MilkDecorator extends CoffeeDecorator {

    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }

    public String getDescription() {
        return coffee.getDescription() + ", Milk";
    }

    public double cost() {
        return coffee.cost() + 20;
    }
}
```

---

#### Sugar Decorator

```java
public class SugarDecorator extends CoffeeDecorator {

    public SugarDecorator(Coffee coffee) {
        super(coffee);
    }

    public String getDescription() {
        return coffee.getDescription() + ", Sugar";
    }

    public double cost() {
        return coffee.cost() + 10;
    }
}
```

---

### 5️⃣ Usage

```java
public class Main {
    public static void main(String[] args) {

        Coffee coffee = new SimpleCoffee();

        coffee = new MilkDecorator(coffee);
        coffee = new SugarDecorator(coffee);

        System.out.println(coffee.getDescription());
        System.out.println(coffee.cost());
    }
}
```

---

## ✅ Output

```
Simple Coffee, Milk, Sugar
80
```

---

## 🔥 Real-World Example (Backend Systems)

### 💳 Payment System

You can decorate payment with:

* Logging
* Fraud check
* Encryption

```java
payment = new LoggingDecorator(payment);
payment = new FraudCheckDecorator(payment);
payment = new EncryptionDecorator(payment);
```

👉 Base payment remains unchanged ✅

---

## ⚡ Another Real Example

### Spring Boot Filters / Middleware

* Request logging
* Authentication
* Rate limiting

👉 These act like **decorators on request flow**

---

## 🧠 Key Characteristics

✔ Same interface maintained
✔ Add behavior dynamically
✔ Follows **Open/Closed Principle**
✔ Avoids subclass explosion

---

## ⚖️ Advantages

✔ Flexible (add/remove features at runtime)
✔ No modification of existing code
✔ Composable behavior
✔ Clean separation of concerns

---

## ❌ Disadvantages

❌ Many small classes
❌ Debugging can be complex
❌ Hard to understand chain

---

## 📌 When to Use

✔ Add features dynamically
✔ Multiple combinations of features
✔ Avoid subclass explosion
✔ Logging, caching, security layers

---

## 🚫 When NOT to Use

❌ Simple system
❌ Fixed behavior
❌ When readability is more important

---

## 🆚 Decorator vs Inheritance

| Feature          | Decorator | Inheritance  |
| ---------------- | --------- | ------------ |
| Flexibility      | Runtime   | Compile-time |
| Extensibility    | High      | Limited      |
| Code duplication | Low       | High         |

---

## 🆚 Decorator vs Adapter

| Feature   | Decorator    | Adapter           |
| --------- | ------------ | ----------------- |
| Purpose   | Add behavior | Convert interface |
| Interface | Same         | Different         |
| Focus     | Enhancement  | Compatibility     |

---

## 🎯 Interview One-Liner

> Decorator dynamically adds behavior to an object without modifying its original structure.

---

## 🔥 Pro Tip (VERY IMPORTANT)

👉 In real systems:

* Logging
* Security
* Metrics
* Caching

👉 All are implemented using **Decorator-like patterns**

---

## 🚀 Want Next Level?

I can show you:

* 🔥 **Decorator in Spring Boot (Filters/Interceptors)**
* 🧠 **Real Payment Decorator (logging + fraud + retry)**
* ⚡ **Comparison: Decorator vs Proxy vs Facade (tricky interview)**

Just tell me 👍


## ============ 2. Real Payment Decorator (logging + fraud + retry) ===
Great — now you’re entering **real-world design level thinking** 🔥
Let’s build a **production-level Payment System using the Decorator Pattern** with:

✔ Logging
✔ Fraud Check
✔ Retry Mechanism
✔ Clean, scalable architecture

---

# 🚀 1. Core Idea (Decorator Pattern in Real Life)

Instead of writing this ❌

```java
processPayment() {
   log();
   fraudCheck();
   retry();
   actualPayment();
}
```

We do this ✅ (flexible & scalable):

```java
PaymentService payment = new RetryDecorator(
                            new FraudCheckDecorator(
                                new LoggingDecorator(
                                    new StripePaymentService()
                                )
                            )
                        );

payment.pay(request);
```

👉 Each responsibility is **separate + reusable**

---

# 🧱 2. Step-by-Step Implementation

---

## 🔹 Step 1: Payment Interface

```java
public interface PaymentService {
    void pay(PaymentRequest request);
}
```

---

## 🔹 Step 2: Base Payment (Actual Gateway)

```java
public class StripePaymentService implements PaymentService {

    @Override
    public void pay(PaymentRequest request) {
        System.out.println("Processing payment via STRIPE: " + request.getAmount());
        
        // simulate random failure
        if (Math.random() < 0.5) {
            throw new RuntimeException("Payment failed in Stripe");
        }

        System.out.println("Payment SUCCESS via STRIPE");
    }
}
```

---

## 🔹 Step 3: Abstract Decorator (Important 🔥)

```java
public abstract class PaymentDecorator implements PaymentService {

    protected PaymentService paymentService;

    public PaymentDecorator(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

👉 This is the **core of Decorator Pattern**

---

# 🔥 3. Real Decorators

---

## 🧾 3.1 Logging Decorator

```java
public class LoggingDecorator extends PaymentDecorator {

    public LoggingDecorator(PaymentService paymentService) {
        super(paymentService);
    }

    @Override
    public void pay(PaymentRequest request) {
        System.out.println("LOG: Payment started for user " + request.getUserId());

        paymentService.pay(request);

        System.out.println("LOG: Payment finished");
    }
}
```

---

## 🛡️ 3.2 Fraud Check Decorator

```java
public class FraudCheckDecorator extends PaymentDecorator {

    public FraudCheckDecorator(PaymentService paymentService) {
        super(paymentService);
    }

    @Override
    public void pay(PaymentRequest request) {

        if (request.getAmount() > 10000) {
            throw new RuntimeException("Fraud detected: Amount too high");
        }

        System.out.println("Fraud check passed");

        paymentService.pay(request);
    }
}
```

---

## 🔁 3.3 Retry Decorator (MOST IMPORTANT 🔥)

```java
public class RetryDecorator extends PaymentDecorator {

    private int maxRetries = 3;

    public RetryDecorator(PaymentService paymentService) {
        super(paymentService);
    }

    @Override
    public void pay(PaymentRequest request) {
        int attempts = 0;

        while (attempts < maxRetries) {
            try {
                paymentService.pay(request);
                return;
            } catch (Exception e) {
                attempts++;
                System.out.println("Retry attempt: " + attempts);

                if (attempts == maxRetries) {
                    throw new RuntimeException("Payment failed after retries");
                }
            }
        }
    }
}
```

---

# 📦 4. PaymentRequest DTO

```java
public class PaymentRequest {
    private String userId;
    private double amount;

    public PaymentRequest(String userId, double amount) {
        this.userId = userId;
        this.amount = amount;
    }

    public String getUserId() { return userId; }
    public double getAmount() { return amount; }
}
```

---

# 🧪 5. Main Execution

```java
public class Main {
    public static void main(String[] args) {

        PaymentService paymentService =
                new RetryDecorator(
                    new FraudCheckDecorator(
                        new LoggingDecorator(
                            new StripePaymentService()
                        )
                    )
                );

        PaymentRequest request = new PaymentRequest("user123", 5000);

        paymentService.pay(request);
    }
}
```

---

# ⚡ 6. Execution Flow (VERY IMPORTANT)

```
RetryDecorator
   → FraudCheckDecorator
       → LoggingDecorator
           → StripePaymentService
```

### Actual Flow:

1. Retry starts
2. Fraud check runs
3. Logging starts
4. Payment executes
5. Logging ends
6. Retry handles failure if any

---

# 🧠 7. Why This is Powerful (REAL INTERVIEW POINT)

### ❌ Without Decorator

* Big messy class
* Hard to extend
* Violates SOLID

### ✅ With Decorator

* Open/Closed Principle ✔
* Reusable components ✔
* Plug & Play features ✔
* Clean production code ✔

---

# ⚠️ 8. Real Industry Usage

Used in:

* Spring Security filters
* Java I/O Streams (`BufferedReader`)
* Payment systems (like above)
* Logging frameworks
* Middleware pipelines

---

# 🔥 9. When to Use

Use Decorator when:

✔ You need **dynamic features**
✔ You want to **avoid subclass explosion**
✔ You want **composable logic (stacking behaviors)**

---

# ❌ When NOT to Use

❌ Too many decorators → hard to debug
❌ Simple logic → overengineering
❌ If order of execution is critical and confusing

---

# 🎯 10. Pro-Level Insight (VERY IMPORTANT)

👉 This architecture is **exactly how real systems like Amazon/Stripe scale features**

Instead of rewriting code, they:

* Add new decorator (e.g., CashbackDecorator 💰)
* Plug it in
* Done ✅

---

# 🚀 If You Want Next Level

I can show you:

👉 Spring Boot version using `@Component` + DI
👉 Dynamic decorator chain from DB/config
👉 Combine with **Factory + Strategy + Adapter**

Just tell me 🔥

## ========== 3. Combine with **Factory + Strategy + Adapter** ======
Perfect 🔥 — now we’ll build a **real-world scalable architecture** by combining:

👉 **Decorator + Factory + Strategy + Adapter**

This is **exactly how production systems (Amazon / Razorpay / Stripe)** are designed.

---

# 🧠 1. Big Picture Architecture

```
Controller
   ↓
PaymentFacade
   ↓
PaymentFactory  → (chooses gateway)
   ↓
Adapter (Stripe / Razorpay / PayPal)
   ↓
Strategy (UPI / CARD / WALLET logic)
   ↓
Decorator (Logging + Fraud + Retry)
```

---

# 🚀 2. Step-by-Step Implementation

---

# 🔹 2.1 DTO

```java
public class PaymentRequest {
    private String userId;
    private double amount;
    private String paymentMethod; // CARD / UPI / WALLET
    private String gateway;       // STRIPE / RAZORPAY

    public PaymentRequest(String userId, double amount, String paymentMethod, String gateway) {
        this.userId = userId;
        this.amount = amount;
        this.paymentMethod = paymentMethod;
        this.gateway = gateway;
    }

    public String getUserId() { return userId; }
    public double getAmount() { return amount; }
    public String getPaymentMethod() { return paymentMethod; }
    public String getGateway() { return gateway; }
}
```

---

# 🔹 2.2 Strategy Pattern (Payment Method Logic)

👉 Different payment behaviors

---

## Strategy Interface

```java
public interface PaymentStrategy {
    void execute(PaymentRequest request);
}
```

---

## CARD Strategy

```java
public class CardPaymentStrategy implements PaymentStrategy {
    public void execute(PaymentRequest request) {
        System.out.println("Processing CARD payment");
    }
}
```

---

## UPI Strategy

```java
public class UpiPaymentStrategy implements PaymentStrategy {
    public void execute(PaymentRequest request) {
        System.out.println("Processing UPI payment");
    }
}
```

---

# 🔹 2.3 Adapter Pattern (External Gateway)

👉 Each gateway has different APIs → we normalize them

---

## Common Interface

```java
public interface PaymentGateway {
    void process(PaymentRequest request);
}
```

---

## External APIs (simulated)

```java
public class StripeAPI {
    public void makePayment(double amount) {
        System.out.println("Stripe API called with amount: " + amount);
    }
}

public class RazorpayAPI {
    public void payNow(double amount) {
        System.out.println("Razorpay API called with amount: " + amount);
    }
}
```

---

## Adapter for Stripe

```java
public class StripeAdapter implements PaymentGateway {

    private StripeAPI stripe = new StripeAPI();

    public void process(PaymentRequest request) {
        stripe.makePayment(request.getAmount());
    }
}
```

---

## Adapter for Razorpay

```java
public class RazorpayAdapter implements PaymentGateway {

    private RazorpayAPI razorpay = new RazorpayAPI();

    public void process(PaymentRequest request) {
        razorpay.payNow(request.getAmount());
    }
}
```

---

# 🔹 2.4 Core Payment Service

```java
public class PaymentService {

    private PaymentGateway gateway;
    private PaymentStrategy strategy;

    public PaymentService(PaymentGateway gateway, PaymentStrategy strategy) {
        this.gateway = gateway;
        this.strategy = strategy;
    }

    public void pay(PaymentRequest request) {
        strategy.execute(request);   // Step 1: payment type logic
        gateway.process(request);    // Step 2: external API
    }
}
```

---

# 🔥 2.5 Decorator Layer (Cross-Cutting Concerns)

---

## Base Decorator

```java
public abstract class PaymentDecorator extends PaymentService {

    protected PaymentService paymentService;

    public PaymentDecorator(PaymentService paymentService) {
        super(null, null);
        this.paymentService = paymentService;
    }
}
```

---

## Logging

```java
public class LoggingDecorator extends PaymentDecorator {

    public LoggingDecorator(PaymentService paymentService) {
        super(paymentService);
    }

    @Override
    public void pay(PaymentRequest request) {
        System.out.println("LOG: Start");

        paymentService.pay(request);

        System.out.println("LOG: End");
    }
}
```

---

## Fraud Check

```java
public class FraudDecorator extends PaymentDecorator {

    public FraudDecorator(PaymentService paymentService) {
        super(paymentService);
    }

    @Override
    public void pay(PaymentRequest request) {

        if (request.getAmount() > 10000) {
            throw new RuntimeException("Fraud detected");
        }

        paymentService.pay(request);
    }
}
```

---

## Retry

```java
public class RetryDecorator extends PaymentDecorator {

    public RetryDecorator(PaymentService paymentService) {
        super(paymentService);
    }

    @Override
    public void pay(PaymentRequest request) {
        int retry = 0;

        while (retry < 3) {
            try {
                paymentService.pay(request);
                return;
            } catch (Exception e) {
                retry++;
                System.out.println("Retry: " + retry);
            }
        }

        throw new RuntimeException("Payment failed");
    }
}
```

---

# 🏭 2.6 Factory Pattern (MOST IMPORTANT 🔥)

👉 Decides everything dynamically

```java
public class PaymentFactory {

    public static PaymentService build(PaymentRequest request) {

        // 1. Choose Strategy
        PaymentStrategy strategy;
        switch (request.getPaymentMethod()) {
            case "UPI":
                strategy = new UpiPaymentStrategy();
                break;
            case "CARD":
            default:
                strategy = new CardPaymentStrategy();
        }

        // 2. Choose Gateway (Adapter)
        PaymentGateway gateway;
        switch (request.getGateway()) {
            case "RAZORPAY":
                gateway = new RazorpayAdapter();
                break;
            case "STRIPE":
            default:
                gateway = new StripeAdapter();
        }

        // 3. Core Service
        PaymentService service = new PaymentService(gateway, strategy);

        // 4. Add Decorators (dynamic layering)
        service = new LoggingDecorator(service);
        service = new FraudDecorator(service);
        service = new RetryDecorator(service);

        return service;
    }
}
```

---

# 🎭 2.7 Facade (Clean Entry Point)

```java
public class PaymentFacade {

    public void processPayment(PaymentRequest request) {
        PaymentService service = PaymentFactory.build(request);
        service.pay(request);
    }
}
```

---

# 🧪 3. Execution

```java
public class Main {
    public static void main(String[] args) {

        PaymentRequest request =
                new PaymentRequest("user1", 5000, "UPI", "RAZORPAY");

        PaymentFacade facade = new PaymentFacade();
        facade.processPayment(request);
    }
}
```

---

# ⚡ 4. Flow Summary

```
Facade
  ↓
Factory
  ↓
Strategy (UPI)
  ↓
Adapter (Razorpay)
  ↓
Decorators (Retry → Fraud → Logging)
```

---

# 🔥 5. Why This is PRO-Level Architecture

### ✅ EXTREMELY FLEXIBLE

* Add new gateway → just Adapter
* Add new payment type → Strategy
* Add feature → Decorator

---

### ✅ ZERO CODE CHANGE (OCP 🔥)

You only **extend**, never modify

---

### ✅ REAL INDUSTRY MATCH

Used in:

* Payment systems
* Microservices
* Middleware pipelines
* Spring Boot enterprise apps

---

# 🎯 6. Interview Killer Line

👉
“Factory decides objects, Strategy decides behavior, Adapter integrates external systems, and Decorator adds dynamic features.”

---

# 🚀 If You Want NEXT LEVEL

I can take this to:

👉 Spring Boot version with `@Service`, `@Component`
👉 Config-driven (DB आधारित dynamic decorators 🔥)
👉 Microservices + Kafka integration
👉 Production folder structure (LLD ready)

Just tell me 😎


### ========== 5. Strategy Design Pattern ======
Great choice 👍 — **Strategy Design Pattern** is one of the most important patterns for interviews + real-world systems.

I’ll explain it in a **clear + practical + production-level way** 🔥

---

# 🧠 1. What is Strategy Pattern?

👉 Strategy Pattern allows you to **change behavior at runtime** without modifying existing code.

### 📌 Simple Definition:

> “Define a family of algorithms, put each in a separate class, and make them interchangeable.”

---

# 🎯 2. Real-Life Example

Think of **payment methods**:

* Credit Card 💳
* UPI 📱
* Wallet 💰

👉 All do *payment*, but logic is different.

Instead of:

```java
if(type == "CARD") { ... }
else if(type == "UPI") { ... }
```

We use Strategy Pattern ✅

---

# 🧱 3. Structure

```
Client
  ↓
Context (PaymentService)
  ↓
Strategy Interface
  ↓
Concrete Strategies (Card, UPI, Wallet)
```

---

# 🚀 4. Step-by-Step Implementation

---

## 🔹 Step 1: Strategy Interface

```java
public interface PaymentStrategy {
    void pay(double amount);
}
```

---

## 🔹 Step 2: Concrete Strategies

---

### 💳 Card Payment

```java
public class CardPayment implements PaymentStrategy {

    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using CARD");
    }
}
```

---

### 📱 UPI Payment

```java
public class UpiPayment implements PaymentStrategy {

    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using UPI");
    }
}
```

---

### 💰 Wallet Payment

```java
public class WalletPayment implements PaymentStrategy {

    @Override
    public void pay(double amount) {
        System.out.println("Paid " + amount + " using WALLET");
    }
}
```

---

## 🔹 Step 3: Context Class (IMPORTANT 🔥)

```java
public class PaymentService {

    private PaymentStrategy strategy;

    // constructor injection
    public PaymentService(PaymentStrategy strategy) {
        this.strategy = strategy;
    }

    public void processPayment(double amount) {
        strategy.pay(amount);
    }
}
```

---

## 🔹 Step 4: Usage

```java
public class Main {
    public static void main(String[] args) {

        PaymentStrategy strategy = new UpiPayment();

        PaymentService service = new PaymentService(strategy);
        service.processPayment(1000);
    }
}
```

---

# ⚡ 5. Dynamic Strategy Change (REAL POWER 🔥)

```java
PaymentService service = new PaymentService(new CardPayment());
service.processPayment(500);

// change at runtime
service = new PaymentService(new UpiPayment());
service.processPayment(800);
```

---

# 🧠 6. Why Not Use IF-ELSE?

### ❌ Problem:

```java
if(method.equals("CARD")) { ... }
else if(method.equals("UPI")) { ... }
```

* Not scalable ❌
* Violates Open/Closed Principle ❌
* Hard to maintain ❌

---

### ✅ Strategy Solution:

* Add new class → done ✔
* No modification ✔
* Clean code ✔

---

# 🔥 7. Real Industry Examples

Used in:

✔ Payment systems
✔ Sorting algorithms (`Collections.sort()` with Comparator)
✔ Spring Security authentication
✔ Discount calculation engines
✔ Route planning (Google Maps 🚗)

---

# 🆚 8. Strategy vs Similar Patterns

| Pattern   | Purpose            |
| --------- | ------------------ |
| Strategy  | Change behavior    |
| Factory   | Create objects     |
| Adapter   | Convert interface  |
| Decorator | Add extra features |

---

# ⚖️ 9. Advantages

✔ Removes if-else chains
✔ Easy to extend
✔ Runtime flexibility
✔ Clean & testable code

---

# ❌ 10. Disadvantages

❌ Too many classes
❌ Client must know which strategy to use
❌ Slight complexity for small cases

---

# 🎯 11. When to Use

Use Strategy when:

✔ You have multiple ways to perform a task
✔ You want runtime switching
✔ You want clean scalable code

---

# 🚫 12. When NOT to Use

❌ Only 1 or 2 simple conditions
❌ No need for runtime change
❌ Overengineering small logic

---

# 🔥 13. Interview Killer Statement

👉
“Strategy pattern replaces conditional logic with polymorphism, allowing behavior to change dynamically at runtime.”

---

# 🚀 14. Next Level (Highly Recommended)

If you want real mastery, next steps:

👉 Strategy + Factory (auto selection)
👉 Strategy + Spring Boot (`@Qualifier`, `@Service`)
👉 Strategy from DB/config (dynamic runtime 🔥)
👉 Combine with Decorator (you already saw 😉)

---

If you want, I can show:

👉 **Spring Boot Strategy (real production code)**
👉 OR **Dynamic strategy selection using Map (VERY IMPORTANT for interviews)**

## ======== 6. Spring Boot Strategy (real production code) ===
Perfect 🔥 — now let’s implement **Strategy Pattern in real Spring Boot production style** (the way it’s actually used in companies).

We’ll build a **clean, scalable, dependency-injected strategy system** — no `if-else`, no `switch` ❌

---

# 🧠 1. Real Production Idea

👉 Instead of manually doing:

```java
if (method.equals("UPI")) { ... }
```

👉 Spring Boot will **automatically inject all strategies** and we’ll select dynamically using a **Map** ✅

---

# 🏗️ 2. Architecture

```
Controller
   ↓
Service (Context)
   ↓
Map<String, PaymentStrategy>  ← Spring injects all strategies
   ↓
Concrete Strategies (@Service)
```

---

# 🚀 3. Full Working Code

---

## 🔹 3.1 Strategy Interface

```java
public interface PaymentStrategy {
    String getType(); // important for mapping
    void pay(PaymentRequest request);
}
```

---

## 🔹 3.2 DTO

```java
public class PaymentRequest {
    private String userId;
    private double amount;
    private String method; // UPI / CARD / WALLET

    // getters & setters
}
```

---

# 🔥 3.3 Concrete Strategies

---

## 💳 Card Strategy

```java
import org.springframework.stereotype.Service;

@Service
public class CardPaymentStrategy implements PaymentStrategy {

    @Override
    public String getType() {
        return "CARD";
    }

    @Override
    public void pay(PaymentRequest request) {
        System.out.println("Processing CARD payment for: " + request.getAmount());
    }
}
```

---

## 📱 UPI Strategy

```java
import org.springframework.stereotype.Service;

@Service
public class UpiPaymentStrategy implements PaymentStrategy {

    @Override
    public String getType() {
        return "UPI";
    }

    @Override
    public void pay(PaymentRequest request) {
        System.out.println("Processing UPI payment for: " + request.getAmount());
    }
}
```

---

## 💰 Wallet Strategy

```java
import org.springframework.stereotype.Service;

@Service
public class WalletPaymentStrategy implements PaymentStrategy {

    @Override
    public String getType() {
        return "WALLET";
    }

    @Override
    public void pay(PaymentRequest request) {
        System.out.println("Processing WALLET payment");
    }
}
```

---

# 🧠 3.4 Context (IMPORTANT 🔥)

👉 This is the **real production trick**

```java
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

@Service
public class PaymentService {

    private final Map<String, PaymentStrategy> strategyMap;

    // Spring injects ALL strategies here automatically
    public PaymentService(List<PaymentStrategy> strategies) {

        this.strategyMap = strategies.stream()
                .collect(Collectors.toMap(
                        PaymentStrategy::getType,
                        strategy -> strategy
                ));
    }

    public void processPayment(PaymentRequest request) {

        PaymentStrategy strategy = strategyMap.get(request.getMethod());

        if (strategy == null) {
            throw new RuntimeException("Invalid payment method");
        }

        strategy.pay(request);
    }
}
```

---

# 🔥 WHY THIS IS POWERFUL

👉 Spring automatically does this:

```java
List<PaymentStrategy> strategies = [
   CardPaymentStrategy,
   UpiPaymentStrategy,
   WalletPaymentStrategy
]
```

Then we convert it into:

```java
Map = {
   "CARD" -> CardPaymentStrategy,
   "UPI" -> UpiPaymentStrategy,
   "WALLET" -> WalletPaymentStrategy
}
```

---

# 🎮 3.5 Controller

```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/payment")
public class PaymentController {

    private final PaymentService paymentService;

    public PaymentController(PaymentService paymentService) {
        this.paymentService = paymentService;
    }

    @PostMapping
    public String pay(@RequestBody PaymentRequest request) {
        paymentService.processPayment(request);
        return "Payment processed successfully";
    }
}
```

---

# 🧪 4. API Example

### Request:

```json
{
  "userId": "user123",
  "amount": 5000,
  "method": "UPI"
}
```

---

### Flow:

```
Controller
   ↓
PaymentService
   ↓
Map → "UPI"
   ↓
UpiPaymentStrategy
```

---

# ⚡ 5. Add New Strategy (ZERO CODE CHANGE 🔥)

👉 Add new class:

```java
@Service
public class NetBankingStrategy implements PaymentStrategy {

    public String getType() {
        return "NET_BANKING";
    }

    public void pay(PaymentRequest request) {
        System.out.println("Processing NetBanking");
    }
}
```

✅ Done — no change anywhere else

---

# 🆚 6. Traditional vs Spring Strategy

| Feature     | Traditional   | Spring Boot  |
| ----------- | ------------- | ------------ |
| Selection   | if-else ❌     | Map lookup ✅ |
| Scalability | Poor ❌        | Excellent ✅  |
| Extension   | Modify code ❌ | Add class ✅  |
| Clean code  | Medium        | High         |

---

# 🔥 7. Real Industry Usage

Used in:

✔ Payment systems
✔ Notification systems (Email/SMS/Push)
✔ Authentication (OAuth, JWT, LDAP)
✔ Discount engines
✔ File processing (CSV, JSON, XML)

---

# 🎯 8. Interview Killer Line

👉
“In Spring Boot, Strategy Pattern is implemented using dependency injection of multiple beans and dynamic resolution via Map, eliminating conditionals completely.”

---

# 🚀 9. Next Level (Highly Recommended)

I can upgrade this to:

👉 Strategy + Factory (auto selection + fallback)
👉 Strategy + Decorator (add logging/retry 🔥)
👉 Strategy from DB (dynamic runtime config)
👉 Full microservice architecture

Just tell me 😎
