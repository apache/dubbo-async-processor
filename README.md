# dubbo-async-processor
Async source processor for Dubbo service.

This extension provides a more convenient way to use Dubbo service on consumer side by adding future-style counterpart for each sync method.

For example, you have a normal Dubbo service define as following:
```java
@DubboAsync
public interface GreetingsService {
    String sayHi(String name);
}
```
The processor can help you to generate an Async Dubbo service:
```java
@javax.annotation.Generated("com.alibaba.dubbo.async.processor.AsyncAnnotationProcessor")
@com.alibaba.dubbo.config.annotation.AsyncFor(com.alibaba.dubbo.samples.api.GreetingsService.class)
public interface GreetingsServiceAsync extends GreetingsService {
CompletableFuture<java.lang.String> sayHiAsync(java.lang.String name);
}
```

To use this extension, add the following configuration to your pom:
```xml
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.7.0</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <annotationProcessorPaths>
                        <path>
                            <groupId>com.alibaba</groupId>
                            <artifactId>dubbo-async-processer</artifactId>
                            <version>1.0.0-SNAPSHOT</version>
                        </path>
                    </annotationProcessorPaths>
                </configuration>
            </plugin>
```
