# Creational patterns（创建型模式）

创建型模式(Creational Pattern): 对类的实例化过程进行了抽象，能够将软件模块中对象的创建和对象的使用分离。

## Factory method（工厂方法）

意图:
> 定义一个创建对象的接口，让其子类自己决定实例化哪一个工厂类，工厂模式使其创建过程延迟到子类进行。

![](http://www.plantuml.com/plantuml/png/SoWkIImgAStDuShCAqajIajCJbK8ACfFAKqkKQZcgkNYIiv9B2vMSCxFAqejIKMHGMXmBafDBCal0Weh086sGZA8dwgXgM05Cml39EAqe5chfrTZ1vT6DGWY1-LWojcX-y1AmSO6OgUT7HTNNdv9ga9EQbg9GduQp10hAoMOevJ0ZjJNLtYwhEdPl3dFvdG-NJBhoOvLJrktFzax-SckvKydDuALG3MKL1QaQZvkQE9ApKjH09dDnUK0P8926G00)

PlantUML Code:

```plantuml
@startuml
interface Product {
}

class ConreteProduct {
}

interface Creator {
    + create() : Product
}

class ConreteCreator {
    + create() : Product
}

ConreteProduct ..|> Product
ConreteCreator ..|> Creator
ConreteCreator ..> ConreteProduct : <<create>>

note left of Creator::"create()"
    // 如果有多个具体产品
    create(type)。
end note

@enduml
```

### 例子

protobuf:
-   [`MessageFactory::GetPrototype`](https://github.com/protocolbuffers/protobuf/blob/main/src/google/protobuf/message.h) 让子类决定具体的创建。
-   [`DynamicMessageFactory::GetPrototype`](https://github.com/protocolbuffers/protobuf/blob/main/src/google/protobuf/dynamic_message.cc)

## Abstract factory（抽象工厂）

意图:
> 提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。

![](http://www.plantuml.com/plantuml/png/jP91ou9048Rlyols-7q4WjUGHIVet5_Orga4KzWT3r7-zzRKRKaBExGWWi_3l2_ZffQLuUPS0d1z93wH4LSGxXGLapaeJIBRMpPAyyxKUQCv6uGM7YzTAcl5o68Fs-KJySD4_6xYrZsrkNHyE2kX3IcciU7VlrZxkkyi32sSbXjYuf_akAG-PLCMLB7BS5-U2uwYcvpyav7ZF4VmZJqErWNGHpauOQvVaZIMf9oHVFuY2mChB2I3wzCP_Ogad7NmnO6Kghoxm7S0)

PlantUML Code:

```plantuml
@startuml

together {
    interface AbstractProductA {
    }

    class ProductA1 {
    }
}

together {
    interface AbstractProductB {
    }

    class ProductB1 {
    }
}

together {
    interface AbstractFactory {
        + createProductA() : ProductA
        + createProductB() : ProductB
    }

    class Factory1 {
        + createProductA() : ProductA
        + createProductB() : ProductB
    }
}

class Client {
}

ProductA1 ..|> AbstractProductA
ProductB1 ..|> AbstractProductB
Factory1 ..|> AbstractFactory

Client ..> AbstractFactory : <<use>>
Client ..> AbstractProductA : <<use>>
Client ..> AbstractProductB : <<use>>

Factory1 ..> ProductA1 : <<create>>
Factory1 ..> ProductB1 : <<create>>

@enduml
```

### 例子

grpc ClientAsyncReaderFactory:

-   [channel_interface](https://github.com/grpc/grpc/blob/master/include/grpcpp/impl/channel_interface.h)
-   [async_stream.h](https://github.com/grpc/grpc/blob/master/include/grpcpp/support/async_stream.h)

## Builder（生成器）

意图:
> 将一个复杂的构建与其表示相分离，使得同样的构建过程可以创建不同的表示。

![](http://www.plantuml.com/plantuml/png/bL91RiCW4BppYfLFZYhx0KvYfMaV4FK73bvNI1GgMDf3cxyN79PQg9VwOfZTcPrPSV8qEGflMWYf9-_XukmS9C6Nk0cX3A5R9ebm3ahFw28CyNk0QxfV8lMntTNGEK8trdkoHZea_9y0Gwz8B-Y3fdO70tlx5NzM3YLPLktWcgSCR3ZhI6iykI3lEXzMpHk7Mg79WMktVVyz5Ya6cowFQQ3hR37n1taiwnpWS8Z5YN0SHPwOwanU6u9FLM4i8MXS2EnI6eQXsOHeKYvEsrUxh0gyKJGp9EbgmDtCIgi3oEf-18EVTLUxsw_aacJEXzpEU5kfPHftzoKT2_BgtlF_MFlfDwzZlubtYiVqedy0)

PlantUML Code:

```plantuml
@startuml

class Product {
}

interface Builder {
    + buildPartA()
    + buildPartB()
}

class ConcreteBuilder {
    + buildPartA()
    + buildPartB()
    + getResult() : Product
}

class Director {
    - builder : Builder
    + construct() : void
}

class Client {
}

ConcreteBuilder ..|> Builder
Builder "-builder" --o Director
ConcreteBuilder ..> Product : <<create>>
Client ..> Director : <<use>>

note left of Director::"construct()"
    builder.buildPartA()
    builder.buildPartB()
end note

note left of Client
    ConcreteBuilder concreteBuilder = new ConcreteBuilder();
    Director director = new Director(concreteBuilder);
    director.construct();
    Product product = concreteBuilder.getResult();
end note

@enduml
```

### 例子

protobuf [`DescriptorBuilder::BuildFileImpl`](https://github.com/protocolbuffers/protobuf/blob/main/src/google/protobuf/descriptor.cc)

## Prototype（原型）

意图:
> 用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。

![](http://www.plantuml.com/plantuml/png/dPBFIiD04CRl-nH3JYsI53q6b88lu2k4EC70EXCsKwIeK0h-WO8Fu471OmN5KuklOrFYMvY49HjA3j8zxJBVzyrlXXtd4XcPN6gbKX8qIHGZd1aMbcc6SAsWEWSGQoOS325qDNMhLAoZF8TJfjYWO5iwtDDtz4-VJf74Qdt8MjgEskPmXYQIb6amhFqzf45mSzAnzJ3jADnoFBzjxO7limLJYbWGD2O2dFHi9mmzEv_NO5RLYI6G2uIOisbjob4d2kaSwgZTmAPB5UA6iq6Z4HGVRzl7LVcvsdxDD-lPPhqidbwBbqLnj_XzlYyVbkNt-yzitiZT98HNxd7iDXfOpWxAkBhMt-KF)

PlantUML Code:

```plantuml
@startuml

interface Prototype {
    + clone() : Prototype
}

class ConcretePrototype1 {
    + clone() : Prototype
}

class ConcretePrototype2 {
    + clone() : Prototype
}

class Client {
    - prototype : Prototype
    + operation()
}

ConcretePrototype1 ..|> Prototype
ConcretePrototype2 ..|> Prototype
Prototype "-prototype" --o Client

note left of ConcretePrototype1::"clone()"
    return the copy of self
end note

note left of Client::"operation()"
    // 客户请求一个原型克隆自身。
    Prototype newPrototype = prototype.clone()
end note

@enduml
```

### 例子

C++ 可以通过复制/移动构造来灵活地实现原型模式。而一些以"引用"来指定绑定对象实例的语言则会提供 shallow copy 或 deep copy 来克隆一个实例。比如: rust, java, lua, python 等。

## Singleton（单件）

意图:
> 确保一个类只有一个实例，并提供对该实例的全局访问。

![](http://www.plantuml.com/plantuml/png/VLB1IiD04BtlLmpj9KLjxos5YdZefPSAdjTaqYnkPdKpswAIitWIFFW7Ve17GV1hBFWNhccico2PIuR9Us_cxSoiO6dPvLg8MCkYWAMYbKOs17S2V1o18tNjS4uUIJ72E8Ju6gkuh97x7z6WgXp02lcN60t-fvP2a644ZIc3IVyWut6lGUzLcgCHzFjzsDu_RLyVjizld--FLTqYEqkjOKL8-NhvQ59K6hMyJQT0Jkk1jrv7SKDnPWsfMqoY_MZ3wgq2MDtcF4E2t5W4pYG1RunFBCga0Ei85F5F0I5Ljk2g5SGPnfTGoDnpL8w7RKdFa6kZ4b3ra4dGm53D0iL0YBwFnr_WJjL3vKeg6eQQtQvRCLyipuuN9wVW4RW9zpjfO4lHpCfW9NkHYK1AW0nZnyQRetMl_CggjMI4tIL1gaZguCBQBjiMsiVHARussdyrTkexhGEFCv-wN8jl)

PlantUML Code:

```plantuml
@startuml

class Singleton {
    - uniqueInstance : Singleton {static}
    - Singleton()
    + getInstance() : Singleton {static}
}

note left of Singleton::"getInstance()"
    // ### 懒汉方式
    return uniqueInstance

    // ### 饿汉方式
    if(uniqueInstance == null){
        synchronized(Singleton.class){
            // When more than two threads run into the first null check same time,
            // to avoid instanced more than one time, it needs to be checked again.
            if(uniqueInstance == null){
                INSTANCE = new Singleton();
            }
        }
    }
    return INSTANCE;
end note

note left of Singleton::"uniqueInstance"
    // ### 懒汉方式
    private static final Singleton uniqueInstance = new Singleton()

    // ### 饿汉方式
    private static volatile Singleton uniqueInstance = null
end note

@enduml
```

[有两种方式实现](https://zh.wikipedia.org/wiki/%E5%8D%95%E4%BE%8B%E6%A8%A1%E5%BC%8F)

1.  懒汉方式。指全局的单例实例在第一次被使用时构建。
2.  饿汉方式。指全局的单例实例在类装载时构建。

[*C++和双重检查锁定模式(DCLP)的风险*](https://blog.csdn.net/linlin003/article/details/79012416)

> 因为 C/C++ 语言没有线程的概念，所以只用 C/C++ 通过双重检查锁定模式(DCLP)来实现线程安全的单例模式是不可能的。

### 例子

protobuf [`MessageFactory::generated_factory`](https://github.com/protocolbuffers/protobuf/blob/main/src/google/protobuf/message.h)
