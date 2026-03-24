## ======= Lecture-5 | LLD: SOLID-1: SRP and OCP =====
Q: What does SOLID principles mean?
ans: 
**SOLID principles** are a set of **5 design principles in Object-Oriented Programming (OOP)** that help you write **clean, maintainable, scalable, and flexible code** — especially important for **system design, LLD, and backend development (Java, Spring Boot, etc.)**.

---

# 🔹 What does SOLID mean?

**S O L I D = 5 Principles**

---

# 1️⃣ S — Single Responsibility Principle (SRP)

👉 **Meaning:**
A class should have **only one reason to change** (i.e., one responsibility).

### ❌ Bad Example:

```java
class User {
    void saveUser() {}
    void sendEmail() {}
}
```

👉 Problem: One class handles DB + Email

### ✅ Good Example:

```java
class UserService {
    void saveUser() {}
}

class EmailService {
    void sendEmail() {}
}
```

✔ Now each class has one responsibility

---

# 2️⃣ O — Open/Closed Principle (OCP)

👉 **Meaning:**
Classes should be **open for extension but closed for modification**

👉 You should **add new features without changing existing code**

### ❌ Bad:

```java
if(type.equals("UPI")) { }
else if(type.equals("CARD")) { }
```

### ✅ Good:

```java
interface Payment {
    void pay();
}

class UPI implements Payment {
    public void pay() {}
}

class Card implements Payment {
    public void pay() {}
}
```

✔ Add new payment method without changing old code

---

# 3️⃣ L — Liskov Substitution Principle (LSP)

👉 **Meaning:**
A subclass should **properly replace its parent class without breaking behavior**

### ❌ Bad:

```java
class Bird {
    void fly() {}
}

class Ostrich extends Bird {
    void fly() { throw new Exception(); } // ❌
}
```

### ✅ Good:

```java
class Bird {}

class FlyingBird extends Bird {
    void fly() {}
}

class Ostrich extends Bird {} // no fly
```

✔ Subclasses behave correctly

---

# 4️⃣ I — Interface Segregation Principle (ISP)

👉 **Meaning:**
Don't force a class to implement methods it doesn't need

### ❌ Bad:

```java
interface Worker {
    void work();
    void eat();
}
```

### ✅ Good:

```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}
```

✔ Small, specific interfaces

---

# 5️⃣ D — Dependency Inversion Principle (DIP)

👉 **Meaning:**
Depend on **abstractions (interfaces)**, not concrete classes

### ❌ Bad:

```java
class OrderService {
    Payment payment = new UPI(); // tightly coupled
}
```

### ✅ Good:

```java
class OrderService {
    Payment payment;

    OrderService(Payment payment) {
        this.payment = payment;
    }
}
```

✔ Loose coupling, easy to change implementation

---

# 🔥 Final Summary (Interview Ready)

| Principle | Meaning                           |
| --------- | --------------------------------- |
| SRP       | One class → one responsibility    |
| OCP       | Extend without modifying          |
| LSP       | Child should behave like parent   |
| ISP       | Small, specific interfaces        |
| DIP       | Depend on interfaces, not classes |

---

# 🚀 Why SOLID is Important (VERY IMPORTANT 🔥)

* Makes code **scalable**
* Improves **maintainability**
* Helps in **system design (LLD)**
* Used heavily in:

  * Spring Boot
  * Microservices
  * Clean Architecture

---

# 🧠 Pro Tip (For YOU)

Since you're targeting **12+ LPA / product companies**,
👉 SOLID is **must-know for interviews**

---

If you want next:
✅ I can give **real project example using SOLID (Spring Boot / MERN)**
✅ Or **interview questions on SOLID with answers**

Just tell me 👍

## =========== 2. SOLID can you give full example of each with proper to understand in more easy way  ====
Perfect 🔥 — now let’s make **SOLID crystal clear with one practical real-world example (Payment System)** so you can **actually understand + explain in interviews confidently**.

---

# 🔷 We’ll Use ONE Example → Payment System

We’ll improve the design step by step using each **SOLID principle**.

---

