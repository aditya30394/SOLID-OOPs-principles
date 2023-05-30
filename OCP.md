# Open-Closed Principle

The Open-Closed Principle (OCP) states that classes should be open for extension, allowing new functionality to be added, but closed for modification, meaning that the existing code should not be altered.

Modifying existing code carries the risk of introducing bugs or unintended side effects. Therefore, the OCP encourages us to avoid making changes to tested and reliable production code whenever possible.

Let's consider the example of the Bookstore Invoice program from the previous SRP explanation. Suppose our customer requests the addition of database persistence to the invoice functionality. One approach would be to modify the InvoicePersistence class as follows:

```
public class InvoicePersistence {
    Invoice invoice;

    public InvoicePersistence(Invoice invoice) {
        this.invoice = invoice;
    }

    public void saveToFile(String filename) {
        // Creates a file with given name and writes the invoice
    }

    public void saveToDatabase() {
        // Saves the invoice to database
    }
}
```

However, this violates the OCP because we are modifying the existing class to accommodate the new requirement. Instead, we should follow the principle by extending the functionality without modifying the existing code. Here's an alternative approach:

```
interface InvoicePersistence {

    public void save(Invoice invoice);
}

public class DatabasePersistence implements InvoicePersistence {

    @Override
    public void save(Invoice invoice) {
        // Save to DB
    }
}

public class FilePersistence implements InvoicePersistence {

    @Override
    public void save(Invoice invoice) {
        // Save to file
    }
}
```

![image](https://github.com/aditya30394/SOLID-OOPs-principles/assets/8336542/86f9f224-5149-4d52-a9e8-468ec5aa00f8)

Now our persistence logic is easily extendable. 

But let's say that we extend our app and have multiple persistence classes like InvoicePersistence, BookPersistence and we create a PersistenceManager class that manages all persistence classes:

```
public class PersistenceManager {
    InvoicePersistence invoicePersistence;
    BookPersistence bookPersistence;
    
    public PersistenceManager(InvoicePersistence invoicePersistence,
                              BookPersistence bookPersistence) {
        this.invoicePersistence = invoicePersistence;
        this.bookPersistence = bookPersistence;
    }
}
```

We can now pass any class that implements the InvoicePersistence interface to this class with the help of polymorphism. This is the flexibility that interfaces provide.

## Another example:

```
// Example violating OCP
class Shape {
   void Draw() {
      if (type == "circle") {
         // Draw circle
      } else if (type == "rectangle") {
         // Draw rectangle
      }
   }
}

// Refactored example following OCP
abstract class Shape {
   abstract void Draw();
}

class Circle : Shape {
   override void Draw() {
      // Draw circle
   }
}

class Rectangle : Shape {
   override void Draw() {
      // Draw rectangle
   }
}

```