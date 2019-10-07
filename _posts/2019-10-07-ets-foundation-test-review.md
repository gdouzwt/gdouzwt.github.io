---
layout:     post
title:      EST 基础测试回顾
date:       '2019-10-07 11:26:02'
subtitle:   整理错题，查漏补缺
author:     招文桃
catalog:    true
tags:
    - Java
    - OCA
---

###  基础测试的情况

当时第一次测试结果报告如下：

![foundation-test](/img/foundation-test.png)

按照考试的主题（或者说考点）来看的话，Java Basics 正确率 6/7。然后，OO Concepts 过关的，Java Data Types 部分就只对了 3 题，有点危险。基础测试没有涉及到垃圾回收的内容，但是真正考试应该会有的，而且现在工作要求也肯定会问，需要了解的。关于操作符和条件判断结构居然也只有对了 6 个题，看起来真的有点 tricky。接下来的数组、循环结构和构造方法这些考点感觉还过得去的样子。 关于方法的使用居然有点不稳，要搞清楚了，可能需要翻译 coderanch 的文章加深一下理解。这次没有设计方法重载的题目，然而关于继承的理解，可能还不够深，要看看编程思想了。`instanceof` 没有考到，异常处理需要加强。最基本的 `String`， 以及相关的类要烂熟了，至少 API 文档过一遍。最后 Java 8 新的时间日期 API 还没有了解，这一次就新的旧的都看一遍吧，所以今天是要看完错题，找出对应的知识点，考点，然后读 API 文档。

### 接下来做什么

#### 整理错题

1. Compared to public, protected, and private accessibilities, default accessibility is ... **(Working with Methods)**

   - [ ] Less restrictive than `public`  肯定错啦，`public` 最宽了。
   - [ ] More restrictive than `public`, but less restrictive than `protected`. 想到 "by default is protect"，默认情况下，就相当于保护了，但是更深一层限制在同一个包里。所以就 `default` < `protected` 所以下一个选项是正确的。
   - [x] More restrictive than `protected`, but less restrictive than `private`.
   - [ ] More restrictive than `private`. 肯定错， `private` 最窄了。
   - [ ] Less restrictive than `protected` from within a package, and more restrictive than `protected` from outside a package. 当时选择了这个选项，因为看起来好像很有道理，而且描述全面。但现在仔细看看，说的是，在同一个包内，访问权限比 `protected` 更受限，这已经错了，同一个包内两个一样的。后面部分就不用看了，当时就没多想，这次知道了，下次就不会错了。

2. What can be the type of a `catch` argument? (Select the best option.) **(Handling Exceptions)**

   - [ ] Any class that extends `java.lang.Exception`
   - [ ] Any class that extends `java.lang.Exception` except any class that extends `java.lang.RuntimeException`
   - [x] Any class that is-a `Throwable`. 这个是正确答案，解释如下

   > The catch argument type declares the type of exception that the handler can handle and must be the name of a class that extends `Throwable` or `Throwable` itself.

   - [ ] Any Object
   - [ ] Any class that extends `Error`

3. 记录一个关于重写的： An overriding method must have a same parameter list and the same return type as that of the overridden method. 翻译成中文就是，重写方法必须与被重写方法具有相同的参数列表和返回类型。

   - [ ] True
   - [x] False 我答对了，不过好像没有理解正确。 解释是这样说的：

   > This would have been true prior to Java 1.5. But from Java 1.5, an overriding method is allowed to change the return type to any subclass of the original return type, also known as **covariant return type**. This does not apply to primitives, in which case, the return type of the overriding method must match exactly to the return type of the overridden method.
   >
   > 所以主要看返回值类型，重写必须参数列表相同才算重写，不然就是重载了。而在 Java 1.5 之前，重写方法的返回值必须与被重写方法返回值一致。不过从 Java 1.5 开始，重写方法的返回值类型可以是被重写方法返回值的任意子类，也称为 covariant return type（好像中文译为“**协变返回类型**”）