# 1️⃣ S — Single Responsibility Principle (SRP)

👉 **One class = One responsibility**

## ❌ Bad Design:

```java
class PaymentService {
    void processPayment() {
        // payment logic
    }

    void generateInvoice() {
        // invoice logic
    }

    void sendEmail() {
        // email logic
    }
}
```

❌ Problem:

* One class doing **3 jobs**
* Hard to maintain

---

## ✅ Good Design:

```java
class PaymentService {
    void processPayment() {}
}

class InvoiceService {
    void generateInvoice() {}
}

class EmailService {
    void sendEmail() {}
}
```

✔ Each class = single responsibility

---

# 2️⃣ O — Open/Closed Principle (OCP)

👉 **Open for extension, closed for modification**

---

## ❌ Bad Design:

```java
class PaymentService {
    void pay(String type) {
        if (type.equals("UPI")) {
            System.out.println("UPI Payment");
        } else if (type.equals("CARD")) {
            System.out.println("Card Payment");
        }
    }
}
```

❌ Adding new payment → modify class → bad

---

## ✅ Good Design:

```java
interface Payment {
    void pay();
}

class UPI implements Payment {
    public void pay() {
        System.out.println("UPI Payment");
    }
}

class Card implements Payment {
    public void pay() {
        System.out.println("Card Payment");
    }
}
```

```java
class PaymentService {
    void process(Payment payment) {
        payment.pay();
    }
}
```

✔ Add new payment → no change in existing code

---

# 3️⃣ L — Liskov Substitution Principle (LSP)

👉 **Child should replace parent without breaking behavior**

---

## ❌ Bad Design:

```java
class Bird {
    void fly() {}
}

class Ostrich extends Bird {
    void fly() {
        throw new RuntimeException("Can't fly"); // ❌
    }
}
```

❌ Violates expectation

---

## ✅ Good Design:

```java
class Bird {}

class FlyingBird extends Bird {
    void fly() {}
}

class Ostrich extends Bird {} // no fly
```

✔ Correct hierarchy

---

# 4️⃣ I — Interface Segregation Principle (ISP)

👉 **Don’t force classes to implement unnecessary methods**

---

## ❌ Bad Design:

```java
interface Payment {
    void pay();
    void refund();
}
```

👉 UPI may not support refund → forced 😬

---

## ✅ Good Design:

```java
interface Payment {
    void pay();
}

interface Refundable {
    void refund();
}

class UPI implements Payment {
    public void pay() {}
}

class Card implements Payment, Refundable {
    public void pay() {}
    public void refund() {}
}
```

✔ Only implement what is needed

---

# 5️⃣ D — Dependency Inversion Principle (DIP)

👉 **Depend on abstraction, not concrete class**

---

## ❌ Bad Design:

```java
class PaymentService {
    UPI upi = new UPI(); // tightly coupled
}
```

❌ Cannot change easily

---

## ✅ Good Design:

```java
class PaymentService {
    Payment payment;

    PaymentService(Payment payment) {
        this.payment = payment;
    }

    void process() {
        payment.pay();
    }
}
```

```java
Payment p = new UPI();
PaymentService service = new PaymentService(p);
service.process();
```

✔ Loose coupling
✔ Easy to switch implementations

---

# 🔥 Final Combined Example (ALL SOLID TOGETHER)

```java
interface Payment {
    void pay();
}

class UPI implements Payment {
    public void pay() {
        System.out.println("UPI Payment");
    }
}

class Card implements Payment {
    public void pay() {
        System.out.println("Card Payment");
    }
}

class PaymentService {
    private Payment payment;

    PaymentService(Payment payment) {
        this.payment = payment;
    }

    void processPayment() {
        payment.pay();
    }
}
```

---

# 🔥 How This Uses SOLID

| Principle | Applied How                       |
| --------- | --------------------------------- |
| SRP       | Separate classes                  |
| OCP       | Add new payment without modifying |
| LSP       | All payments behave correctly     |
| ISP       | Small interfaces                  |
| DIP       | Depends on Payment interface      |

---

# 🧠 Interview One-Liner

