# Open-Closed Principle

The Open-Closed Principle requires that classes should be open for extension and closed to modification.

Modification means changing the code of an existing class, and extension means adding new functionality.

So what this principle wants to say is: We should be able to add new functionality without touching the existing code for the class. This is because whenever we modify the existing code, we are taking the risk of creating potential bugs. So we should avoid touching the tested and reliable (mostly) production code if possible.

Suppose in the SRP example, our customer asks to add persistance to database. One way would be to modify the InvoicePersistence like following:

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

However, doing this breaks OCP principle. We should refactor this:

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

Another example:
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