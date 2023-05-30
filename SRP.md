# The Single Responsibility Principle

The Single Responsibility Principle (SRP) states that a class should have only one reason to change, indicating that it should be responsible for a single task or responsibility.

Let's consider a simple example of a bookstore invoice program. We will define two classes: the Book class, representing a book, and the Invoice class, representing an invoice.

```
class Book {
	String name;
	String authorName;
	int year;
	int price;
	String isbn;

	public Book(String name, String authorName, int year, int price, String isbn) {
		this.name = name;
		this.authorName = authorName;
		this.year = year;
        this.price = price;
		this.isbn = isbn;
	}
}

public class Invoice {

	private Book book;
	private int quantity;
	private double discountRate;
	private double taxRate;
	private double total;

	public Invoice(Book book, int quantity, double discountRate, double taxRate) {
		// logic
	}

	public double calculateTotal() {
        // logic
    }

	public void printInvoice() {
        // logic
	}

    public void saveToFile(String filename) {
	// Creates a file with given name and writes the invoice
	}
}
```

In the above code, the SRP is violated because the Invoice class has multiple reasons to change. It should only change when the logic to calculate the total changes, but currently, it will also change if the print logic or save to file logic is modified.

To adhere to the SRP, we need to refactor the code and create separate classes, each responsible for a single task. Here's the modified code:


```
public class InvoicePrinter {
    private Invoice invoice;

    public InvoicePrinter(Invoice invoice) {
        this.invoice = invoice;
    }

    public void print() {
    }
}

public class InvoicePersistence {
    Invoice invoice;

    public InvoicePersistence(Invoice invoice) {
        this.invoice = invoice;
    }

    public void saveToFile(String filename) {
        // Creates a file with given name and writes the invoice
    }
}
```

Another easier example is

```
// Example violating SRP
class Customer {
   void CalculateDiscount() { /* ... */ }
   void SendEmail() { /* ... */ }
   void SaveToDatabase() { /* ... */ }
}

// Refactored example following SRP
class Customer {
   void CalculateDiscount() { /* ... */ }
}

class EmailService {
   void SendEmail() { /* ... */ }
}

class Database {
   void SaveToDatabase() { /* ... */ }
}
```