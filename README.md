***

# Java Repository Design Pattern Example

This project is a simple, clear, and executable Java example demonstrating the **Repository Design Pattern**. Its primary purpose is to illustrate how to decouple an application's business logic from its data access layer, leading to a more flexible, maintainable, and testable codebase.

## What is the Repository Pattern?

The Repository Pattern is a software design pattern that mediates between the domain (business logic) and data mapping layers of an application. It provides an abstraction layer that hides the details of how data is stored and retrieved.

Think of it as a collection-like interface for accessing domain objects. The business logic interacts with the repository to query, add, and remove objects, without needing to know anything about the underlying database technology (e.g., SQL, NoSQL, or even a simple in-memory store).

## Key Benefits Illustrated by this Project

This example is structured to highlight the three primary advantages of using the Repository Pattern.

### 1. Decoupling and Abstraction

The business logic, encapsulated in the `ProductService`, is completely decoupled from the data storage mechanism. It depends only on the `Repository<T>` interface, not on a concrete implementation.

This allows us to easily swap the data source. In the `main` method, we seamlessly switch between an in-memory data store and a simulated database store without changing a single line of code in the `ProductService`.

```java
// In main(), we can switch implementations easily:

// Use in-memory repository for testing or development
Repository<Product> inMemoryRepository = new InMemoryProductRepository();
ProductService inMemoryProductService = new ProductService(inMemoryRepository);

// Use database repository for production
Repository<Product> databaseRepository = new DatabaseProductRepository();
ProductService databaseProductService = new ProductService(databaseRepository);

// The ProductService code doesn't change at all!
```

### 2. Enhanced Testability

Decoupling the business logic from the data layer makes unit testing significantly easier. The `ProductService` can be tested in isolation by providing it with a fast, predictable, in-memory repository (`InMemoryProductRepository`). This eliminates the need for a live database connection during testing, resulting in faster and more reliable unit tests.

### 3. Centralized Data Access Logic

All data access code is encapsulated within specific repository implementations (`InMemoryProductRepository`, `DatabaseProductRepository`). This means:
*   **Easier Maintenance:** If you need to change a database query or switch from JDBC to JPA, you only need to modify the relevant repository class. The business logic remains untouched.
*   **Improved Code Organization:** It prevents data access logic from being scattered throughout your application's business layer, making the codebase cleaner and easier to navigate.
*   **Cross-cutting Concerns:** Caching, logging, or other cross-cutting concerns related to data access can be implemented in one central place (the repository implementation).

## Project Structure

The project is contained within a single file, `RepositoryPatternExample.java`, and is composed of the following key components:

*   **`Product`**: A simple entity class representing the data model.
*   **`Repository<T>`**: The generic interface that defines the contract for our data access operations (e.g., `findById`, `save`). This is the core of the pattern.
*   **`InMemoryProductRepository`**: A concrete implementation of the `Repository` interface that uses an in-memory `HashMap` as its data store. Ideal for development and testing.
*   **`DatabaseProductRepository`**: A mock implementation that simulates interaction with a real database by printing messages to the console. In a real-world application, this is where your JDBC or JPA code would reside.
*   **`ProductService`**: The business logic layer. It depends on the `Repository<T>` interface to perform its operations, making it independent of the data source.
*   **`RepositoryPatternExample`**: The main class with the `main` method, which wires the application together and demonstrates the ease of switching between different repository implementations.

## How to Compile and Run

You can compile and run this example using a standard Java Development Kit (JDK).

1.  **Save the Code**: Save the provided code as `RepositoryPatternExample.java` inside a directory structure that matches the package name (e.g., `./csd214/f25/RepositoryPatternExample.java`).

2.  **Navigate to the Root Directory**: Open a terminal or command prompt and navigate to the root directory (the one containing the `csd214` folder).

3.  **Compile the Java file**:
    ```bash
    javac csd214/f25/RepositoryPatternExample.java
    ```

4.  **Run the application**:
    ```bash
    java csd214.f25.RepositoryPatternExample
    ```

### Expected Output

You will see the following output, which demonstrates the application logic first using the in-memory repository and then the simulated database repository.

```
Using In-Memory Repository:
Created product: Product{id=1, name='Laptop', price=1200.0}
Product{id=1, name='Laptop', price=1200.0}

Using Database Repository:
Database: Saving product Product{id=null, name='Mouse', price=25.0}
Created product (in database): Product{id=null, name='Mouse', price=25.0}
```