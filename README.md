# Introduction
We plan to include two parts in this repo
 * AsyncSignal, specially designed to distinguish async method.
 * A compiler hacker processer that is able to generate async counterpart for each method in you Dubbo service interface.
 
## AsyncSignal
AsyncSignal is an optional convenient type provided to unify your service definitions when you need an extra async method signature.

Imagine you have a Dubbo service defined like this:
```java
public interface GreetingsService {
    String sayHi(String name);
}
```

Overwrite this method with a new one.
```java
public interface GreetingsService {
    String sayHi(String name);
    
    // AsyncSignal is totally optional, you can use any parameter type as long as java allows your to do that.
    default CompletableFuture<String> sayHi(String name, AsyncSignal signal) {
        return CompletableFuture.completedFuture(sayHi(name));
    }
}
```

Now, you can call the new method with the CompletableFuture signature directly on Consumer side.

## Compiler hacker processer
It should run at compile time, and overwrite each method in your service definition with an async equivalence.
I think we can try to realize it by referencing [lombok](https://projectlombok.org/).

# Conclude
The essential part is to overwrite a new async method. 
Another way would be to generate a new method with a different name, for example, `sayHiAsync`, then we can get rid of `AsyncSignal`. But there's an obvious flaw of this approach, that is, all method level configurators and routers defined to `sayHi` will not take effect anymore.
