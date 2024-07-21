# Behavioural patterns（行为模式）

行为型模式(Behavioral Pattern): 是对在不同的对象之间划分责任和算法的抽象化。

## Strategy（策略）

意图:
> 定义一个算法的系列，将其各个分装，并且使他们有交互性。策略模式使得算法在用户使用的时候能独立的改变。

![](http://www.plantuml.com/plantuml/png/ZP11oi8m58JtSuf7Ll-Ff0Ue80MFu0b2VMq3RPuaJr3Kkzjg8WKtPfKPvfi9QPAwgETf17nGZfrhcGuQdN9_fHjeFXjoOo_Hwp3z_UC1jADBYVOIsiZAFwULBvf3bbAcCYCddhMNy6Q-kglglgEYyB6j5JAsT9co0WHHfkZxGKcwOjUrMUsOrtHXgzMhj-1mfAK2QERhyZrF)

PlantUML Code:

```plantuml
@startuml

interface Strategy {
    + algorithm()
}

class ConcreteStrategy {
    + algorithm()
}

class Context {
    - strategy
    + operation()
}

ConcreteStrategy ..|> Strategy
Strategy --o "-strategy" Context

note right of Context::"operation()"
    strategy.algorithm();
end note

@enduml
```

## Template method（模板方法）

意图:
> 定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。

![](http://www.plantuml.com/plantuml/png/SoWkIImgAStDuU9AJ2ekAKfCBb58paaiBbPmX7ATmRngBWKWq5OeISqjo4aiIVLDBSd8Jz7GX0eN56NcPfPabgNw5wGM9PPavkSvQcWgLCEChCMfp0bLMIqN5yHsv_oyvABKabIexVYimMSso41KetHrQ-nG_SR5bPTVaggGavfMef2VXYfdPQM4xcCbi7tw-We9w3892a-tRtgwRjxplWtlz_IyQEXvDgVpoQxPpwRjVBPvwejbZSysDZrTE-7vnjqGDOyRcc16wUdfWPZO_MTDM9KJcghKl1G5aAUuk1o0J53i0W00)

PlantUML Code:

```plantuml
@startuml

abstract class AbstractClass {
    + templateMethod()
    + primitiveOperationA() {abstract}
    + primitiveOperationB() {abstract}
}

class ConcreteClass {
    + primitiveOperationA()
    + primitiveOperationB()
}

ConcreteClass --|> AbstractClass

note left of AbstractClass::"templateMethod()"
    // `templateMethod()` 已实现，而有些方法要求子类实现。
    // ...
    primitiveOperationA();
    // ...
    primitiveOperationB();
    // ...
end note

@enduml
```

## Chain of responsibility（责任链）

意图:
> 避免请求发送者与接收者耦合在一起，让多个对象都有可能接收请求，将这些对象连接成一条链，并且沿着这条链传递请求，直到有对象处理它为止。

![](http://www.plantuml.com/plantuml/png/RO_1ReCm44Jl-nKZJgqg_04eGf5wwRb_u3fBW-JOgdTzelnxWmDkHB8dc9qPlpsAsgJvuib-YIRh5CvR4NpOSFASC16kqqAoSomI4xfjLpPlE9U_J_x9BFhoYcbhccackhMzn-0IAzVMvzcxW1yvAAP5sOUD-UqhmoOsRILiqBQn6jOcOse67Gw7BDptH24gm_EWYCEUikkQ7LzJiCS1peQLVJrbcPsvw3FOoxsfKfgdTk9mmWAregNn-rpORcVG_pkF1JLwl7xbY_y3)

PlantUML Code:

```plantuml
@startuml

abstract class Handler {
    - successor : Handler
    + handleRequest() {abstract}
}

class ConcreteHandler {
    + handleRequest()
}

class Client {
}

ConcreteHandler ..|> Handler
Handler "-successor" --o Handler
Client ..> Handler : <<use>>

note left of ConcreteHandler::"handleRequest()"
    if can handle {
        handleRequest()
    } else {
        successor.handleRequest()
    }
end note

note right of Client
    handler.handleRequest()
end note

@enduml
```

## Command（命令）

意图:
> 将一个请求封装成一个对象，从而使您可以用不同的请求对客户进行参数化。
>
> *请求最终会调用接收者具体实现的功能，而调用者只需调用设置好的请求即可。*

![](http://www.plantuml.com/plantuml/png/VL9DRzD05BplhrZvbAea5Hm3L9KuSUq_SCrhiV27P6yA4bGABHKfgbfLL800WK2KZujRBWsGf7-6hErR_WBUx8xJs40EiVtslPbvCxiI2piI7TzZp5wBHMGxWZkU7SVyLkZxTd27FsIy-3LvH0wvcnJnDbyrhzEJHUxepiPVEXPC2pqWfoEeiS2s60D-u4GagEIP7TqEDiDx584Q1BmDKGOr9c4AZLfBXHbzCm7G29f5NkpkXi6SHi-bG6XfLRtDvgFbKi-i67AhQSHOM8I1ofp3AAkSPq4eY7kqBdYuZcgxRPM-MLED66n1wdL61QiQha20MKt1JjSyR_A0xgEgC5JJqXANoSUqry-JYsoKo9DHzle9d9yCU_bfF_s-FHdkgMv0jUe9Q3YWjwhs6xF1U7Wad_xIqAY34wCGk5LTDN8txgfTJKhOGklNcCKAwS59UdoUTtUdvmW2Ji8Cr45cz4ABwQfSWGP0vcBmQ4Dzh8L2X3v4ZNyCyi-FipU9KlJgD4--bQcaRxlPe9UVxMJ7NoydeEnVhUPT126VlSztL3XojpyUFPV9k-pW3BbSRV7v68z6uy6dopPS-STkDqnvURRL5F5QWQZ55SFoy8MyU5c_9vfcCjoNFy_JlI_vX_rwYh-xs_zPjTHNk-DgKDchVMLifVZl-DuV)

PlantUML Code:

```plantuml
@startuml

abstract class Command {
    - receiver : Receiver
    + command(receiver : Receiver)
    + execute()
}

class ConcreteCommand {
}

class Invoker {
    - command : Command
    + setCommand(command : Command)
    + executeCommand()
}

class Receiver {
    + action()
}

class Client {
}

ConcreteCommand --|> Command
Command --o "-command" Invoker
Receiver --o "-receiver" Command

note left of Command::"execute()"
    receiver.action();
end note

note left of Invoker
    Invoker invoker = new Invoker();
    // 客户设置好的 command（请求）
    invoker.setCommand(command);
    Invoker.executeCommand() {
        // 发出请求
        command.execute();
    }
end note

note right of Client
    // 设置请求的接收者
    Receiver receiver = new Receiver();
    Command command = new ConcreteCommand(receiver);
end note

legend bottom
    // 主要目的是让 Invoker 最终调用客户设置的 Receiver.action()（功能的具体实现）
    1.  客户设置命令的接收者, Command command = new <Command>(receiver)
    2.  调用者取得客户设置好的命令，并执行 command.execute()。最终调用客户设置的接收者 receiver.action()。
endlegend

@enduml
```

## Interpreter（解释器）

意图:
> 给定一个语言，定义它的文法表示，并定义一个解释器，这个解释器使用该标识来解释语言中的句子。

![](http://www.plantuml.com/plantuml/png/d53DIWCn4BxFKmmvMQGfNbSeGl7goHT8jxCQo6OaCuM2-kvEIpGBHa7dj3lVptnVxaH3qUES0COKH737MUca-0hl0D6-onH6mllJIo6HoDaGjBd62sXRlHghPlXKhqnS_Hwfp367z6-31yu_UgoHlbPYwaRuqubTYfHhvSujxz-sI-jEeWvh0RbrY-cCoFrIK7DulyN-jaO7oAo4YIP5dlfcm-1-A-y0RJORF33AST_ojIUxC1hWlzcjRXSc-aneJ6rwYsRRAFry7YWVyDm38D7J-MVFgZm3sjpuZy4JujEUrgSJLhzOllXbUzVJcIkUxEn-kcJQyrajJtOqFDar-sdhYgSR6vxiN_YiSVtZXYQmPYCzM8o-sD3yVCeAYDvdatkVx9q3L0Eo668Z5vS3a0GcVW00)

PlantUML Code:

```plantuml
@startuml

class Context {
}

interface Expression {
    + interpret(context : Context)
}

class TerminalExpression {
    + interpret(context : Context)
}

class NonTerminalExpression {
    - expressions : Expression
    + interpret(context : Context)
}

class Client {
}

TerminalExpression ..|> Expression
NonTerminalExpression ..|> Expression
Expression "-expression" ..o NonTerminalExpression
Client ..> Expression
Client ..> Context

note left of NonTerminalExpression::"interpret(context : Context)"
    // do subexpression interpret
    expression.interpret(context);
    // do the rest interpret of this NonTerminalExpression.
end note

legend bottom
    1. Context 包含解释器之外的一些全局信息。
    2. Client 调用解释操作。
endlegend

@enduml
```

## Iterator（迭代器）

意图:
> 提供一种方法顺序访问一个聚合对象中各个元素, 而又无须暴露该对象的内部表示。

![](http://www.plantuml.com/plantuml/png/ZLBDJiCm3BxdAQoTMg5bzsvKcpWX8TuXAruNgKkbn1KWsBiJawwUcWcMKn9_F_QNR0CPJyEfKqrdGe1dmXDygRDIrX7wWscGxxoXtiTxYEi1ZYQyuWSLvNXswH19IUIfTur7mXbn2JQg1wZWnGRQi5MT5396OThMOsi88tftsPTp_rZSzts7naadwPh5kI6p3-HDGv0wcwIcMQAj4T_4dTg-iC_vRA9qxt12ASh_pKT4YyHIWMlNoj9FPz5HUh8iTws_Qr7CMrykOtqwAYbeNKEcLi5capgkQwL6OqOAZo53uBgK9K-fAjT7T8S7WlwG1rHLYnkXBHJ4HKSRTChw4Ho-myvxyodH5ELQeNi3ThZ3P_u4oIIY1k_oRydcvT_oWexgz_thMvDDG2rVO6xiRNlyTKvXiuY8YiAOqur4rqoHzx6NpRNzFA34MQq4hRMMphPnDvow7m00)

PlantUML Code:

```plantuml
@startuml

class Item {
}

interface Iterator {
    + hasNext()
    + next()
}

class ConcreteIterator {
    - items : List<Item>
    + ConcreteIterator(aggregate : Aggregate)
    + hasNext() : boolean
    + next() : Item
}

abstract class Aggregate {
    + createIterator() : Iterator {abstract}
}

class ConcreteAggregate {
    - items : List<Item>
    + ConcreteAggregate()
    + createIterator() : Iterator
    + getItems() : List<Item>
}

class Client {
}

ConcreteIterator ..|> Iterator
ConcreteAggregate --|> Aggregate
Aggregate ..> ConcreteIterator : <<create>>

Client ..> Aggregate : <<use>>
Client ..> Iterator : <<use>>

note left of ConcreteAggregate::"ConcreteAggregate()"
    this.items = new ArrayList<Item>();
end note

note left of ConcreteAggregate::"createIterator()"
    return ConcreteIterator(this)
end note

note left of ConcreteIterator::"ConcreteIterator(aggregate : Aggregate)"
    this.items = aggregate.getItems()
end note

note right of Client
    Aggregate aggregate = new ConcreteAggregate();
    Iterator iterator = aggregate.createIterator();
    // iterator ...
end note

@enduml
```

## Mediator（中介者）

意图:
> 用一个中介对象来封装一系列的对象交互，中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。
>
> Colleagues 都聚合 Mediator，Mediator 也都聚合 Colleagues 即可。这样 Colleagues 之间不用相互聚合。

![](http://www.plantuml.com/plantuml/png/ZLJ1IiD04BtlLmmvjSXkQOuHfE1P1Fs2DPtKG9f0CZtLWZUlwj6JP_q3yNCKwb-u2PbTsjs4vZJBl3TltjibYuPqJPjr8OI-QwZMAK5QwAQ1oweeKAys25i1vfEmncMkgQeXWQk-x3fdi4Aw9KqucSAMn-pwdYdpgZpix8HWaJAjaT2ApM7hpJmQDCAEJsFd9M6TwHIj3-Rr7d6IMlU9Io8WVJH0GkexIW8sXz1n21sVl5mWJoaVBXpAHyG326TDATFda-028iaF8W8fgp6DkG7xmJ3jK2wmOOWT3x15MH52WHb1bNdj98d6GuowgFCCN5-vjpJjhxdzPZDGIjcZRq_P9yUOwvjXU6pXvnre9x1SGcjcm98JCgQO6mct6fS_ts-_pozWVBS9cEFxt-Fh_kbZ__NPGVWM5IT3ztbZjd3w2rEsluX_)

PlantUML Code:

```plantuml
@startuml

abstract class Colleague {
    - mediator : Mediator
    + getState() {abstract}
    + action() {abstract}
}

class ConcreteColleague1 {
    + getState()
    + action()
}

class ConcreteColleague2 {
    + getState()
    + action()
}

interface Mediator {
    + mediate(colleague : Colleague)
}

class ConcreteMediator {
    - concreteColleague1 : ConcreteColleague1
    - concreteColleague2 : ConcreteColleague2
    + mediate(colleague : Colleague)
}

ConcreteColleague1 --|> Colleague
ConcreteColleague2 --|> Colleague
ConcreteMediator ..|> Mediator
Mediator "-mediator" --o Colleague
ConcreteColleague1 "-concreteColleague1" --o ConcreteMediator
ConcreteColleague2 "-concreteColleague2" --o ConcreteMediator

note left of ConcreteMediator::"mediate(colleague : Colleague)"
    if (colleague.getState()) {
        // ...
        concreteColleague1.action()
        OR
        concreteColleague2.action();
    }
end note

note left of ConcreteColleague1::"action()"
    // ...
    // 会向 medator 传递自身
    mediator.mediate(this);
end note

@enduml
```

## Memento（备忘录）

意图:
> 在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。

![](http://www.plantuml.com/plantuml/png/VP9DQiCm48NtSug75oMXs0TmKnFekdJH4uXq4a9j2QGnNPJUlV98Gg8_sOtyllbcBFiOn7XPbvdeD9iGyCmBMt7u903e4NDXhUznONdTZhizHcXLe18eNS8zVHNhYxzUhjQ8yt-AJvxZ8OzMUwvpxZd4LjujwMdDcn5FnhEOTwuJVj4Rd4jqVOuxzBshtTIiEWmJ2Z_YS4XhuWvhu6cYqfF0sgTiGiWwOSny5hXpWunZz-ETErqw2bTlOVa39GbwbG_4zWsRxPRpttlAUdNX4JaVwWTj_STORd_4Dm00)

PlantUML Code:

```plantuml
@startuml

class Memento {
    - state
    + getState() : State
    - setState(state : State)
}

class Originator {
    - state
    + createMemento() : Memento
    + restore(memento : Memento)
}

class Caretaker {
    - memento : Memento
}

Memento "-memento" --o Caretaker
Originator ..> Memento : <<create & use>>
Caretaker ..> Originator : <<use>>

note left of Originator::"createMemento()"
    return new Memento(state);
end note

note left of Originator::"restore(memento : Memento)"
    state = memento.getState();
end note

@enduml
```

## Observer（观察者）

意图:
> 定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。

![](http://www.plantuml.com/plantuml/png/XP9HQiGW48RVFSMGfombEK2WPQ47wCEUm2HZcsArwAHGjdltZcIKu0RM5yNvyytylpb7qe7MBwlKuWY3qHF2snWn_620gm9UJx1-pvgmFQcRKfFLCSAhTrD0mahQGLp7Jvm81hXi9xdt8hmGar8rxGTuFKOAcW5R7u70jS946CgOGj54Ulfeis8dE8bYnaSAvsanlqT6wq74vv6TTzoksqoDvI9nxwBh-xyNA6RgXbt7rPnb-QRfIX8DItnHoCu2cN0hrqsLOgi85Ws1DtVbOMZoepJ9HFLypzb-l_EAReO4hT0o41CbN24Q7q1R2nuML_0nvIlBzsc44kDjr-3Cn_JF_WC0)

PlantUML Code:

```plantuml
@startuml

interface Observer {
    + update()
}

class ConcreteObserver {
    - subject : Subject
    - observerState : State
    + update()
}

abstract class Subject {
    - observers : Observer
    + attach(observer : Observer)
    + detach(observer : Observer)
    + notify()
}

class ConcreteSubject {
    - subjectState : State
    + getState()
    + setState()
}

ConcreteObserver ..|> Observer
ConcreteSubject --|> Subject
Observer "-observers" --o Subject
Subject "-subject" --o ConcreteObserver

note left of ConcreteObserver::"update()"
    observerState = subject.getState()
end note

note left of Subject::"notify()"
    for all o in observers {
        o.update()
    }
end note

@enduml
```

## State（状态）

意图:
> 让一个对象在其内部状态改变的时候，其行为也随之改变。状态模式需要对每一个系统可能获取的状态创立一个状态类的子类。当系统的状态变化时，系统便改变所选的子类。

![](http://www.plantuml.com/plantuml/png/bLFDJi904BxlKqnweZ4jqMD93E8Zy09Tom2DjOtT0KoKHACdOzG3HYC7F7hquey4zMKevUOhIDTjqt8mE9UsCz_txJTVirL1FAJEdiUOwnE6JUuWr8aJmY63HQr1c_iD3qiwMD0Dt0RhC-PuN0ZO3dmdH489t_edlhOIdl205D00aV0KASJz3WhAMAhTGfBgib_FuKKwa2BfS8djBAoqfBsYdQL5JVDeKuaNLyfFc7o0PiW3DJ2C85s8CJyW10-R144pxLgMbYsbIKEIQQRnCFGRa10LFNHRxIL-th_IA9TXDhgUZeVHvIWQFaIJc_dpQtp-CZi-cGtUewkxxyd5zECh-hm5EplUXldJQJnuZ8TlqNWGtv-1NVHY__hhkUIqIrtkTL1NVoafXmk4TOj1hhByVUn1CS-IhbSAs9qG-MwbABUngbpjRpcPY6cQyhEVoxIwU4skmGy0)

PlantUML Code:

```plantuml
@startuml

interface State {
    + handle() : void
}

class ConcreteStateA {
    + handle() : void
}

class ConcreteStateB {
    + handle() : void
}

class Context {
    - state
    + request() : void
    + getState() : State
    + setState(state : State) : void
    + changeState() : void
}

ConcreteStateA ..|> State
ConcreteStateB ..|> State

State "-state" --o Context

note left of Context::"request()"
    state.handle()
end note

note right of Context::"changeState()"
    // 某些原因引起状态改变，使得 `request()` 调用相应的 `state.handle()`
    switch(value) {
        case 1:
            setState(new ConcreteStateA());
            break;
        case 2:
            setState(new ConcreteStateB());
            break;
        // ...
    }
end note

@enduml
```

## Visitor（访问者）

意图:
> 主要将数据结构与数据操作分离。

![](http://www.plantuml.com/plantuml/png/rLGzRnD14EtlLunSsKA-o5kU8YiGKLCWGNtPZEE3ywsrDvS68aM4MB4420cg425AIWeAX1p-3Fw0od-1jNTyPlS2Hj2GQyuxy-QzsRattiafrbJqqY0WrGTIiAU8L0_s4usI4fKj4WT8NTJmA03p91cXxiGIuLwI9wHit3utu70HsrMPr4XuEyKUPjIgIoy04VYOnWOIuPE8Aecm94V1yYNJECzB23VRkbRTYl-mswFVE8AnJjUnbDYd-Y6R9LhPfdhxIjz_piCOrKSTBrnxixwPNlmz_IsNFbE4a6A7G7KgbuYYsY-vQoKvvyAhlp5razmgOhQ_bDiiBFvTM5mbREvintbdxm5Akakwa5Hev6asGQeqvV-E4hkR9eQlX2YlDFB96CVDiQUTFNOHeeeG0HkU2-x7wSDwmmqA3fe-AzuBmyTju9EV5NxMKUPQFlbPuMQP0shDzTngq6ogoaXuXc7H3yjoY61xkyYntIxJEZdeaa5uvJyX0ySs7iwuPjs8jSdaz6JsyTti_EdtDuF9oxFvwDctaylfs_FnfoysCXcScgo5VJr-4fHUSPJO14R0_IHMP6iB9Xy6qrVFnvSNyz4PG-QZeJdFNhyVVtvHSOtHd0wFfnT3gxoawIWDzTLCaT-HNzfOh_uA5Bgxqbej-FekE-PdqKj_0000)

PlantUML Code:

```plantuml
@startuml

top to bottom direction

together {
    interface Visitor {
        + visitConcreteElement1(concreteElement1 : ConcreteElement1)
        + visitConcreteElement2(concreteElement2 : ConcreteElement2)
    }

    class ConcreteVisitor1 {
        + visitConcreteElement1(concreteElement1 : ConcreteElement1)
        + visitConcreteElement2(concreteElement2 : ConcreteElement2)
    }

    class ConcreteVisitor2 {
        + visitConcreteElement1(concreteElement1 : ConcreteElement1)
        + visitConcreteElement2(concreteElement2 : ConcreteElement2)
    }
}

together {
    interface Element {
        + accept(visitor : Visitor)
    }

    class ConcreteElement1 {
        + accept(visitor : Visitor)
        + operationA()
    }

    class ConcreteElement2 {
        + accept(visitor : Visitor)
        + operationB()
    }

    note left of ConcreteElement1::"accept(visitor : Visitor)"
        visitor.visitConcreteElement1(this)
    end note

    note left of ConcreteElement2::"accept(visitor : Visitor)"
        visitor.visitConcreteElement2(this)
    end note
}

class ObjectStructure {
    - collection
}

class Client {
    - objectStructure : ObjectStructure
    - visitor1 : ConcreteVisitor1
    - visitor2 : ConcreteVisitor2
    + visitor1Walk()
    + visitor2Walk()
}

ConcreteElement1 ..|> Element
ConcreteElement2 ..|> Element
ConcreteVisitor1 ..|> Visitor
ConcreteVisitor2 ..|> Visitor
Element "-collection" --o ObjectStructure
Client .up.> Visitor : <<use>>
Client .right.> ObjectStructure : <<use>>

note left of ObjectStructure::"collection"
    元素的集合，能枚举集合内的元素。
end note

note left of Client::"visitor1Walk()"
    // 将操作（visitor）作用于 objectStructure 的所有元素
    for (Element element : objectStructure) {
        element.accept(visitor1)
    }
end note

@enduml
```
