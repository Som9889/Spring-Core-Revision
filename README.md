# Spring Core — Notes
---
## A. What is Spring Framework?

Spring is a **lightweight, modular, open-source framework** for building Java enterprise applications.

### Key Features
- Loose coupling  
- Dependency Injection  
- IOC Container  
- AOP support  
- Transaction management  
- Easy integration with Hibernate, JPA, Kafka, etc.  
- Modular architecture  

## B. Inversion of Control (IoC)

**IoC means Spring creates and manages objects instead of the programmer.**

### Traditional Java
```java
Car car = new Car(new Engine());
```

### Spring IoC
```
Car car = context.getBean(Car.class);
```
## C. Dependency Injection (DI)

Spring automatically injects dependencies into classes.

### Benefits

- Loose coupling
- Easier testing
- Better maintainability
- Types of DI
- Constructor Injection ✔ Recommended
- Setter Injection
- Field Injection ✘ Avoid

## D. What is a Bean?

A bean is an object managed by Spring.

#### Declaring Beans
##### Using @Component
```
@Component
class Engine {}
```
##### Using @Bean
```
@Configuration
class AppConfig {

    @Bean
    Engine engine() {
        return new Engine();
    }
}
```
## E. Spring Containers
### BeanFactory
- Basic container
- Lazy initialization

### ApplicationContext
- Advanced container
- Eager initialization
- AOP support
- Event handling

## F. Bean Scopes

| Scope        | Description                                               |
|--------------|-----------------------------------------------------------|
| singleton    | One instance for the entire application.                  |
| prototype    | A new instance is created every time it is requested.     |
| request      | One instance per HTTP request.                            |
| session      | One instance per user session.                            |
| application  | One instance per servlet context (shared across the app). |

##### Example
```
@Scope("prototype")
@Component
class Car {}
```
## G. @Autowired
Injects dependencies automatically.
### Order
1. Constructor
2. Setter
3. Field
``` 
@Autowired
private Engine engine;
```
## H. @Qualifier
Used when multiple beans of same type exist.
```
@Autowired
@Qualifier("dieselEngine")
Engine engine;
```
## I. @Primary

Marks a bean as the default.
```
@Primary
@Bean
Engine petrolEngine() {
    return new PetrolEngine();
}
```

## J. @Configuration & @Bean

Used for manual bean creation.
```
@Configuration
class AppConfig {

    @Bean
    public Engine engine() {
        return new Engine();
    }
}
```
## K. Bean Lifecycle

- Instantiate
- Populate dependencies
- @PostConstruct
- Bean ready
- @PreDestroy
- Destroy

## L. @PostConstruct & @PreDestroy

```
@PostConstruct
void init() {}

@PreDestroy
void destroy() {}
```
## M. Component Scanning

Spring auto-detects beans using annotations like:

- @Component
- @Service
- @Repository
- @Controller
- @ComponentScan("com.myapp")

## N. Spring Stereotype Annotations & Their Layers

| Annotation        | Layer / Purpose                     | Description |
|-------------------|--------------------------------------|-------------|
| `@Component`      | **Generic Component**                | A general-purpose stereotype; indicates that the class is a Spring-managed bean. Parent stereotype for others. |
| `@Service`        | **Business Logic Layer**             | Marks a class that holds business logic. Helps with clarity and may enable specific features such as AOP. |
| `@Repository`     | **Data Access Layer (DAO)**          | Indicates a class that interacts with the database. Enables exception translation into Spring’s `DataAccessException` hierarchy. |
| `@Controller`     | **MVC Controller (Web Layer)**       | Handles web requests in Spring MVC; returns views (e.g., Thymeleaf). |
| `@RestController` | **REST API Controller (Web Layer)**  | A specialized controller that returns JSON/XML directly. Equivalent to `@Controller + @ResponseBody`. |

## O. Injecting Properties (@Value)
#### application.properties
```
app.name=SpringApp
```
##### Usage
```
@Value("${app.name}")
String appName;
```
## P. Environment Interface

Used to read properties dynamically.
```
@Autowired
Environment env;

String value = env.getProperty("app.name");
```

## Q. @Profile (Environment-specific beans)
Spring Profiles allow you to switch configurations automatically based on environment, making your application flexible, safe, and maintainable.
(Spring Profile is used for segrating the environment env. like Developer., Qa., Security,Production)
```
@Profile("dev")
@Component
class DevConfig {}
```

### Activate profile:
```
spring.profiles.active=dev
```
## R. AOP (Aspect Oriented Programming)

Used for cross-cutting concerns like:

- Logging
- Security
- Transactions
- Concepts
- Aspect
- Advice
- Pointcut
- JoinPoint
- Weaving
- Example
- @Aspect
- @Component
```
class LogAspect {

    @Before("execution(* com.app.service.*.*(..))")
    void log() {
        System.out.println("Method called");
    }
}
```


## Spring Core Interview Questions

### 1. What is Spring Framework?

Spring is a lightweight, open-source, and modular Java framework used to build enterprise applications.
It provides:

- Loose coupling using IoC
- Dependency Injection
- AOP
- Transaction management
- Easy integration with ORM tools
- Simplified application development
Spring solves Java application complexity by providing a structured way to manage - objects and their dependencies.

### 2. What is Inversion of Control (IoC)?

IoC is a principle where the control of object creation is shifted from programmer → Spring container.

#### Instead of:
```
Car car = new Car(new Engine());

```
#### We do:
```
Car car = context.getBean(Car.class);  //its also not requried in springboot
                                      //springboot provide auto configuration
```
### Benefits

- Reduces tight coupling
- Improves testability
- Promotes cleaner architecture

Spring implements IoC using the IoC Container which manages all beans.

