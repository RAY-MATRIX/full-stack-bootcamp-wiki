# 15 Oct 2023 - Lecture
## Java第八课 Spring原理详解（续）

- Spring AOP
    In Spring AOP (Aspect-Oriented Programming), there are several common concepts and components that you need to be familiar with to understand how AOP works and how it's applied in the Spring framework. These concepts include:

    - **Aspect**: 用来定义一个切面。An aspect is a module that encapsulates cross-cutting concerns, such as logging, security, or transaction management. Aspects contain advice and can be applied to multiple parts of an application.

    - **Advice**通知: Advice is the action taken by an aspect at a particular join point. It represents the code that gets executed when a specific pointcut is encountered in the application's execution.

    - **Join Point**: A join point is a point in the execution of the application where an aspect's advice can be applied. Examples of join points include method calls, method executions, and object instantiation.

    - **Pointcut**: 用于定义切入点表达式。A pointcut is an expression that defines a set of join points where advice will be applied. It specifies the conditions or criteria for selecting join points in the application's execution flow.

    - **AspectJ Expressions**: Spring AOP uses AspectJ-style pointcut expressions to define pointcuts. These expressions allow you to precisely target specific methods or join points for advice application.

    - **Weaving**: Weaving is the process of integrating aspects with the target object's code. There are two main types of weaving: compile-time weaving and runtime weaving. Spring AOP primarily uses runtime weaving, which means that aspects are applied at runtime without modifying the original source code.

    - **Proxy**: Spring AOP uses dynamic proxies to create proxy objects that wrap the target object. The proxy intercepts method calls and applies the advice defined by aspects before or after the actual method execution.

    - **Advice Types**: Spring AOP supports different types of advice, including "before" advice (executed before the join point), "after" advice (executed after the join point), and "around" advice (wraps the join point method and can control its execution).

    - **Target Object**: The target object is the object that is being advised. It is the actual object on which methods are invoked.

    - **AspectJ Annotations**: Spring AOP also provides annotations, such as 
        - `@Aspect`, 
        - `@Before`, 用于定义前置通知
        - `@After`, 用于定义后置通知
        - `@Around`, 用于定义环绕通知 etc., that can be used to declare aspects and advice in a more concise and Java-friendly manner.

