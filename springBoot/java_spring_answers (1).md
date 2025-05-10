# Java and Spring Framework Answers

## I. Questions Core Java ABHISHEK

1. **Define JDK, JRE and JVM:**
   - JDK (Java Development Kit): It is a package that has all tools needed to make Java programs. It has compiler, debugger and other tools.
   - JRE (Java Runtime Environment): It is the software that helps to run Java programs on your computer.
   - JVM (Java Virtual Machine): It is the engine that changes Java code into machine code that your computer can understand.

2. **Significance of Public Static void main ():**
   - It is the starting point of every Java program
   - Public: Anyone can access it from anywhere
   - Static: You can use it without making an object
   - Void: It does not give back any value
   - Main: It is the name that JVM looks for to start running your program

3. **Why main method support String args ()?**
   - It lets you give inputs to your program when you start it
   - These inputs are saved as text strings in an array called args
   - Example: When you type "java MyProgram hello world", "hello" and "world" go into the args array

4. **Constructor chaining:**
   - It means calling one constructor from another constructor in same class
   - We use "this()" to call another constructor in same class
   - We use "super()" to call parent class constructor
   - It helps to avoid writing same code again and again

5. **OOPs Concept (Inheritance, Polymorphism, Abstraction, encapsulation):**
   - Inheritance: It is like a child getting features from parents. A class can get properties and methods from another class.
   - Polymorphism: It means "many forms". Same method can work differently based on inputs or which object uses it.
   - Abstraction: It means hiding the complex details and showing only what is needed. Like a TV remote - you press buttons without knowing how it works inside.
   - Encapsulation: It means putting data and the methods that use that data together in one unit (class) and hiding the data.

6. **Difference b/w Interface and Abstract Class:**
   - Abstract class:
     - Can have both normal methods and abstract methods (methods without body)
     - Can have constructors
     - Can have instance variables
     - A class can extend only one abstract class
   - Interface:
     - Before Java 8, can have only abstract methods
     - After Java 8, can have default and static methods too
     - No constructors
     - Only constants (final variables)
     - A class can implement many interfaces

7. **Multifacet Block:**
   - It refers to the blocks used in exception handling
   - Try: The code that might cause error goes here
   - Catch: Handles the error if it happens
   - Finally: This code always runs, whether error happens or not
   - Throw: Used to create a new error
   - Throws: Tells that a method might cause an error

8. **Final, Finally and Finalize:**
   - Final: A keyword used to:
     - Make a variable constant (cannot change)
     - Make a method that cannot be overridden
     - Make a class that cannot be inherited
   - Finally: A block in exception handling that always runs
   - Finalize: A method that runs automatically before an unused object is removed from memory

9. **Why String is inheritable and how can we create our own inheritable class?**
   - Actually, String class is NOT inheritable because it is marked as "final"
   - To make your own inheritable class:
     - Do not use "final" keyword with your class
     - Example: `public class MyClass { ... }` can be inherited
     - But `public final class MyClass { ... }` cannot be inherited

10. **HashMap vs Concurrent HashMap:**
    - HashMap:
      - Not safe for multiple threads
      - Faster in single thread programs
      - May cause problems if many threads try to change it at same time
    - ConcurrentHashMap:
      - Safe for multiple threads
      - Uses segment locking to allow many threads to work with it
      - Slightly slower than HashMap in single thread programs
      - Better choice for multi-threaded programs

11. **HashSet vs HashMap:**
    - HashSet:
      - Stores only values
      - No duplicates allowed
      - Has methods like add(), remove(), contains()
      - Uses HashMap internally
    - HashMap:
      - Stores key-value pairs
      - No duplicate keys allowed
      - Has methods like put(), get(), remove()
      - Faster for retrieving specific elements

12. **Internal working of HashMap:**
    - It has array of linked lists or trees (called buckets)
    - It uses hashCode() method to decide which bucket to use
    - When two keys have same hash code, it's called collision
    - In Java 8+, it uses linked list for few elements and tree for many elements in same bucket
    - Load factor decides when to increase array size (default is 0.75)

13. **How HashSet avoid duplicate values in collection?**
    - It uses HashMap internally
    - The value you add to HashSet becomes key in that HashMap
    - It adds a dummy object as value in HashMap
    - Before adding, it checks hashCode() and equals() methods
    - If both are same for an object, it is considered duplicate and not added

14. **Stream API and its uses:**
    - Stream API was added in Java 8
    - It helps to process collections of objects
    - It supports functional-style operations
    - Common operations: filter, map, reduce, collect
    - Example: `list.stream().filter(n -> n > 10).collect(Collectors.toList())`

15. **How to restrict a class to create only single instance?**
    - Use Singleton pattern
    - Make constructor private
    - Create a static method that returns the single instance
    - Example:
      ```java
      public class Singleton {
          private static Singleton instance;
          private Singleton() {}
          public static Singleton getInstance() {
              if (instance == null) {
                  instance = new Singleton();
              }
              return instance;
          }
      }
      ```

16. **How to create a class to create only single instance? (Programs):**
    ```java
    public class Singleton {
        // Static variable for the single instance
        private static Singleton instance;
        
        // Private constructor so no one can create objects directly
        private Singleton() {
            System.out.println("Instance created");
        }
        
        // Public method to get the instance
        public static Singleton getInstance() {
            // Create object only if it doesn't exist
            if (instance == null) {
                instance = new Singleton();
            }
            return instance;
        }
        
        // Method to test
        public void showMessage() {
            System.out.println("Hello from Singleton!");
        }
    }

    // How to use it:
    class TestSingleton {
        public static void main(String[] args) {
            // Cannot do this: Singleton obj = new Singleton();
            
            // Correct way:
            Singleton obj1 = Singleton.getInstance();
            obj1.showMessage();
            
            Singleton obj2 = Singleton.getInstance();
            
            // This will print true as both variables point to same object
            System.out.println(obj1 == obj2);
        }
    }
    ```

