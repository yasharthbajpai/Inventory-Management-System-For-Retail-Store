# E-Commerce Application

This project is a Spring Boot-based e-commerce system that manages customers, products, and orders using Spring Data JPA for data persistence.

## Features

- CRUD operations for customers, products, and orders
- In-memory H2 database for easy development and testing
- RESTful API endpoints
- Automatic database schema generation and updates

## Project Structure

```
com.yasharthbajpai
├── controller
│   └── ProductController.java
├── model
│   ├── Customer.java
│   ├── Order.java
│   └── Product.java
├── repository
│   ├── CustomerRepository.java
│   ├── OrderRepository.java
│   └── ProductRepository.java
├── service
│   └── ProductService.java
└── YasharthApplication.java
```

## Setup and Running

1. Ensure you have Java 21 and Maven installed.
2. Clone the repository.
3. Run `mvn spring-boot:run` in the project root.
4. The application will start on `http://localhost:8080`
5. Access the H2 console at `http://localhost:8080/h2-console`
  - JDBC URL: `jdbc:h2:mem:ecommerce_db`
  - Username: `yasharth`
  - Password: `root`

## API Endpoints

### Products
- GET `/products`: Retrieve all products
- GET `/products/{id}`: Get a product by ID
- POST `/products`: Create a new product
- DELETE `/products/{id}`: Delete a product

Similar endpoints exist for customers and orders (to be implemented).

## Configuration

Key application properties (in `src/main/resources/application.properties`):

```properties
spring.application.name=yasharth
spring.datasource.url=jdbc:h2:mem:ecommerce_db
spring.h2.console.enabled=true
spring.jpa.hibernate.ddl-auto=update
```

## Technologies

- Spring Boot 3.1.5
- Spring Data JPA
- H2 Database
- Lombok
- Maven

## Future Improvements

- Implement Customer and Order services and controllers
- Add validation and error handling
- Implement security features
- Create unit and integration tests

For more details, please refer to the source code and comments within the project.
