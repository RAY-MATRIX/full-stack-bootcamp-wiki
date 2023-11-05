# 10 Oct 2023 - Lecture
## Java第七课 Spring 原理详解

- SOLID原则
    - 单一职责原则(Single Responsibility Principle)
        This principle states that a class should have only one reason to change. In other words, a class should have a single, well-defined responsibility. When a class has multiple responsibilities, it becomes harder to maintain and reason about, and changes in one area can unintentionally affect other areas.
    - 开闭原则(Open Close Principle)
        The Open/Closed Principle suggests that software entities (such as classes, modules, or functions) should be open for extension but closed for modification. In other words, you should be able to add new functionality or behavior to a software component without changing its existing code. This principle promotes code reusability and minimizes the risk of introducing new bugs when extending a system.
    - 里氏替换原则（Liskov Substitution Principle)
        The Liskov Substitution Principle states that objects of a derived class should be able to replace objects of the base class without affecting the correctness of the program. In simpler terms, if a class inherits from another class, it should be a true subtype and adhere to the behavior expected of the base class. Violating this principle can lead to unexpected behavior in a program.
    - 接口隔离原则(Interface Segregation Principle)
        The Interface Segregation Principle recommends that clients should not be forced to depend on interfaces they do not use. In other words, when defining interfaces, it's better to have smaller, specialized interfaces rather than large, monolithic ones. This allows clients to implement only the methods they need and prevents them from being burdened with irrelevant methods.
    - 依赖倒置原则(Dependence Inversion Principle)
        The Dependency Inversion Principle encourages high-level modules to depend on abstractions, not on concrete implementations. It suggests that the details of low-level components should depend on high-level policies and abstractions, not the other way around. This principle facilitates loose coupling, making code more flexible and easier to maintain.
    - 迪米特法则(Law of Demter)

    
- 23 种设计模式
    - 创建型模式： 对类的实例化过程进行了抽象。 5种
        - Singleton
        - Factory Method
        - Abstract Factory
        - Builder
        - Prototype
    - 结构型模式： 关注于对象的组成以及对象之间的依赖关系。 7种
        - Adapter
        - Bridge
        - Composite
        - Decorator
        - Facade
        - Flyweight
        - Proxy
    - 行为型模式： 关注于对象的行为问题，对象之间的职责划分和相互作用。11种
        - Chain of Responsibility
        - Command
        - Interpreter
        - Iterator
        - Mediator
        - Memento
        - Observer
        - State
        - Strategy
        - Template Method
        - Visitor
    - Architectural Patterns:
        - Model-View-Controller (MVC)
        - Model-View-Presenter (MVP)
        - Model-View-ViewModel (MVVM)