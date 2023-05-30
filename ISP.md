# Interface Segregation Principle
The Interface Segregation Principle (ISP) is about separating interfaces to keep things organized and focused. It states that clients should only depend on the interfaces they actually use. Instead of having large interfaces that encompass multiple functionalities, we should create smaller, more specific interfaces tailored to the needs of each client.

The goal of ISP is to prevent clients from being burdened with unnecessary dependencies and obligations. By adhering to this principle, we can design cleaner and more maintainable code.

Let's look at the code example to understand how ISP can be applied:

```
// Example violating ISP
interface IShape {
   void Draw();
   void Resize();
}

class Circle : IShape {
   void Draw() { /* ... */ }
   void Resize() { /* ... */ }
}

class Square : IShape {
   void Draw() { /* ... */ }
   void Resize() { /* ... */ }
}
```

In the initial code example, we have an interface called IShape that defines two methods: Draw() and Resize(). This interface is implemented by two classes, Circle and Square, each providing their own implementation of the methods.

The problem with this design is that not all classes implementing the IShape interface require both Draw() and Resize() methods. For instance, the Circle class only needs to implement the Draw() method, while the Square class needs to implement both methods.

This violates the ISP because clients that depend on the IShape interface, such as a client specifically interested in drawing shapes, are forced to implement the Resize() method, even if it is not relevant to their needs.

To address this issue and adhere to the ISP, we can refactor the code by creating separate interfaces for each behavior:

```
// Refactored example following ISP
interface IDrawable {
   void Draw();
}

interface IResizable {
   void Resize();
}

class Circle : IDrawable {
   void Draw() { /* ... */ }
}

class Square : IDrawable, IResizable {
   void Draw() { /* ... */ }
   void Resize() { /* ... */ }
}
```

In the refactored code, we have two interfaces: IDrawable and IResizable. The Circle class now only implements the IDrawable interface since it only needs to provide a Draw() method. On the other hand, the Square class implements both the IDrawable and IResizable interfaces as it requires both the Draw() and Resize() methods.

By segregating the interfaces based on behavior, we ensure that clients can depend on the specific interfaces they need. Clients interested in drawing shapes can depend on the IDrawable interface, while clients interested in resizing shapes can depend on the IResizable interface. This approach avoids burdening clients with irrelevant methods, improves code readability, and promotes better separation of concerns.

In conclusion, the Interface Segregation Principle (ISP) guides us to create focused interfaces that align with client needs. By adhering to this principle, we achieve code that is more modular, flexible, and easier to maintain and understand.