# Structural patterns（结构型模式）

结构型模式(Structural Pattern): 描述如何将类或者对象结合在一起形成更大的结构，就像搭积木，可以通过 简单积木的组合形成复杂的、功能更为强大的结构。

## Adapter（适配器）

意图:
> 将一个类的接口转换成客户希望的另外一个接口。适配器模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。

#### Object adapter（对象适配器）

用组合的方法实现。

![](http://www.plantuml.com/plantuml/png/T8xDQiCm48Jl-nI3JYt5XbvTA2szzr3w0a8U5OChETBwbEJTGumaGaoEk_CptqTMctJzdqMce4pUEkBNfZygZW80BqWyhCpwz2nd4JVRdF4LWqjKDcTJFaUxA5C9Tx3RJGn5uUFIOcYxUQ6R_EH-Rgrtotr_UY-yKgVtFwBh8anXCHLI94GbPdf5zFKx3AR1cGsbvFeTq9imZBYMb2gRyVswPUhmPKzszC72y_di7K_sx7c_f-Tf_wfdCzO_czFvj7t1anshdlKjVTg_-CcEtgSJUXutD86rF-rV_tpA2hWfJrjRd-wT33TeLilJjHEURzmDL0tpzDCD6u5cljYx1cJQp1L0MvxitG_N_wXXpjF-6Syw9Zngc8ja2j28f_EwGNOY_E40P8BI0m00)

PlantUML Code:

```plantuml
@startuml

interface Target {
    + request()
}

class ConcreteTarget {
}

class Adapter {
    - adaptee : Adaptee
    + request()
}

class Adaptee {
    + specificRequest()
}

ConcreteTarget ..|> Target
Adapter ..|> Target
Adaptee --o "adaptee" Adapter

note left of Adapter::"request()"
    adaptee.specificRequest();
end note

note as N1
    Adaptee 与 Target 相似，只是有几个接口不同。
    可用 Adaptee 充当 Target 的子类。Adapter 就是转接器，使 Adaptee “变成” Target 的类型。
end note

@enduml
```

#### Class adapter（类适配器）

用多继承的方法实现。

![](https://www.plantuml.com/plantuml/png/V8x12i8m44Jl-nLBJufKy5f15EyUn1zOqwq4ObAJhHVrtmqObwhW77VUxCmw2KKPpWwCevJmGF74WZV0h1b6lWoSP3A51nHY6xo9BAoaEfkMUk7u9rmGLYJrR6ndpNwCVZNKzNrLTi6xOdJ31llXwApvCBKfkz5UIHZ01s5qt0c63WlSD9NEh02pw1MS_qnR0liMVT1Nb72tU3P51Ya8mq0KlPf72s9HiJO1NLIj8eXGYbAx2vZWNUhyn9rNS29qw6uMyz-RpwICoCWMmrNnOCcvhykXcr2seLfYVYRRXE8AVI6xXidVQlgFGxTnMmi5MGQ_4T1-Xhk9tPEtXuPuTCpy-5kYxMgsa7z9lbSLT1aMfbV74OKHEAsIKtfCUEBV_m00)

PlantUML Code:

```plantuml
@startuml

class Target {
    + request()
}

class ConcreteTarget {
}

class Adapter {
    + request()
}

class Adaptee {
    + specificRequest()
}

ConcreteTarget --|> Target
Adapter --|> Target : public
Adapter --|> Adaptee : private

note left of Adapter::"request()"
    adaptee.specificRequest();
end note

note as N1
    Adaptee 与 Target 相似，只是有几个接口不同。
    可用 Adaptee 充当 Target 的子类。Adapter 就是转接器，使 Adaptee “变成” Target 的类型。
end note

@enduml
```

## Bridge（桥接）

意图:
> 将抽象部分与实现部分分离，使它们都可以独立的变化。

![](http://www.plantuml.com/plantuml/png/ZP4nhiCW38Ptdy9YUazFv00Pdb9rwjeRK68a908Hk5EQknUQLYLDXdh2ykyF_fykiOfy7Ho0kYIEIZDgfrB2mxErmUC4c4kY7KP70taE4LiylRl7_0_3I56LZPzVd5wy6MQ0XNacOptrM_HgjUYjPuf6QQflsOhpSD4l_6FmEXBJTpixhv7ozbyxXprYqsHHRRuU2bc5938mh7ZW0nCwCep1xEJHjg9AGW3cge3DXmloFHOYG9UFvHll)

PlantUML Code:

```plantuml
@startuml

abstract class Abstraction {
    - implementor : Implementor
    + operation() {abstract}
}

class RefinedAbstraction {
    + operation()
}

interface Implementor {
    + operationImp()
}

class ConcreteImplementor {
    + operationImp()
}

RefinedAbstraction --|> Abstraction
ConcreteImplementor ..|> Implementor
Implementor "-implementor" --o Abstraction

note left of RefinedAbstraction::"operation()"
    implementor.operationImp()
end note

@enduml
```

## Composite（组合）

意图:
> 将对象组合成树形结构以表示"部分-整体"的层次结构。组合模式使得用户对单个对象和组合对象的使用具有一致性。（一个结点或多个结点都可看作是一棵树）

![](http://www.plantuml.com/plantuml/png/jP8_Rl904CNxESN85UZ3tm5GX91ekSHYnc39ta6xGvea4eeYb08fAQFa1ZHfAPBJnEHFbIjalNQyHA9b2ylxPiRl_KOUMb56baKEe2PMZ4e4arnPYoCk5gn92ru0klCF4rvgwNInZvcMHbFJQQVRl1ig-9pQwunaDC_oKES56IKPQwTS0TGsOrBfQHqyYCs46fiOash8a7O-uypAMOiwE4qEpnZ7buEmL0YfZXNytgFuDsZacZY006WMmfnIGKw3tkz71ywH3vEoGLB9l8PsF2qzO7EyyFymC-afLXO0ESsgouH5kF0J5KmlUEBxmtEvMvMxRM3uVjcKhslqsFkxxP6E7dwkDdy_4ehuyNZpzRJUtz--hQwhB7K8WA5xkb_r1m00)

PlantUML Code:

```plantuml
@startuml

abstract class Component {
    + count() : int
    + add()
    + remove()
    + getChild()
    + operation()
}

class Leaf {
    + count() : int
    + add()
    + remove()
    + getChild()
    + operation()
}

class Composite {
    - children : List<Component>
    + count() : int
    + add()
    + remove()
    + getChild()
    + operation()
}

Leaf --|> Component
Composite --|> Component
Component "-children" --o Composite

note left of Composite::"operation()"
    for each child in children
        child.operation()
end note

legend bottom
    Leaf, Composite 是一个 Component，但 Leaf 只是一个 Component, 而 Composite 是 Component 的集合。
endlegend

@enduml
```

## Decorator（装饰）

意图:
> 动态地给一个对象添加一些额外的职责。就增加功能来说，装饰器模式相比生成子类更为灵活。

![](http://www.plantuml.com/plantuml/png/ZL9RIiH04FplKpIRduXi5lsCAElZ8kDi1nQoqykaHK7t5Xx2IuYN4Ro6apYRZhG1vwipLTLLbMJTYOloi6i1ja4eDiuJtl9kpu62u3DWEgV8UufEjXpA4pW0-DPrNQ857qd8qWsIP7ykSlUTHES5VpRUaUS4S_oViQKRfFHZk5fxpQInXQhgvKfbO8sNoqGO7q41eynJKl140865iAL6a1iRlwuNauoB8VRa8lVkpzYpck0N0tm02XliZRAT6rwXq9CC-6g5HL7Wv_l7r-jRVwEvTIrUOg17HOxBjV7cE9rbhcbphLzZNdq-fxufrI7BLVIxjVPTfx7_kxy1)

PlantUML Code:

```plantuml
@startuml

interface Component {
    + operation()
}

class ConcreteComponent {
    + ConcreteComponent()
    + operation()
}

abstract class Decorator {
    + operation()
}

class ConcreteDecorator {
    + ConcreteDecorator(component : Component)
    + operation()
}

ConcreteComponent ..|> Component
ConcreteDecorator --|> Decorator
Decorator ..|> Component
Component --o Decorator

note left of ConcreteDecorator::"operation()"
    operation() {
        component.operation();
    }
end note

note as Context
    // ### 使用
    Component component = new Component();
    Decorator decorator1 = new ConcreteDecoratorA(component);
    Decorator decorator2 = new ConcreteDecoratorB(decorator1);
    decorator2.operation();
end note

@enduml
```

## Facade（外观）

意图:
> 为子系统中的一组接口提供一个一致的界面，外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。

![](http://www.plantuml.com/plantuml/png/ZP9DQiCm48NtSueXAuU2WwMTcvf0e1Ve2OGyTaqLgSGoNOJSlL8q9OeHRRMGxF7tvkTPXzchirzE1a0RDS_ughJg2I-0OJrXxzxSYVpxPCTg2rS0xyRmIcScFfN-K2Fzd4qCyqhvZd7FmrT8UqakodBnJxaiosL17XBmf7NTtRjlSR-Vh3PBJrrN8CIVX5mHS3GJMT_S6CRRbQq94RyXQzzvFCvzoKt9Fx5pQM3hFA6XmQCRk4xRRXxgR6uq_pETZXUqzCMnQljy9qGYH4w81q8cGkQ434JMQ5E71lmo35LAnlBTBhXH0zaUOzi0X_0Oh-at)

PlantUML Code:

```plantuml
@startuml

class Facade {
    + doSomething()
}

together {
    package package1 {
        class Class1 {
        }
    }

    package package2 {
        class Class2 {
        }
    }

    package package3 {
        class Class3 {
        }
    }
}

Facade ..> package1 : <<include>>
Facade ..> package2 : <<include>>
Facade ..> package3 : <<include>>

Client1 ..> Facade : doSomething()
Client2 ..> Facade : doSomething()

together {
    class Client1 {
    }

    class Client2 {
    }
}

note left of Facade::"doSomething()"
    Class1 class1 = new Class1();
    Class2 class2 = new Class2();
    Class3 class3 = new Class3();

    class1.doStuff(class2);
    // ...
end note

@enduml
```

### 例子

常用的有实现和接口分离。`detail` namespace:
-   [async_simple](https://github.com/alibaba/async_simple/blob/main/async_simple/coro/Lazy.h)

## Flyweight（享元）

意图:
> 运用共享技术有效地支持大量细粒度的对象。

主要解决:
> 在有大量对象时，有可能会造成内存溢出，我们把其中共同的部分抽象出来，如果有相同的业务请求，直接返回在内存中已有的对象，避免重新创建。

![](http://www.plantuml.com/plantuml/png/Z55RQiCm4FpNAHP_9HJd04umXK8kK7e0OO_ZfUegI1kIqFhkhTBKCP4fsI_1pcDsz1pL1ZryE6DO5A6p3MZhpaVmhbVwDFGpJ-Jt25RPom8d3IoHcrUrYgKPZ6cSZP5Ul3G1YdjoIInJokEARn9x6z3EA3ykCfAsjb4VpYDt1nrtYtUSbrJTm9Ep74EIus1C7cIr-gedh4dY_u6tHL5sV-zOK5dwBB6vH4WITNvDHPlD8QAkfwZCVXwMfytXHho273ebtsNsLLLaDHQNVhcZHTumHyA9eyPb-eRh1EWXoE-2PKTZ7-iBP22uY0c-2R0A4XpleMbisn8hgIVjGNllNGf-wtXzxwlzhDPW82sbwwyTgDydfUz1mW-ivEdQ6K-RLZpTkUtvkeNF9xGzwsnuDgVpwVgTBpOkV3whHG4rIHO_RcW2wte-POL20df0wc64bHxEj9sWy7J3ngVzwv_iN_TioauqQq2s81pk06G2ypO0)

PlantUML Code:

```plantuml
@startuml

interface Flyweight {
    + operation(extrinsicState)
}

class ConcreteFlyweight {
    - intrinsicState
    + operation(extrinsicState)
}

class UnsharedConcreteFlyweight {
    + operation(extrinsicState)
}

class FlyweightFactory {
    + getFlyweight(key) : Flyweight
}

class Client {
}

ConcreteFlyweight ..|> Flyweight
UnsharedConcreteFlyweight ..|> Flyweight
Flyweight "-flyweights" --o FlyweightFactory
Client ..> FlyweightFactory : <<use>>
Client ..> ConcreteFlyweight : <<use>>
Client ..> UnsharedConcreteFlyweight : <<use>>

note left of FlyweightFactory::"getFlyweight(key)"
    if (getFlyweight(key) is exists) {
        return existing flyweight;
    } else {
        create new flyweight;
        add it to the pool of flyweights;
        return the new flyweight;
    }
end note

note top of Client
    存储并管理所有对象的 extrinsicStates。
    用 `FlyweightFactory.getFlyweight(key).operation(extrinsicState)` 就可修改 extrinsicState。
end note

@enduml
```

### 例子

protobuf 的 [`DynamicMessageFactory::GetPrototypeNoLock`](https://github.com/protocolbuffers/protobuf/blob/main/src/google/protobuf/dynamic_message.cc) 会先从对象池中查找, 如果找不到则生成, 然后加入对象池中。

## Proxy（代理）

意图:
> 为其他对象提供一种代理以控制对这个对象的访问。

![](http://www.plantuml.com/plantuml/png/bPB1IiGm48RlUOgXfoxIxBsxbWLVGFG9uZgxMub9IQQegEzkEfdAU14sXnAc___DH-aXaqiqltfZA9wHBfZWqq0vH-zoXiVvwGMFnBDwRY1Ec1oDCRGRdduRLNX0vwyktQVu_g7Y7MH1zAl1lwW2gw0xFs8eYvU9Dh7sQ_WbyRQ_epNNTBAuWQwBrSi8r5h9izP-FsSS1cD290IF9u9ugeM-RvHYmuxRRUbRlie6gp8wWk4P5gQGqtY-CBfQS7Ar41BSGi0t_UNRpOw3h0CJFsk89wqK9SNljSvEIHpATVazVW00)

PlantUML Code:

```plantuml
@startuml

together {
    class Subject {
        + operation()
    }

    class RealSubject {
        + operation()
    }

    class Proxy {
        - subject : Subject
        + operation()
    }
}

class Client {
}

RealSubject ..|> Subject
Proxy ..|> Subject
Subject "-subject" --o Proxy
Client ..> Subject : <<use>>

note left of Proxy::"operation()"
    // ...
    subject.operation()
    // ...
end note

note right of Client
    Subject subject = new RealSubject();
    Proxy proxy = new Proxy(subject);
    proxy.operation();
end note

@enduml
```

### 例子

-   cpp 的智能指针