👉
**“SOLID principles help in designing maintainable and scalable systems by ensuring single responsibility, extensibility, proper inheritance, minimal interfaces, and loose coupling using abstractions.”**

---

# 🚀 Pro Tip (VERY IMPORTANT FOR YOU)

Since you're targeting **product companies (8–12+ LPA)**:

👉 Always:

* Use **interfaces + polymorphism**
* Avoid `if-else` chains
* Write **loosely coupled code**

---

# 🔥 If you want next level

I can give you:

✅ **Spring Boot real project using SOLID (industry-level)**
✅ **Design patterns (Strategy, Factory) based on this example**
✅ **Interview coding questions using SOLID**

Just tell me 👍

## ============ 3.Spring Boot real project using SOLID (industry-level) ===
Perfect 🔥 — now let’s build an **industry-level Spring Boot structure using SOLID principles** (this is exactly how good companies design backend systems).

We’ll design a **Payment Module** (very common in real systems).

---

# 🧩 Project Structure (Industry Style)

```id="kq1h0k"
com.project
 ├── controller
 ├── service
 ├── service.impl
 ├── repository
 ├── model
 ├── dto
 ├── strategy   (for polymorphism)
 ├── factory
 └── config
```

---

# 🔷 1️⃣ MODEL (Entity)

```java id="r6btsj"
@Entity
public class Payment {
    @Id
    private Long id;
    private String type;
    private double amount;
}
```

---

# 🔷 2️⃣ REPOSITORY

```java id="7o4t4s"
@Repository
public interface PaymentRepository extends JpaRepository<Payment, Long> {
}
```

✔ SRP → only DB operations

---

# 🔷 3️⃣ STRATEGY (INTERFACE) → 🔥 SOLID CORE

```java id="t1qj8m"
public interface PaymentStrategy {
    void pay(double amount);
}
```

✔ DIP + OCP
✔ This is the heart of polymorphism

---

# 🔷 4️⃣ STRATEGY IMPLEMENTATIONS

```java id="p1rghd"
@Component
public class UpiPayment implements PaymentStrategy {
    public void pay(double amount) {
        System.out.println("Paid via UPI: " + amount);
    }
}
```

```java id="p0b8tc"
@Component
public class CardPayment implements PaymentStrategy {
    public void pay(double amount) {
        System.out.println("Paid via Card: " + amount);
    }
}
```

✔ LSP → interchangeable
✔ OCP → add new payment easily

---

# 🔷 5️⃣ FACTORY (Select Strategy)

```java id="2f94fj"
@Component
public class PaymentFactory {

    @Autowired
    private List<PaymentStrategy> strategies;

    public PaymentStrategy getPaymentMethod(String type) {
        return strategies.stream()
                .filter(s -> s.getClass().getSimpleName().equalsIgnoreCase(type + "Payment"))
                .findFirst()
                .orElseThrow(() -> new RuntimeException("Invalid payment type"));
    }
}
```

✔ OCP → no if-else
✔ Clean scalable design

---

# 🔷 6️⃣ SERVICE (INTERFACE)

```java id="o78r38"
public interface PaymentService {
    void processPayment(String type, double amount);
}
```

✔ Abstraction → DIP

---

# 🔷 7️⃣ SERVICE IMPLEMENTATION

```java id="t5s9c1"
@Service
public class PaymentServiceImpl implements PaymentService {

    @Autowired
    private PaymentFactory paymentFactory;

    @Autowired
    private PaymentRepository paymentRepository;

    public void processPayment(String type, double amount) {

        PaymentStrategy strategy = paymentFactory.getPaymentMethod(type);
        strategy.pay(amount);

        Payment payment = new Payment();
        payment.setType(type);
        payment.setAmount(amount);

        paymentRepository.save(payment);
    }
}
```

✔ SRP → business logic only
✔ DIP → depends on abstraction
✔ OCP → no modification needed for new types

---

# 🔷 8️⃣ CONTROLLER