## II. Spring Iscot

1. **Significance of springboot over spring:**
   - Less configuration needed (no XML)
   - Embedded servers (Tomcat, Jetty)
   - Automatic configuration based on dependencies
   - Faster development and deployment
   - Standalone applications without external servers

2. **Different annotations (Authorized Requested Parma etc.):**
   - @Autowired: For automatic dependency injection
   - @Controller: Marks a class as Spring MVC controller
   - @RestController: Combination of @Controller and @ResponseBody
   - @RequestMapping: Maps web requests to methods
   - @GetMapping, @PostMapping: Shortcuts for specific HTTP methods
   - @RequestParam: Extracts query parameters
   - @PathVariable: Extracts values from URI path
   - @RequestBody: Converts request body to object
   - @ResponseBody: Converts return value to response body
   - @Service: Marks service layer classes
   - @Repository: Marks data access classes
   - @Component: General Spring managed component
   - @Configuration: For Java-based configuration

3. **Difference b/w @ component and @ service:**
   - @Component: General purpose annotation for any Spring-managed class
   - @Service: Special type of @Component used for service layer classes
   - Functionally same but @Service has more specific meaning in application design

4. **@ Qualifier:**
   - Used when multiple beans of same type exist
   - Specifies which exact bean should be wired
   - Example: `@Autowired @Qualifier("userService") private UserService service;`

5. **Define MVC architecture of SpringBoot:**
   - Model: Represents data and business logic
   - View: Displays data to user (templates, JSP, Thymeleaf)
   - Controller: Handles user requests and responses
   - In Spring Boot:
     - Controllers handle requests (@Controller classes)
     - Models are POJOs or repositories
     - Views are templates (Thymeleaf, JSP) or REST responses

6. **Spring Actuator:**
   - Provides production-ready features
   - Helps monitor and manage application
   - Exposes endpoints for health, metrics, info
   - Can be secured with Spring Security
   - Example endpoints: /health, /metrics, /info

7. **Spring Scope of Bean:**
   - Singleton: One instance per Spring container (default)
   - Prototype: New instance every time requested
   - Request: One instance per HTTP request
   - Session: One instance per HTTP session
   - Application: One instance per ServletContext
   - WebSocket: One instance per WebSocket session

8. **SpringBoot Scope of bear:**
   - Same as Spring scopes mentioned above
   - Configured using @Scope annotation
   - Example: `@Scope("prototype")`

9. **Default scope:**
   - Singleton is the default scope in Spring/Spring Boot
   - Only one instance exists per Spring container

### Hibernate

1. **What is the significance of Hibernate over JDBC:**
   - ORM (Object-Relational Mapping) - work with objects instead of queries
   - Less code to write
   - Database independent
   - Automatic connection management
   - Caching mechanisms for better performance
   - Transaction management
   - Lazy loading of objects

2. **What is Session and Sessionfactory in Hibernate:**
   - SessionFactory:
     - Heavy-weight object created once during application start
     - Thread-safe and used by all application threads
     - Creates Session objects
     - Expensive to create, so usually one per database
   - Session:
     - Lightweight object
     - Created per operation/transaction
     - Not thread-safe
     - Short-lived object
     - Used to get a physical connection with database

3. **What is Dialect in Hibernate:**
   - Tells Hibernate which SQL variant to use
   - Helps Hibernate generate appropriate SQL for specific database
   - Examples:
     - MySQL: org.hibernate.dialect.MySQLDialect
     - Oracle: org.hibernate.dialect.OracleDialect
     - PostgreSQL: org.hibernate.dialect.PostgreSQLDialect

4. **Mappings in Hibernate:**
   - Ways to define relationship between Java objects and database tables
   - Types:
     - @OneToOne: One row in first table relates to one row in second table
     - @OneToMany: One row in first table relates to many rows in second table
     - @ManyToOne: Many rows in first table relate to one row in second table
     - @ManyToMany: Many rows in first table relate to many rows in second table

5. **What is lazy Eager Loading in Hibernate:**
   - Lazy Loading:
     - Data is loaded only when specifically requested
     - Saves memory and initial load time
     - May cause N+1 query problem if not used carefully
     - Default for collections (@OneToMany, @ManyToMany)
   - Eager Loading:
     - Data is loaded immediately when parent object is loaded
     - Requires more memory but avoids multiple database hits
     - Default for single point associations (@OneToOne, @ManyToOne)

6. **How do we declare multiple primary in Hibernate:**
   - Use @EmbeddedId annotation with an embeddable class
   - Example:
     ```java
     @Embeddable
     public class OrderItemId implements Serializable {
         private Long orderId;
         private Long productId;
         // getters, setters, equals, hashCode
     }
     
     @Entity
     public class OrderItem {
         @EmbeddedId
         private OrderItemId id;
         // other fields
     }
     ```

7. **What is a POJO Class:**
   - POJO = Plain Old Java Object
   - Simple Java class with:
     - Private fields
     - Public getters/setters
     - No special restrictions
     - Not dependent on framework classes
   - Used in Hibernate as entity classes with @Entity annotation