## 3. What is Dependency Injection (DI)? Explain types.

DI is a pattern where Spring injects required dependencies into an object instead of the object creating them.

### Types of DI
#### 1. Constructor Injection (Recommended)
```
public Car(Engine engine) {}

```
- Ensures immutable dependencies
- Detects circular dependencies early

#### 2. Setter Injection
 ```
setEngine(Engine engine) { }
```

- Optional dependencies
- Flexible

#### 3. Field Injection (Not Recommended)
```
@Autowired
Engine engine;
```

- Hard to test
- Not recommended by Spring team

## 4. What is a Spring Bean?

A bean is simply an object created, managed, and destroyed by the Spring IoC container.

#### Beans can be created using:

- `@Component`, `@Service`, `@Repository`
- `@Bean` inside a `@Configuration` class
- XML configuration (legacy)

## 5. What is the difference between @Component and @Bean?
### @Component

- Used on classes
- Automatically detected using component scanning
- Suitable for simple beans

### @Bean

Used inside `@Configuration` class
Gives full control over bean creation
Suitable for third-party libraries

✔ If you own the class → use `@Component`
✔ If you don’t own the class → use `@Bean`

## 6. What is a Spring Container? (BeanFactory vs ApplicationContext)
### BeanFactory

- Basic IoC container
- Lazy initialization
- No advanced features

### ApplicationContext

- Advanced, feature-rich container
- Eager initialization
- Supports AOP
- Event handling
- Internationalization

#### ApplicationContext is used in 99% applications.

## 7. What are Bean Scopes?
| Scope        | Description                                               |
|--------------|-----------------------------------------------------------|
| singleton    | One instance for the entire application.                  |
| prototype    | A new instance is created every time it is requested.     |
| request      | One instance per HTTP request.                            |
| session      | One instance per user session.                            |
| application  | One instance per servlet context (shared across the app). |

### Example:
```
@Scope("prototype")
@Component
class Car {}
```
## 8. Explain the Bean Lifecycle.

1. Bean Instantiated
2. Dependencies Injected
3. BeanNameAware / BeanFactoryAware (optional)
4. BeanPostProcessor (before init)
5. @PostConstruct
6. Bean Ready for Use
7. @PreDestroy
8. Bean Destroyed

## 9. What is @Autowired? How does it work internally?

`@Autowired` is used for automatic dependency injection.

### Internally:

- Spring uses Reflection to find matching types
- If more than one bean → throws exception
- To fix conflict → use @Qualifier or @Primary
- After identifying the correct bean → injects it

## 10. What is @Qualifier?

Used when more than one bean of the same type exists.
```
@Autowired
@Qualifier("dieselEngine")
Engine engine;
```

It removes ambiguity.

## 11. What is @Primary?

Marks a bean as default when multiple beans match the type.
```
@Primary
@Bean
Engine petrolEngine() {}
```
## 12. What is Component Scanning?

Allows Spring to automatically detect classes annotated with:

- `@Component`
- `@Service`
- `@Repository`
- `@Controller`

Example:
```
@ComponentScan("com.myapp")
```
## 13. What is AOP? Why do we use it?

AOP = Aspect Oriented Programming
Used to separate cross-cutting concerns such as:

- Logging
- Security
- Transactions
- Performance monitoring

### AOP Concepts

- Aspect → Class containing cross-cutting logic
- Advice → Code executed at a specific point
- Pointcut → Expression defining where to apply advice
- JoinPoint → Actual method execution
- Weaving → Process of linking advice with methods

## 14. What is @Value?

Injects values from `application.properties` into fields.
```
@Value("${app.name}")
private String appName;
```
## 15. What is @Profile?

Used to create environment-specific beans (dev, test, prod).
```
@Profile("dev")
@Component
class DevConfig {}
```
## 16. What is @Lazy?

Delays bean initialization until it is actually needed.
```
@Lazy
@Component
class HeavyBean {}
```
17. What is circular dependency?

Circular dependency occurs when:
```
A needs B
B needs A
```

Constructor injection → ❌ breaks
Setter injection → ✔ works

Spring detects and prevents circular dependencies.

## 18. What is BeanPostProcessor?

It allows us to intercept bean creation before and after initialization.

### Uses:

- Logging
- Auditing
- Custom initialization
- Changing bean properties

## 19. What is FactoryBean?

A special bean that returns another bean.

### Used for:

- Complex objects
- Dynamic proxies
```
class CarFactory implements FactoryBean<Car> {
    public Car getObject() { return new Car(); }
}
```
## 20. What are Stereotype Annotations?

## Specialized Forms of `@Component`

Spring provides several stereotype annotations that are specialized versions of `@Component`:

| Annotation        | Purpose        |
|-------------------|----------------|
| `@Service`        | Business logic |
| `@Repository`     | Data access    |
| `@Controller`     | MVC            |
| `@RestController` | REST APIs      |


## 21. What is the Environment Interface?

Helps read environment properties at runtime.
```
env.getProperty("app.name");
```
## 22. What is @PropertySource?

Loads external `.properties` files.
```
@PropertySource("classpath:app.properties")
```
## 23. What is ApplicationContextAware?

If a class implements this interface, Spring injects the ApplicationContext into it.

Useful for framework-based applications.

## 24. What is the difference between Singleton and Prototype?
### Singleton:

- One instance per Spring container

### Prototype:

- New instance for each request

Singleton is default for Spring beans.

## 25. Difference between Spring and Spring Boot?
### Spring:

- Requires manual configuration
- Complex setup
- No embedded server

### Spring Boot:

- Auto-configuration
- Embedded servers
- Starter dependencies
- Production ready

“In this way, we studied the same topic twice.” 😄😊
