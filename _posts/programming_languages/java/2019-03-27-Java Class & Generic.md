---
title: Java Class & Generic
category: PL
comments: true
excerpt_separator: <!--more-->
layout: post
date: 2019-03-27 16:01:00
tags: [java, Class]
---
Here's some Java Class & Generic related tricks:
1. Create an array of a generic type
2. Create an array of a primitive type
3. Getting array class type of a class type
<!--more-->

### Create an array
To create an array of generic type, we need to use java.lang.reflect.Array.newInstance() method to create the array.
```Java
public <T> T[] createArray(Class<T> cls, int size) {
  return (T[])java.lang.reflect.Array.newInstance(size, cls);
}
```

### Create an array for primitive types
Using the method up, we can also create an array for primitive types. However, we can not call "createArray" method on primitive types. The reason is Generic is not compatible with primitive types. So in order to make createArray support primitive types, there are two ways:
1. Change the return type of "createArray" to Object and cast to primitive type array out of this function.
```Java
public <T> Object createArray(Class<T> cls, int size) {
  return java.lang.reflect.Array.newInstance(size, cls);
}
int[] arr = (int[]) createArray(int.class, 10);
```
2. Change the "cls" parameter passed to this function to array class
```Java
public <T> T createArray(Class<T> cls, int size) {
  if (!cls.isArray()) {
        throw new IllegalArgumentException(cls.toString());
  }
  Class<?> primitiveType = cls.getComponentType();
  return cls.cast(java.lang.reflect.Array.newInstance(primitiveType, size));
}
int[] arr = createArray(int[].class, 10);
```

### Getting array class type of a class type
Sometimes, we only know the name of the class, such "A", we need to get the Class of "A[].class". If we have the direct reference of Class A, that would not be problem, but what if not? Here is the solution:
1. Class A is not a primitive type:
```Java
Class<?> cls_A_Array = Class.forName("[Lxxx.xxx.A")
```
Here "xxx.xxx.A" is full name of the class, which is necessary.
2. Class A is a primitive type:
```Java
Class<?> cls_A_Array = Class.forName("[xxx.xxx.A")
```
The only difference is that for non-primitive types, a prefix *"[L"* is added the class name. While for primitive types, only *"["* is added to the class name. For example:
```Java
String[].class == Class.forName("[Ljava.lang.String")
int[].class == Class.forName("[I") // "I" is class name for int ("D" is class name for double, etc. You can you X.class.getName() to view others)
```

### References
1. [how to get a array class using classloader in java?](https://stackoverflow.com/questions/20738207/how-to-get-a-array-class-using-classloader-in-java)
2. [How to convert List<T> to Array t\\[\\] (for primitive types) using generic-method?](https://stackoverflow.com/questions/25149412/how-to-convert-listt-to-array-t-for-primitive-types-using-generic-method)
