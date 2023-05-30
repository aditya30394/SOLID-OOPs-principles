# The Single Responsibility Principle

A class should have only one reason to change, meaning it should have a single responsibility.

We will look at the code for a simple bookstore invoice program as an example. Let's start by defining a book class to use in our invoice followed by the invoice classs.

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

The SRP is violated because the inly reason Invoice class should change is when the logic to calculate the total changes. But this will change even if we have change to print mogic or save to file logic.

To fix this, we should create separate classes so that they are responsible for only one thing. Here's the modified code

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