```java id="9czx86"
@RestController
@RequestMapping("/payments")
public class PaymentController {

    @Autowired
    private PaymentService paymentService;

    @PostMapping
    public String pay(@RequestParam String type, @RequestParam double amount) {
        paymentService.processPayment(type, amount);
        return "Payment Successful";
    }
}
```

---

# 🔥 How SOLID is Applied (VERY IMPORTANT)

| Principle | Where Used                               |
| --------- | ---------------------------------------- |
| SRP       | Separate Controller, Service, Repository |
| OCP       | Add new payment without modifying code   |
| LSP       | All strategies interchangeable           |
| ISP       | Small interfaces (PaymentStrategy)       |
| DIP       | Service depends on interface, not class  |

---

# 🔥 Adding New Payment (REAL TEST)

👉 Add NetBanking:

```java id="3z51dp"
@Component
public class NetBankingPayment implements PaymentStrategy {
    public void pay(double amount) {
        System.out.println("Paid via NetBanking: " + amount);
    }
}
```

✅ DONE — no change anywhere else
🔥 This is **real OCP**

---

# 🧠 Interview Explanation (VERY POWERFUL)

👉 If interviewer asks:

**“How do you use SOLID in Spring Boot?”**

You say:

> “I use the Strategy pattern with interfaces for extensibility (OCP), separate layers for responsibilities (SRP), ensure interchangeable implementations (LSP), keep interfaces minimal (ISP), and inject dependencies via abstractions (DIP).”

---

# 🚀 Why This is Industry-Level

* No `if-else` logic ❌
* Fully scalable ✅
* Easy to test ✅
* Follows clean architecture ✅
* Works with Spring DI ✅

---

# 🔥 Bonus (For YOU)

Since you're aiming for **product companies (10–12 LPA)**:

👉 This exact pattern is used in:

* Payment gateways
* Notification systems
* Authentication providers

---

# 👉 If you want next level

I can give you:

✅ **Full project (GitHub-style) with folder + config + DB**
✅ **Add JWT auth + microservices design**
✅ **LLD interview question based on this system**

Just tell me 👍

## ============= 4.Full project (GitHub-style) with folder + config + DB ===
Perfect 🔥 — here’s a **complete GitHub-style Spring Boot project** using **SOLID principles + Payment system + DB (MySQL)** exactly like industry projects.

---

# 🧩 Project Name

```
payment-service-solid
```

---

# 📁 Folder Structure (Clean Architecture)

```id="6h0b2t"
payment-service-solid
 ├── src/main/java/com/project/payment
 │    ├── controller
 │    │     └── PaymentController.java
 │    ├── service
 │    │     └── PaymentService.java
 │    ├── service/impl
 │    │     └── PaymentServiceImpl.java
 │    ├── strategy
 │    │     ├── PaymentStrategy.java
 │    │     ├── UpiPayment.java
 │    │     ├── CardPayment.java
 │    │     └── NetBankingPayment.java
 │    ├── factory
 │    │     └── PaymentFactory.java
 │    ├── repository
 │    │     └── PaymentRepository.java
 │    ├── model
 │    │     └── Payment.java
 │    └── PaymentApplication.java
 │
 ├── src/main/resources
 │    ├── application.properties
 │    └── data.sql (optional)
 │
 └── pom.xml
```

---

# 🔷 1️⃣ pom.xml (Dependencies)

```xml id="i6hx6g"
<dependencies>
    <!-- Spring Boot -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- JPA -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- MySQL -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
    </dependency>

    <!-- Lombok (optional) -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </dependency>
</dependencies>
```

---

# 🔷 2️⃣ application.properties

```properties id="y6zyu4"
spring.datasource.url=jdbc:mysql://localhost:3306/payment_db
spring.datasource.username=root
spring.datasource.password=your_password

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

server.port=8080
```

---

# 🔷 3️⃣ Entity (Payment)

```java id="nue7lc"
@Entity
public class Payment {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String type;
    private double amount;

    // getters & setters
}
```

---

# 🔷 4️⃣ Repository

```java id="y2dxwq"
@Repository
public interface PaymentRepository extends JpaRepository<Payment, Long> {
}
```

---

# 🔷 5️⃣ Strategy Interface (SOLID Core)

