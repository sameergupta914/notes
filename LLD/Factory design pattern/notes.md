# factory design principle

# Simple factory design

- The simple factory method is not a formal design pattern but rather a design principle. It involves a factory class that decides which concrete class to instantiate based on the type provided. Essentially, the simple factory creates objects without exposing the instantiation logic to the client.

- In the context of the example provided, consider a burger shop where multiple types of burgers are available. The factory class would determine which specific type of burger to create based on the input type (e.g., standard or premium). The factory class has a method that returns a product of a specific type, allowing the client to request an object without needing to know the details of how that object is created.

- The definition of the simple factory can be summarized as follows: it is a factory class that decides which concrete class to instantiate based on the type provided. This encapsulation of object creation helps in decoupling the client from the specific classes being instantiated, making the code cleaner and easier to manage.

# Factory design pattern

- The factory method is an extended version of the simple factory design principle. It is a proper design pattern that allows a class to delegate the responsibility of instantiating objects to subclasses. In the factory method pattern, a factory class decides which concrete class to instantiate based on certain criteria, such as the type of product requested.

- For example, consider a scenario with different types of burgers being prepared in a burger shop. The factory method would allow the burger shop to create different types of burgers (like King Burger and Sing Burger) without the client needing to know the specifics of how these burgers are created. The client simply requests a burger, and the factory method handles the instantiation of the appropriate burger class.

- In essence, the factory method promotes loose coupling by decoupling the client from the specific classes it needs to instantiate, allowing for greater flexibility and easier maintenance in the code. This design pattern is particularly useful when the exact types of objects to be created are not known until runtime.

# Abstract factory method

- The abstract factory method provides an interface for creating families of related objects without specifying their concrete classes. This means that a single factory can produce multiple products that are related to each other. The abstract factory method allows for the creation of objects that belong to a particular family, ensuring that the products are compatible with one another.

- In the context provided, the abstract factory method is explained through the example of a factory responsible for producing different types of products (like concrete products). Each factory can create multiple related products, and the method allows for the creation of these products without needing to know the specific classes of the products being created.

- The abstract factory method is considered a proper design pattern, unlike a simple factory, which is more of a design principle. The abstract factory method allows for a higher level of abstraction in the creation of objects, making it easier to manage and extend the codebase.