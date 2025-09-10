# decorator design pattern

- The decorator design pattern is a structural pattern that allows behavior to be added to individual objects, either statically or dynamically, without affecting the behavior of other objects from the same class.

- In the context provided in the transcript, the decorator pattern is illustrated using a character named Mario. The idea is to enhance the capabilities of a base class (in this case, Mario) by wrapping it with additional functionalities through decorators.

Here's a detailed explanation based on the transcript:

- Base Class and Child Class: The pattern starts with a base class (Mario) and a child class that extends the base class. The child class can override methods from the base class to change or enhance its behavior.

- Dynamic Behavior Change: The decorator pattern allows for dynamic changes to the output of methods. For example, when the method doSomething is called on an object, it might return "I did something." However, using decorators, this output can be modified dynamically. For instance, a decorator can change the output to "I did something amazingly."

- Chaining Decorators: The transcript mentions that decorators can be stacked or chained. This means you can attach multiple decorators to an object. For example, you can have a decorator that adds a "height up" capability, another that adds "gun power," and yet another that adds "star power." Each decorator enhances the base functionality of the Mario character.

- Printing Enhanced Outputs: As each decorator is applied, the output can be printed to show how the character's capabilities have changed. For instance, after applying all decorators, the final output might show "Mario with height up, with gun power, with star power."

- Flexibility and Extensibility: The decorator pattern provides flexibility as new decorators can be added without modifying the existing code. This allows for easy extensibility of functionalities.

- Implementation: In practice, a decorator class would implement the same interface as the base class and hold a reference to an instance of the base class. When a method is called on the decorator, it can call the method on the base class and then modify the result before returning it.

- In summary, the decorator design pattern is a powerful way to enhance or modify the behavior of objects at runtime, allowing for greater flexibility and maintainability in code.