```java id="2l2xpl"
public interface PaymentStrategy {
    void pay(double amount);
}
```

---

# 🔷 6️⃣ Strategy Implementations

### UPI

```java id="0q2vl3"
@Component
public class UpiPayment implements PaymentStrategy {
    public void pay(double amount) {
        System.out.println("Paid via UPI: " + amount);
    }
}
```

### Card

```java id="wlsq5z"
@Component
public class CardPayment implements PaymentStrategy {
    public void pay(double amount) {
        System.out.println("Paid via Card: " + amount);
    }
}
```

### NetBanking

```java id="k2y8q2"
@Component
public class NetBankingPayment implements PaymentStrategy {
    public void pay(double amount) {
        System.out.println("Paid via NetBanking: " + amount);
    }
}
```

---

# 🔷 7️⃣ Factory (No if-else 🔥)

```java id="ed5ov6"
@Component
public class PaymentFactory {

    @Autowired
    private List<PaymentStrategy> strategies;

    public PaymentStrategy getPaymentMethod(String type) {
        return strategies.stream()
                .filter(s -> s.getClass().getSimpleName()
                .equalsIgnoreCase(type + "Payment"))
                .findFirst()
                .orElseThrow(() -> new RuntimeException("Invalid type"));
    }
}
```

---

# 🔷 8️⃣ Service Interface

```java id="qk7l9v"
public interface PaymentService {
    void processPayment(String type, double amount);
}
```

---

# 🔷 9️⃣ Service Implementation

```java id="8hbdhi"
@Service
public class PaymentServiceImpl implements PaymentService {

    @Autowired
    private PaymentFactory factory;

    @Autowired
    private PaymentRepository repository;

    public void processPayment(String type, double amount) {

        PaymentStrategy strategy = factory.getPaymentMethod(type);
        strategy.pay(amount);

        Payment payment = new Payment();
        payment.setType(type);
        payment.setAmount(amount);

        repository.save(payment);
    }
}
```

---

# 🔷 🔟 Controller

```java id="im1bf7"
@RestController
@RequestMapping("/payments")
public class PaymentController {

    @Autowired
    private PaymentService service;

    @PostMapping
    public String pay(@RequestParam String type,
                      @RequestParam double amount) {

        service.processPayment(type, amount);
        return "Payment Successful";
    }
}
```

---

# 🔷 1️⃣1️⃣ Main Class

```java id="k33kof"
@SpringBootApplication
public class PaymentApplication {
    public static void main(String[] args) {
        SpringApplication.run(PaymentApplication.class, args);
    }
}
```

---

# 🔷 1️⃣2️⃣ MySQL Setup

```sql id="0npvy0"
CREATE DATABASE payment_db;
```

---

# 🔥 API TEST

### POST Request:

```
POST http://localhost:8080/payments?type=Upi&amount=500
```

### Try:

* `Upi`
* `Card`
* `NetBanking`

---

# 🔥 How This Uses SOLID (Final Recap)

| Principle | Applied                               |
| --------- | ------------------------------------- |
| SRP       | Controller, Service, Repo separated   |
| OCP       | Add new payment without changing code |
| LSP       | All strategies interchangeable        |
| ISP       | Small interface (PaymentStrategy)     |
| DIP       | Service depends on abstraction        |

---

# 🚀 Next Level Improvements (Industry)

You can upgrade this project with:

* ✅ DTO + Validation (`@Valid`)
* ✅ Exception Handling (`@ControllerAdvice`)
* ✅ Logging (`Slf4j`)
* ✅ JWT Authentication
* ✅ Swagger API docs
* ✅ Docker + Deployment

---

# 🧠 Resume Tip (VERY IMPORTANT)

👉 You can write:

> “Designed a scalable payment module using Spring Boot following SOLID principles with Strategy pattern, supporting multiple payment methods and clean architecture.”

---

# 🔥 If you want next

I can give you:

✅ Full GitHub README.md (copy-paste ready)
✅ Add JWT + Authentication system
✅ Convert this into Microservices

Just tell me 👍

