# Introduction
There will have two parts in this repo
 * AsyncSignal class specially designed to distinguish async method.
 * A compiler hacker processer that is able to generate async counterpart for each method in you Dubbo service interface.

## AsyncSignal
AsyncSignal is an optional convenient type provided to unify your service definitions.

Imagine you have a Dubbo service defined like this:
```java
public interface GreetingsService {
    String sayHi(String name);
}
```

Add an overwrite counterpart method.
```java
public interface GreetingsService {
    String sayHi(String name);
    
    // AsyncSignal is totally optional, you can use any parameter type as long as java allows your overwrite.
    default CompletableFuture<String> sayHi(String name, AsyncSignal signal) {
        return CompletableFuture.completedFuture(sayHi(name));
    }
}
```

Now, you can call the new method with the CompletableFuture signature directly on Consumer side.

## Compiler hacker processer
It should run at compile time, and overwrite each method in your service definition with an async equivalence.
I think we can try to realize it by referencing [lombok](https://projectlombok.org/).