4. Which of the following statements are true?**(Working with Inheritance)** 选择两个正确的，当时选错了。

   - [x] Private methods cannot be overridden in subclasses.

      > Only methods that are inherited can be overridden and private methods are not inherited. 只有被继承的方法才能被重写，私有方法不会被继承。

   - [ ] A subclass can override any method in a non-final superclass.

     > Only the methods that are not declared to be final can be overridden. Further, private methods are not inherited so they cannot be overridden either.

   - [ ] An overriding method can declare that it throws a wider spectrum of checked exceptions than the method it is overriding.

     > 这个没有在选项给出解释，看底下更长的解释。

   - [ ] The parameter list of an overriding method must be a subset of the parameter list of the method that it is overriding.

     > An overriding method (the method that is trying to override the base class's method) must have the same parameters.

   - [x] The overriding method may opt not to declare any throws clause even if the original method has a throws clause.

     > No exception(i.e. an empty set of exceptions) is a valid subset of the set of exceptions thrown by the original method so an overriding method can choose to not have any throws clause.

   A method can be overridden by defining a method with the same signature(i.e. name and parameter list) and return type as the method in a superclass. The return type can also be a subclass of the original method's return type.
   
   Only methods that are accessible can be overridden. A private method cannot, therefore, be overridden in subclasses, but the subclasses are allowed to define a new method with exactly the same signature.
   
   A final method cannot be overridden.
   
   An overriding method cannot exhibit behavior that contradicts the declaration of the original method. An overriding method therefore cannot return a different type (except a subtype) or throw a wider spectrum of exceptions than the original method in the superclass. This, of course, applies only to checked exceptions because unchecked exceptions are not required to be declared at all.
   
   A subclass may have a static method with the same signature as a static method in the base class but it is not called overriding. It is called hiding because **the concept of polymorphism doesn't apply to static members.**
   
   这部分这有点意思，之前没有了解到。关于方法重写和重载，还有静态成员的知识。对于静态方法，子类中具有与父类相同方法签名的静态方法，不叫重写，叫隐藏，因为多态的概念不适用于静态成员。
   
5. Given: **(Working with Methods)**

   ```java
   //In file AccessTest.java
   package a;
   public class AccessTest {
       int a;
       private int b;
       protected void c() {}
       public int d() {	return 0; }
   }
   
   //In file AccessTester.java
   package b;
   import a.AccessTest;
   
   public class AccessTester extends AccessTest {
       public static void main(String[] args) {
           AccessTest ref = new AccessTest();
       }
   }
   ```

   Identify the correct statements -

   - [ ] Only `c()` and `b()` can be accessed by `ref`.
   - [ ] `b`, `c()` as well as `d()`, can be accessed by `ref`.
   - [x] Only `d()` can be accessed by `ref`.
   - [ ] Only a and `d()` can be accessed by `ref`.

   The wording of this question is a bit vague because it is not clear what is meant by "can be accessed by". Expect such wording in the real exam as well. Our guess is that it means what variables of class `AceesssTest` can be accessed using the reference named `ref`.
   
   Since a public member is always accessible to every one, `ref.d()` is definitely correct. private is only accessible within that class, therefore, b cannot be accessed from anywhere outside of class `AccessTest`. A default (aka package protected) member is accessible only from members of the same package. Since `AccessTester` is in a different package `a` cannot be accessed from `AccessTester` either.
   
   Now, the question is only about the method `c()`. A protected member is inherited by a subclass and it is therefore accessible in the subclass. However, in the words of Java Language Specification, protected members of a class are accessible outside the package only in subclasses of that class, and only when they are fields of objects that are being implemented by the code that is accessing them.
   
   Basically, it implies that a protected member is accessible in the subclass only using a reference whose declared type is of the same subclass (or its subclass.).
   
   In this case, the declared type of `ref` is `AccessTest`, which is not of the same type as the class from which you are trying to access `c()`. Therefore, you cannot do `ref.c()` in `AccessTester`. If you had `AccessTester ref = new AccessTester();` you could do `ref.c()` because now the declared type of `ref`(i.e. `AccessTester`) is the same subclass from which you are trying to access `c()`. It will work even if the declared type of the reference is a child of the subclass. For example, the following would be valid as well.
   
   ​            `SubAccessTester ref = new SubAccessTester();`
   
   ​            `ref.c();   // this is valid`
   
   Where `SubAccessTester` is a subclass of `AccessTester` - 
   
   `class SubAccessTester extends AccessTester {}`
   
6. Which of these statements concerning the use of modifiers are true? **(Java Basics)**

   - [ ] By default (i.e. no modifier) the member is only accessible to classes in the same package and subclasses of the class.

     > No. the member will be accessible only within the package.

   - [x] You cannot specify visibility of local variables.

     > They are always only accessible within the block in which they are declared.

   - [ ] Local variable always have default accessibility.

     > A local variable (aka automatic variable) means a variable declared in a method. They don't have any accessibility. They are accessible only from the block they are declared in.
     >
     > **Remember**, they are not initialized automatically. You have to initialize them explicitly.

   - [ ] Local variables can be declared as private.

   - [ ] Local variables can only by declared as public.

   You cannot apply any modifier except `final` to a local variable. i.e. you cannot make them `transient`, `volatile`, `static`, `public`, and `private`.

   But you can apply access modifier (`public` `private` and `protected`) and `final`, `transient`, `volatile`, `static` to instance variables.

   You cannot apply `native` and `synchronized` to any kind of variable.

   这些很经典的基础，关键词的运用，必须熟记。

7. An abstract method cannot be overridden. **(Working with inheritance)**

   - [ ] True
   - [x] False

   > Abstract methods are meant to be overridden in the subclass. Abstract methods describe a behavior but do not implement it. So the subclasses have to override it to actually implement the behavior. A subclass may chose to override it, in which case, the subclass will have to be abstract too.

8. What is the correct declaration for an abstract method 'add' in a class that is accessible to any class, takes no arguments and returns nothing? (You had to select 1 option)

   - [ ] `public void add();`

     > An abstract method must have the `abstract` keyword and must not have a method body i.e. {}.

   - [ ] `abstract add();`

     > A method that is not supposed to return anything must specify `void` as its return type.

   - [ ] `abstract null add();`

     > A method that is not supposed to return anything must specify `void` as its return type. `null` is not a type, though it is a valid return value for any reference type.

   - [ ] `abstract public void add();`

     > It is invalid because has a method body i.e. {}.  有方法体都不可以。

   - [x] abstract public void add() throws Exception;

9. Which of the following are correct about `java.util.function.Predicate` ?

   - [ ] It is an interface that has only one abstract method (among other non-abstract methods) with the signature - `public void test(T t);`
   - [x] It is an interface class that has only one abstract method (among other non-abstract methods) with the signature - `public boolean test(T t);`
   - [ ] It is an abstract class that has only abstract method (among other non-abstract methods) with the signature - `public abstract void test(T t);`
   - [ ] It is an abstract class that has only on abstract method (among other non-abstract methods) with the signature - `public abstract boolean test(T t);`

   > `java.util.function.Predicate` is one of the several functional interfaces that have been added to Java 8. This interface has exactly one abstract method named `test`, which takes any object as input and returns a `boolean`. This comes in very handy when you have a collection of objects and you want to go through each object of that collection and see if that object satisfies some criteria. For example, you may have a collection of `Employee` objects and, in one place of your application, you want to remove all such employees whose age is below 50, while in other place, you want to remove all such employees whose salary is above 100,000. In both the cases, you want to go through your collection of employees, and check each `Employee` object to determine if it fits the criteria. This can be implemented by writing an interface named `CheckEmployee` and having a method `check(Employee)` which would return `true` if the passed object satisfies the criteria. The following code fragments illustrate how it can be done - 

   ```java
   //define the interface for creating criteria
   interface CheckEmployee {
   	boolean check(Employee e);
   }
   
   ...
       
   //write a method that filters Employees based on given criteria.
   public void filterEmployees(ArrayList<Employee> dataList, CheckEmployee p) {
       Iterator<Employee> i = dataList.iterator();
       while(i.hasNext()) {
           if(p.check(i.next())) {
               i.remove();
           }
       }
   } 
   
   ...
       
   //create a specific criteria by defining a class that implements CheckEmployee
   class MyCheckEmployee implements CheckEmployee {
       public boolean check(Employee e) {
           return e.getSalary()>100000;
       }
   }    
   
   ...
       
   //use the filter method with the specific criteria to filter the collection.
   filterEmployees(employeeList, new MyCheckEmployee());   
   
   ```

   > This is a very common requirement across applications. The purpose of `Predicate` interface (and other standard functional interfaces) is to eliminate the need for every application to write a customized interface. For example, you can do the same thing with the `Predicate` interface as follows - 

   ```java
   public void filterEmployees(ArrayList<Employee dataList, Predicate<Employee> p) {
       Iterator<Employee> i = dataList.iterator();
       while(i.hasNext()) {
           if(p.test(i.next())) {
               i.remove();
           }
       }
   }
   
   ...
       
   // Instead of defining a MyPredicate class (like we did with MyCheckEmployee), we could also define and instantiate an anonymous inner class to reduce code clutter
   Predicate<Employee> p = new Predicate<Employee>() {
       public boolean test(Employee e) {
           return e.getSalary()>1000000;
       }
   }    
   ...
       
   filterEmployees(employeeList, p);
   ```

   > Note that both the interfaces (`CheckEmployee` and `Predicate`) can be used with lambda expressions in exactly the same way. Instead of creating an anonymous inner class that implements the `CheckEmployee` or `Predicate` interface, you could just do - 
   >
   > `filterEmployees(employeeList, e -> e.getSalary()>1000000);`
   >
   > The benefit with `Predicate` is that you don't have to write it, It is already there in the standard java library.
   
10. Which of the following are valid declarations in a class? (You had to select 1 option) **(Working with Inheritance)**

    - [ ] abstract int absMethod(int param) throws Exception;

    - [ ] abstract native int absMethod(int param) throws Exception;

      > `native` method cannot be `abstract`

    - [ ] float native getVariance() throws Exception;

      > return type should always be on the immediate left of method name.

    - [ ] abstract private int absMethod(int param) throws Exception;

      > `private` method cannot be `abstract`. A `private` method is not inherited so how can a subclass implement it?

11. Which of the following statements is/are true? (You had to select 1 option)

    - [ ] Subclasses must define all the abstract methods that the superclass defines.

      > Not if the subclass is also defined abstract!

    - [ ] A class implementing an interface must define all the methods of that interface.

      > Not if the class is defined abstract. Further, Java 8 allows interface to have and static methods, which need not be implemented by a non-abstract class that says it implements that interface.

    - [x] A class cannot override the super class's constructor.

      > Because constructors are not inherited.

    - [ ] It is possible for two classes to be the superclass of each other.

    - [ ] An interface can implement multiple interfaces.

      > Interface cannot "implement" another interfaces. It can extend multiple interfaces. The following is a valid declaration `interface I1 extends I2, I3, I4 {}`。 **记得了，接口是不可以实现其它接口的，但是可以实现多个接口。**

12. Which of the following statements regarding 'break' and 'continue' are true? (You had to select 1 option) **(Using Loop Constructs)** 这题选错了，注意。

    - [x] `break` without a label, can occur only in a `switch`, `while`, `do`, or `for` statement.

    - [ ] `continue` without a label, can occur only in a `switch`, `while`, `do`, or `for` statement.

      > It cannot occur in a switch.

    - [ ] `break` can never occur without a label.

    - [ ] `continue` can never occur WITH a label.

    - [ ] None of the above.

    A break statement with no label attempts to transfer control to the innermost enclosing switch, while, do, or for statement; this statement, which is called the break target, then immediately completes normally. If no switch, while, do, or for statement encloses the break statement, a compile-time error occurs.

    A break statement with label Identifier attempts to transfer control to the enclosing labeled statement  that has the same Identifier as its label; this statement, which is called the break target, then immediately completes normally. In this case, the break target need not be a while, do, for, or switch statement.

    A continue statement with no label attempts to transfer control to the innermost enclosing while, do, or for statement; this statement, which is called the continue target, then immediately ends the current iteration and begins a new one. If no while, do, or for statement encloses the continue statement, a compile-time error occurs.

    A continue statement with label Identifier attempts to transfer control to the enclosing labelled statement that has the same Identifier as its label; that statement, which is called the continue target, then immediately ends the current iteration and begins a new one. The continue target must be a while, do, or for statement or a compile-time error occurs. If no labelled statement with Identifier as its label contains the continue statement, a compile-time error occurs.

13. What class of objects can be declared by the throws clause? **(Handling Exceptions)**

    - [x] `Exception`
    - [x] `Error`
    - [ ] `Event`
    - [ ] `Object`
    - [x] `RuntimeException`

    You can declare anything that is a `Throwable` or a subclass of `Throwable`, in the `throws` clause.

14. Identify the valid members of Boolean class.

    - [ ] `parseBoolean(String)`

    - [ ] `valueOf(boolean)`

    - [ ] `parseBoolean(boolean)`

    - [ ] `FALSE`

      > `TRUE` and `FALSE` are valid static members of `Boolean` class.

    - [ ] `Boolean(Boolean)`

      > There is no constructor that takes a `Boolean`.

    **You need to remember the following points about Boolean:**

    1. `Boolean` class has two constructors - `Boolean(String)` and `Boolean(boolean)`

       The `String` constructor allocates a `Boolean` object representing the value `true` if string argument is not null and is equal, ignoring case, to the string "true". Otherwise, allocate a Boolean object representing the value `false`. Examples: `new Boolean("True")` produces a `Boolean` object that represents `true`. `new Boolean("yes")` produces a `Boolean` object that represents `false`.

       The `boolean` constructor is self explanatory.

    2. `Boolean` class has two static helper methods for creating booleans - `parseBoolean` and `valueOf`.

       `Boolean.parseBoolean(String )` method returns a primitive boolean and not a `Boolean` object (Note - Same is with the case with other `parseXXX` methods such as `Integer.parseInt` - they return primitives and not objects). The boolean returned represents the value `true` if the string argument is not null and is equal, ignoring case, to the string "true".

       `Boolean.valueOf(String )` and its overloaded `Boolean.valueOf(boolean )` version, on the other hand, work similarly but return a reference to either `Boolean.TRUE` or `Boolean.FALSE` wrapper objects. Observe that they don't create a new Boolean object but just return the static constants `TRUE` or `FALSE` defined in `Boolean` class.

    3. When you use the equality operator ( == ) with booleans, if exactly one of the operands is a `Boolean` wrapper, it is first unboxed into a `boolean` primitive and then the two are compared (JLS 15.21.2). If both are `Boolean` wrappers, then their references are compared just like in the case of other objects. Thus, `new Boolean("true") == new Boolean("true")` is `false`, but `new Boolean("true") == Boolean.parseBoolean("true")` is `true`.

#### 读 API 文档

##### String

##### StringBuilder

##### StringBuffer