## ============ 5.Full GitHub README.md (copy-paste ready) ====
# 💳 Payment Service (Spring Boot + SOLID Principles)

A scalable and maintainable **Payment Service** built using **Spring Boot**, following **SOLID principles** and **clean architecture**.
This project demonstrates how to design real-world backend systems using **Strategy Pattern + Factory Pattern + Dependency Injection**.

---

## 🚀 Features

* ✅ Multiple payment methods (UPI, Card, NetBanking)
* ✅ Follows SOLID principles (clean & scalable design)
* ✅ Strategy Pattern for extensibility
* ✅ Factory Pattern to eliminate if-else logic
* ✅ REST APIs using Spring Boot
* ✅ MySQL database integration
* ✅ Layered architecture (Controller → Service → Repository)

---

## 🧠 Tech Stack

* Java 17
* Spring Boot
* Spring Data JPA
* MySQL
* Maven
* Lombok (optional)

---

## 📁 Project Structure

```
com.project.payment
 ├── controller       # REST Controllers
 ├── service          # Service Interfaces
 ├── service.impl     # Business Logic
 ├── strategy         # Payment Strategies (SOLID)
 ├── factory          # Strategy Selector
 ├── repository       # JPA Repositories
 ├── model            # Entities
 └── PaymentApplication.java
```

---

## ⚙️ Setup Instructions

### 1️⃣ Clone the Repository

```
git clone https://github.com/your-username/payment-service-solid.git
cd payment-service-solid
```

---

### 2️⃣ Configure Database

Create MySQL database:

```
CREATE DATABASE payment_db;
```

Update `application.properties`:

```
spring.datasource.url=jdbc:mysql://localhost:3306/payment_db
spring.datasource.username=root
spring.datasource.password=your_password

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

---

### 3️⃣ Run the Application

```
mvn spring-boot:run
```

Server will start at:

```
http://localhost:8080
```

---

## 📡 API Endpoints

### 🔹 Make Payment

**POST** `/payments`

#### Request:

```
http://localhost:8080/payments?type=Upi&amount=500
```

#### Parameters:

| Param  | Type   | Description             |
| ------ | ------ | ----------------------- |
| type   | String | Upi / Card / NetBanking |
| amount | double | Payment amount          |

---

### ✅ Sample Response

```
Payment Successful
```

---

## 🔥 Supported Payment Types

* UPI
* Card
* NetBanking

👉 Easily extendable by adding new strategy class (no code modification required)

---

## 🧩 SOLID Principles Used

| Principle | Implementation                                    |
| --------- | ------------------------------------------------- |
| SRP       | Separate layers (Controller, Service, Repository) |
| OCP       | Add new payment methods without modifying code    |
| LSP       | All strategies interchangeable                    |
| ISP       | Small focused interface (`PaymentStrategy`)       |
| DIP       | Service depends on abstraction                    |

---

## 🏗️ Design Patterns Used

* ✅ Strategy Pattern → Multiple payment behaviors
* ✅ Factory Pattern → Select strategy dynamically
* ✅ Dependency Injection → Loose coupling

---

## 🔄 Adding New Payment Method

1. Create new class in `strategy` package:

```
@Component
public class WalletPayment implements PaymentStrategy {
    public void pay(double amount) {
        System.out.println("Wallet Payment: " + amount);
    }
}
```

2. Done ✅ No changes required anywhere else

---

## 📌 Future Improvements

* 🔐 JWT Authentication & Authorization
* 📄 Swagger API Documentation
* 📊 Logging & Monitoring
* 🧪 Unit & Integration Testing
* 🐳 Docker Deployment
* ☁️ Cloud Deployment (AWS)

---

## 🧠 Learnings

* Applied SOLID principles in real-world project
* Designed scalable backend architecture
* Implemented Strategy & Factory design patterns
* Understood dependency injection deeply

---

## 👨‍💻 Author

**Ritik Sharma**

* 💼 Full Stack Developer (MERN + Spring Boot)
* 🚀 Passionate about scalable system design

---

## ⭐ If you like this project

Give it a ⭐ on GitHub and share it!
