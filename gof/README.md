# GoF

## 说明

GoF 23 种设计模式

## 三大模式
-   [Creational patterns](./creational)
-   [Structural patterns](./structural)
-   [Behavioral patterns](./behavioural)

## 设计模板的七大原则

[面向对象的设计模式有七大基本原则：](https://cloud.tencent.com/developer/article/1650116)

-   合成/聚合复用原则（Composite/Aggregate Reuse Principle，CARP）

    尽量使用合成/聚合，而不是通过继承达到复用的目的。

-   依赖倒转原则（Dependency Inversion Principle，DIP）

    依赖于抽象，不能依赖于具体实现。

    很多模式都抽象出一个抽象基类。而客户一般只使用这个抽象基类。

    ”讲了这么多，估计大家对“倒置”这个词还是有点不理解，那到底什么是“倒置”呢？我们先说“正置”是什么意思，依赖正置就是类间的依赖是实实在在的实现类间的依赖，也就是面向实现编程，这也是正常人的思维方式，我要开车就依赖奔驰车，我要使用电脑就直接依赖笔记本电脑，而编写程序需要的是对现实世界的事物进行抽象，抽象的结果就是有了抽象类和接口，然后我们根据系统设计的需要产生了抽象间的依赖，代替了人们传统思维中的事物间的依赖，“倒置”就是从这里产生“。

-   开闭原则（Open Closed Principle，OCP）

    对扩展开放，对修改关闭。

    定义是：一个软件实体如类、模块和函数应该对扩展开放，对修改关闭。模块应尽量在不修改原（是"原"，指原来的代码）代码的情况下进行扩展。

-   里氏代换原则（Liskov Substitution Principle，LSP）

    所有引用基类的地方必须能透明地使用其子类的对象。

    定义是：所有引用基类的地方必须能透明地使用其子类的对象，也可以简单理解为任何基类可以出现的地方，子类一定可以出现。（《Effective C++》中的“条款 36: 绝不重新定义继承而来的 non-virtual 函数” ，因为这样会破坏 is-a 的关系。）

    里氏代换原则是对"开-闭"原则的补充。

-   单一职责原则（Single Responsibility Principle, SRP）

    一个类只负责一个功能领域中的相应职责。

-   接口隔离原则（Interface Segregation Principle，ISP）

    类之间的依赖关系应该建立在最小的接口上。

    当一个接口过于臃肿时，可以接口拆分为多个接口。然后用这个小接口进行灵活地组合。

    如何看待接口隔离原则和单一职责原则：

    > 单一职责原则是从职责角度出发，而接口隔离是从接口角度出发。有可能满足单一职责原则但是不满足接口隔离原则。

-   迪米特法则（Law of  Demeter，LOD），也叫最少知识原则（Least Knowledge Principle，LKP）

    一个软件实体应当尽可能少的与其他实体发生相互作用。知道得越少越好。

    迪米特法则的初衷在于降低类之间的耦合。由于每个类尽量减少对其他类的依赖，因此，很容易使得系统的功能模块功能独立，相互之间不存在（或很少有）依赖关系。迪米特法则不希望类之间建立直接的联系。如果真的有需要建立联系，也希望能通过它的友元类（中间类或者跳转类）来转达。比如：中介者模式。

个人对于原则的关联性划分：

-   开闭原则、里氏代换原则: 强调扩展，排斥修改或覆盖。
-   合成/聚合复用原则: 尽量用组合而不是继承。这样可以减少耦合。
-   依赖倒转原则: 可以做到不直接关联具体实现。这样可以减少耦合。与"迪米特法则"有共通点, 不知道具体类。
-   单一职责原则、接口隔离原则、迪米特法则: 强调单一(从职责/功能/代码的角度来说)，排斥耦合。

## References

-   [设计模式](https://www.runoob.com/design-pattern/design-pattern-tutorial.html): 有不错的例子
-   [设计模式 (计算机)](https://zh.wikipedia.org/wiki/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F_(%E8%AE%A1%E7%AE%97%E6%9C%BA))
-   《设计模式：可复用面向对象软件的基础》
-   《Head First 设计模式》
-   [JakubVojvoda/design-patterns-cpp](https://github.com/JakubVojvoda/design-patterns-cpp)
-   [图说设计模式](https://design-patterns.readthedocs.io/zh_CN/latest/index.html)
