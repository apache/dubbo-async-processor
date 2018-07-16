# dubbo-async-processor
Async source processor for Dubbo service.

This extension provides a more convenient way to use Dubbo service on consumer side by adding future-style counterpart for each sync method.

For example, you have a normal Dubbo service, you have to do the following three steps:**(Please notice that all these should be done by developers defining the interface, normally it's service provider on Dubbo)**
1. Add @DubboAsync to your interface.
```java
@DubboAsync
public interface GreetingsService {
    String sayHi(String name);
}
```
2. Add the following configuration to your pom:
```xml
           <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>dubbo-async-processer</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
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

3. Now you can package your service (xxx-api.jar) as normal, the processor will help you generating an Async Dubbo service besides the sync interface:

```java
@javax.annotation.Generated("com.alibaba.dubbo.async.processor.AsyncAnnotationProcessor")
@com.alibaba.dubbo.config.annotation.AsyncFor(com.alibaba.dubbo.samples.api.GreetingsService.class)
public interface GreetingsServiceAsync extends GreetingsService {
    CompletableFuture<java.lang.String> sayHiAsync(java.lang.String name);
}
```
