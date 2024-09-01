---
layout: post
title: "Guice Notes"
date: 2022-07-18 09:57:00 +0800
categories: Java
---
## Dependency Injection

[Dependency injection](https://en.wikipedia.org/wiki/Dependency_injection) is a design pattern wherein classes declare their *dependencies as arguments* instead of creating those dependencies directly.

```java
class Foo {
  private Database database;  // We need a Database to do some work

  // The database comes from somewhere else. Where? That's not my job, that's
  // the job of whoever constructs me: they can choose which database to use.
  Foo(Database database) {
    this.database = database;
  }
}
```

## Core Guice Concepts

### @Inject constructor

Java class constructors that are annotated with @Inject can be called by Guice through a process called *constructor injection*, during which the constructors' arguments will be created and provided by Guice.

标记了 @Inject 的构造方法，Guice 会自动装配其参数。 

```java
class Greeter {
  private final String message;
  private final int count;

  // Greeter declares that it needs a string message and an integer
  // representing the number of time the message to be printed.
  // The @Inject annotation marks this constructor as eligible to be used by
  // Guice.
  @Inject
  Greeter(@Message String message, @Count int count) {
    this.message = message;
    this.count = count;
  }

  void sayHello() {
    for (int i=0; i < count; i++) {
      System.out.println(message);
    }
  }
}
```

The `Greeter` class's constructor arguments are its `dependencies` and applications use `Module` to tell Guice *how to satisfy those dependencies*.

### Guice modules

The above Greeter class has two dependencies (declared in its constructor):

- A `String` object for the message to be printed
- An `Integer` object for the number of times to print the message

```java
/**
 * Guice module that provides bindings for message and count used in
 * {@link Greeter}.
 */
import com.google.inject.Provides;

class DemoModule extends AbstractModule {
  @Provides
  @Count
  static Integer provideCount() {
    return 3;
  }

  @Provides
  @Message
  static String provideMessage() {
    return "hello world";
  }
}
```

### Guice Injectors

```java
    @Inject
    Greeter(@Message String message, @Count int count) {
      this.message = message;
      this.count = count;
    }
```

### A simple guice application

## Mental Model

### Guice is a map

These objects that your application needs are called **dependencies**.

- Guice key: a *key* in the map which is used to fetch a particular value from the map.
- Provider: a *value* in the map which is used to create objects for your application.

### Guice Keys

Guice uses **Key** to identify a dependency that can be resolved using the "Guice map".

```java
@Message String --> Key<String>
@Count int --> Key<Integer>
```

```java
// Identifies a dependency that is an instance of String.
Key<String> databaseKey = Key.get(String.class);
```

However, applications often have dependencies that are *of the same type*:

```java
final class MultilingualGreeter {
  private String englishGreeting;
  private String spanishGreeting;
  // 两个都是 String 类型 
  MultilingualGreeter(String englishGreeting, String spanishGreeting) {
    this.englishGreeting = englishGreeting;
    this.spanishGreeting = spanishGreeting;
  }
}
```