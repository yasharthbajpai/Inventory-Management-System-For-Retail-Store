# E-Commerce Application

## Project Scenario 2:

You are building an e-commerce system that tracks
orders, products, and customer details. The system needs
to store data in a relational database. The company
wants you to use Hibernate for mapping the object model
to the database, and they are also considering adopting
Spring Data JPA for easier data management in the
future.

## Project Questions with Answers:

### 1. Hibernate's Automatic Mapping of Java Objects to Database Tables

Hibernate is an Object-Relational Mapping (ORM) framework that facilitates the mapping of Java objects to database tables, enabling developers to work with database records as if they were Java objects. Here’s how it works:

#### a. Annotations and XML Configuration
- **Annotations**: Hibernate uses annotations to define how a Java class maps to a database table. For example:
  - `@Entity`: Marks the class as a persistent entity.
  - `@Table`: Specifies the table name if it differs from the class name.
  - `@Id`: Indicates the primary key.
  - `@GeneratedValue`: Defines the strategy for primary key generation.
  - `@Column`: Maps a field to a specific database column.

  Example:
  ```bash
  import javax.persistence.*;

  @Entity
  @Table(name = "customers")
  public class Customer {
      @Id
      @GeneratedValue(strategy = GenerationType.IDENTITY)
      private Long id;

      @Column(name = "name")
      private String name;

      @Column(name = "email")
      private String email;

      // Getters and Setters
  }
  ```

- **XML Configuration**: Alternatively, developers can use XML files to define mappings, though this is less common with the prevalence of annotations.

#### b. Session Management
- **Session**: A `Session` is a single-threaded, short-lived object that represents a connection to the database. It is used to perform CRUD operations. The `SessionFactory` is responsible for creating `Session` instances.
  
- **Object State Management**: Hibernate manages the state of the Java objects (transient, persistent, detached, and removed) and automatically synchronizes changes made to these objects with the database.

#### c. Schema Generation
- **Automatic Schema Generation**: Hibernate can automatically generate database schemas based on the entity mappings. The `hibernate.hbm2ddl.auto` property in the configuration file specifies the behavior of schema management:
  - `update`: Updates the existing schema without dropping data.
  - `create`: Creates a new schema, replacing any existing schema.
  - `create-drop`: Creates the schema at startup and drops it at shutdown.
  - `validate`: Validates the schema against the existing database structure without making changes.

### 2. Database Schema Evolution

- **Schema Evolution**: Hibernate supports schema evolution through the configuration options that allow developers to specify how to manage changes in the database schema over time. This can include adding new fields, modifying existing ones, or changing relationships between entities. With the `update` setting, Hibernate can adjust the schema to match the current entity model without losing existing data.

### 3. Difference Between Hibernate and Spring Data JPA

| Feature                   | Hibernate                                      | Spring Data JPA                               |
|---------------------------|------------------------------------------------|------------------------------------------------|
| **Type**                  | ORM framework for direct database interaction  | Abstraction over JPA (Java Persistence API)  |
| **Boilerplate Code**      | Requires more boilerplate code for session management and CRUD operations | Reduces boilerplate code through repository interfaces |
| **Built-in Repositories**  | Does not provide built-in repository interfaces; requires manual implementation | Provides predefined repository interfaces like `CrudRepository` and `JpaRepository` |
| **Integration**           | Can be used standalone or with other frameworks | Designed to work seamlessly with Spring and Spring Boot |
| **Query Mechanism**       | Uses HQL (Hibernate Query Language) and native SQL | Supports derived queries, JPQL, and criteria queries |
| **Caching**               | Supports first-level and second-level caching  | Integrates with Hibernate's caching mechanisms |

### 4. Migrating from Hibernate to Spring Data JPA

Migrating an application from Hibernate to Spring Data JPA involves several steps:

#### a. Add Dependencies
Include the Spring Data JPA dependency in your `pom.xml` or `build.gradle`:

**Maven**:
```bash
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

#### b. Define Repository Interface
Create an interface for the `Customer` entity that extends `JpaRepository`:

```bash
import org.springframework.data.jpa.repository.JpaRepository;

public interface CustomerRepository extends JpaRepository<Customer, Long> {
    List<Customer> findByName(String name);
}
```

This automatically provides common CRUD methods, reducing boilerplate code.

#### c. Service Layer
Create a service class to encapsulate the business logic and interact with the `CustomerRepository`:

```bash
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class CustomerService {

    @Autowired
    private CustomerRepository customerRepository;

    public Customer saveCustomer(Customer customer) {
        return customerRepository.save(customer);
    }

    public List<Customer> getAllCustomers() {
        return customerRepository.findAll();
    }

    public Customer getCustomerById(Long id) {
        return customerRepository.findById(id).orElse(null);
    }

    public void deleteCustomer(Long id) {
        customerRepository.deleteById(id);
    }
}
```

#### d. Configuration
Use `application.properties` for configuration instead of `hibernate.cfg.xml`:

```bash
spring.datasource.url=jdbc:mysql://localhost:3306/ecommerce
spring.datasource.username=root
spring.datasource.password=password
spring.jpa.hibernate.ddl-auto=update
```

#### e. Spring Boot Initialization
Ensure your application class is annotated with `@SpringBootApplication` to enable component scanning and auto-configuration.

### Conclusion

Migrating to Spring Data JPA not only simplifies the codebase but also leverages the capabilities of Spring's dependency injection, transaction management, and other features. This transition enhances maintainability and readability while reducing boilerplate code significantly.
