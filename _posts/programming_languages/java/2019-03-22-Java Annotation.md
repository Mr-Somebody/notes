---
title: Java Annotation
category: PL
comments: true
excerpt_separator: <!--more-->
layout: post
date: 2019-03-13 15:13:00
tags: [java, Annotation]
---
Java Annotation is a very powerful feature, with which, you can do a lot of magical things. Such as tell JVM to perform specific behavior or add some actions to annotated files or methods. This article will demonstrate how to use Annotation to add customized operation to fields.
 <!--more-->
## Declaration of Annotation
Declaring an Annotation is kind of the same to declaring an interface. Instead of using "interface" to decorate the class name, "@interface" is used as the decoration. The following is a simple annotation declaration.
```Java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD})
public @interface TestAnnotation {
  string name() default "Joe";
  int id() default 1;
}
```
In the above declaration, we declared a annotation called "TestAnnotation" with two fields. There are two annotations added to this annotation. "Retention" means how long we will keep this annotation, in this example, we keep this annotation all the time. We can also specify to keep it only during compiling time or before compile. "Target" means which kinds of elements it can be used to decorate. In this example, it can only be used for fields.

There are also two fields declated for this annotation:
1. name with default value of "Joe"
2. id with default value of 1

## Use annotations
To use the annotation declared above, we define a class and use it to decorate one field of it.
```Java
class A {
  @TestAnnotation(name="Alice", id=3)
  private String key;
}
```

## Perform actions on annotated field
Now we can perform action on annotated fields. In the following example, we use annotation and Java Reflection ([Java Reflection](http://blog.chewords.com/pl/2019/03/13/Java-Reflection.html)) techniques to assign value to one field of a object.
```Java
Class Test {
  public static void main(String []args) {
    A a = new A();
    Field field = A.class.getDeclaredField("key");
    field.setAccessible(true);
    TestAnnotation annotation = field.getDeclaredAnnotation(TestAnnotation.class);
    field.set(a, String.format("%s-%d", annotation.name(), annotation.id()));
  }
}
```
Pretty easy, right? This is only an easy example. However, you can do massive things with annotation. Can't wait to explore? Open your IDE and try by yourself.
