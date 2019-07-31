---
title: java反射机制
tags: java
abbrlink: 44374
date: 2018-12-24 15:31:26
---
## 一、什么是反射？
在运行状态中，对于任意一个类都能够获取到这个类的所有属性和方法，对于任意一个对象，都能够调用任一一个属性和方法（包括私有属性和方法），这种动态获取信息以及动态调用对象的方法的功能称为java的反射机制。反射实现在运行时可以知道任意一个类的属性和方法。
## 二、Class类和类类型
Class类：Class是所有类的类，所有类都是Class的实例对象。
### 1. 获取Class对象的方式
普通对象的创建方式：  
```java
User user = new User();
```
获取Class对象的三种方式：
(1) 通过获取类的静态成员变量class得到
 ```java
 Class class1 = User.class
 ```
  这说明任何一个类都有一个隐含的静态成员变量class
(2) 通过类对象的getClass()方法得到
```java
Class class2 = user.getClass();
```
(3) 通过类的全量限定名得到
```java
Class class3 = Class.forName("com.test.reflect.User");
```
class1,class2,class3都是Class的对象，它们是完全一样的，统一叫做`User的类类型`。
### 2. 类类型
就是类的类型，描述一个类是什么，都有哪些东西，我们可以通过类类型知道一个类的属性和方法，并且可以调用一个类的属性和方法。
## 三、Java反射的相关操作
```java
Class<UserServiceImpl> class1 = UserServiceImpl.class;
```
### 1. 获取成员方法
```java
/**
* 获取类中所有方法，不包括父类中的
*/
Method[] methods = class1.getDeclaredMethods();

/**
* 获取类中所有public方法，包括父类中的
*/
Method[] methods = class1.getDeclaredMethods();

/**
* 获取指定方法名和指定参数的方法，包括父类中的
*/
Method method = class1.getMethod("getUser", String.class, String.class);
```
具体使用：
```java
Class<UserServiceImpl> class1 = Class.forName("com.test.reflect.service.UserServiceImpl");
//创建该类的实例对象，该方法只会调用类的无参构造方法
UserServiceImpl userService = class1.newInstance();
Method method = class1.getMethod("getUser", String.class, String.class);
//通过invoke调用该方法，第一个参数为实例对象，后两个为传入的具体值
method.invoke(userService, "name", "email");
```
### 2. 获取成员变量
```java
/**
* 获取该类自身声明的所有变量，不包括父类变量
*/
Field[] fields = class1.getDeclaredFields();

/**
* 获取该类所有public成员变量，包括父类的
*/
Field[] fields1 = class1.getFields();
```
具体使用：
```java
Class<User> userClass = User.class;
Field name = userClass.getDeclaredField("name");
User user = userClass.newInstance();
// 设置是否允许访问，因为name变量是private的，所以要手动设置允许访问，如果name是public的就不需要这行了
name.setAccessible(true);
Object o = name.get(user);
System.out.println(o);
```
### 3. 获取构造函数
```java
/**
* 获取该类所有的构造器，不包含父类的 参数为构造函数参数类的类类型列表
*/
Constructor<User> constructors = class1.getDeclaredConstructor();
/**
* 获取该类所有public的构造器，包含父类的
*/
Constructor<User> constructors1 = class1.getConstructor();
```
Class的newInstance()方法只能创建包含无参构造函数的类，如果某类只有带参数的构造函数，name使用下面的方法实例化对象：
```java
Class<User> userClass = User.class;
Constructor<User> constructor = userClass.getDeclaredConstructor(String.class);
// 如果构造方法是private的，需要手动设置允许访问，如果是public则不需要
constructor.setAccessible(true);
User user = constructor.newInstance("test");
```
