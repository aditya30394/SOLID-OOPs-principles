# Liskov Substitution Principle
The Liskov Substitution Principle (LSP) states that subclasses should be able to be used interchangeably with their base classes without causing any unexpected or incorrect behavior.

In simpler terms, if class B is a subclass of class A, we should be able to pass an object of class B to any method or code that expects an object of class A, and everything should work as expected without any strange outcomes.

Let's examine an example that violates the LSP:

```
// Example violating LSP
class Rectangle {
   int Width { get; set; }
   int Height { get; set; }

   void SetWidth(int width) {
      Width = width;
   }

   void SetHeight(int height) {
      Height = height;
   }

   public int getArea() {
		return width * height;
	}
}

class Square : Rectangle {
   override void SetWidth(int width) {
      Width = width;
      Height = width;
   }

   override void SetHeight(int height) {
      Height = height;
      Width = height;
   }
}
```
In this example, we have a Rectangle class and a Square class that extends the Rectangle class. The issue arises when we override the setters for SetWidth and SetHeight in the Square class to ensure that both properties are updated simultaneously, maintaining the square property.

Although it seems like a reasonable approach, it violates the Liskov Substitution Principle. The reason is that the behavior of the Square class differs from the expected behavior of the base Rectangle class, where setting width and height can result in different values.

This violation can cause problems in code that relies on the LSP. For example:
```
class Test {

   static void getAreaTest(Rectangle r) {
      int width = r.getWidth();
      r.setHeight(10);
      System.out.println("Expected area of " + (width * 10) + ", got " + r.getArea());
   }

   public static void main(String[] args) {
      Rectangle rc = new Rectangle(2, 3);
      getAreaTest(rc);

      Rectangle sq = new Square();
      sq.setWidth(5);
      getAreaTest(sq);
   }
}
```

The above code expects the calculated area to be consistent and predictable. However, due to the violation of the LSP in the Square class, the calculated area is not as expected.

A better approach to adhere to the LSP is to refactor the code by introducing an abstract base class called Shape:
```
public interface IShape
{
    double Area();
}

public class Rectangle : IShape
{
    public double Width { get; set; }
    public double Height { get; set; }

    public double Area()
    {
        return Width * Height;
    }
}

public class Square : IShape
{
    public double Side { get; set; }

    public double Area()
    {
        return Side * Side;
    }
}
```

In this refactored code, both the Rectangle and Square classes implement the IShape interface, which defines the contract for calculating the area of a shape. Now, any code that depends on the IShape interface can work with any object that implements the interface, including both rectangles and squares.

By adhering to the Liskov Substitution Principle, we ensure that objects of derived classes (Rectangle, Square) can be substituted for objects of the base class (IShape) without causing any issues or violating the expected behavior defined by the base class.

To summarize, the Liskov Substitution Principle promotes the use of interfaces or abstract classes to define contracts and ensures that derived classes honor those contracts when substituting for the base class. This allows for interchangeable components, enhances code reusability, and makes the code more maintainable and flexible.