---
title: Java Reflection
category: PL
comments: true
excerpt_separator: <!--more-->
layout: post
date: 2019-03-13 15:13:00
tags: [java, reflection]
---
Reflection in java can be used to access the methods and fields of a Java class, which can also be used to function-orientated programming. Examples are given to show how to use Java reflection.
<!--more-->
## Get or set the field values of a object by field name
There are two ways to get the fields of a class:
1. cls.getDeclaredField(String name)
2. cls.getField(String name)

The difference between those to is that, the first one can access all the fields defined in current class, which the second one can get all the public fields declared in the currented class and inherited from super classes. Such as, in the follow example, call getDeclaredFields() on class B would return field "name" and "id", while call getFields() on class B would return "name" and "gender".
```Java
class A {
  public String gender;
}

class B {
  public String name;
  private int id;

  public static int DEFAULT_NAME = "Bob";

  private int add(int a, int b) {
    return a + b;
  }

  public static int deduct(int a, int b) {
    return a - b;
  }
}
```
To set the value of one object's attribute, we can use the Field object gotten by calling getField() or getDeclaredField(). However, do remember to set the field accessible first. Example is given as follow:
```Java
B b = new B();
Field nameField = B.class.getField("name");
nameField.setAccessible(true);
nameField.set(b, "Jason");
System.out.println(nameField.get(b));
Field idField = B.class.getDeclaredField("id");
idField.setAccessible(true);
idField.set(b, 1);
System.out.println(idField.get(b));
```
If the field is static field, how should we get this field's value? The answer is that, instead of passing object to get() or set() method, we pass the class to it like follow:
```Java
Field defaultNameField = B.class.getDeclaredField("DEFAULT_NAME");
defaultNameField.setAccessible(true);
System.out.println(defaultNameField.get(B.class));
defaultNameField.set(B.class, "Mike");
```

## Get and invoke the method of a class
Same as field, there are also two different ways of getting method declared in a class.
1. getMethod()
2. getDeclaredMethod()
And the difference between those two methods are identical to fields.

To invoke a method on an object, call invoke of Method object as follow:
```Java
B b = new B();
Method add = B.class.getDeclaredMethod("add");
int result = add.invoke(b, 1, 2);
```

To invoke a static method, do as follow:
```Java
B.class.getMethod("deduct").invoke(B.class, 5, 4)
```
