---
title: Spring Framework Architecture
seoTitle: Spring Framework Architecture
date: 2019-06-25T00:27:00+00:00
author: bcaashka
layout: post
permalink: /spring-framework-architecture/
categories:
  - Guides
tags:
  - guides
draft: true
---
Hey guys. Today I am going to introduce you to Spring Framework and its Core functionality. 

##Spring Framework – when did it appear and become popular? 
It appeared in the good old days when EJB was dominating the world. It was a powerful monster used for developing corporate applications. Ideas upon which EJB is based are good, but for simple projects it was too inflexible to use it and launch in complex containers. 

## What are Spring’s main advantages due to which it won the hearts of millions? 
### Lightweight jar libraries
First of all, Spring is a lightweight framework. Lightweight in two respects. Firstly, it consists of a collection of small jar-files. It used to be one jar-file which occupied 2 megabytes. Now it is divided into separate components. That is, if you don’t need to work with the database you don’t have to connect spring-jdbc module. 

On the other hand, what lightweight means is that the classes that you develop could be independent of Spring. That means that there is a division between the Spring and the classes. As a result, you are not tied firmly to the framework. 

### Container that manages lifecycle of objects
Moving on, Spring is a container. It means that Spring controls the lifecycle of the objects that it creates and which live in it. You won’t have to write new, dispose, create dependent objects. That means that the objects will live and move around in the container independently. 

### Framework - classes and utilities for application creation
Spring is a framework. That is, apart from being a container with objects, it offers you multiple wheels that are ready to roll. I.e., Spring has many utility-classes that simplify working with the database, mail and web services. I.e., lots of java-related technologies that are used in different applications. 

### Dependency Injection(Inversion of Control)
And, surely, the main shtick is dependency injection. It is implementation of the principle of inversion of control, when objects don’t create their own dependencies but receive them instead. We will get back to it later. 

### Aspect oriented programming (AOP)
And the second shtick is the aspect-oriented programming. Spring realizes support of AOP itself and many of its utility classes and the functions work with help of this technology. AOP will be discussed later on as well. 

## Spring Modules
So, what are Spring’s main modules? 

### Spring Core Container
Firstly, it’s the Core Container that consists of core-modules, that is to say, Spring’s heart. You are going to use these modules mostly, even when creating a simple app. You start with these and then you add new modules if necessary. The two main modules are Beans and Core that control the beans and realize injection of dependencies. Context that controls the context where the beans are stored and that provides access to them. Expression is a special language of expressions that can be used for searching and modifying the bean graph during run-time. 

### Spring AOP Module
The next module is AOP support module. It’s an AOP internal embedded module that allows you to use aspect-oriented programming. On top of that, it is an Aspects module that provides support of AspectJ library. 

### Spring Instrumentation Module
Moving on, the Instrumentation module. It is necessary when you use Spring in an application server, for example, Tomcat. It ensures uploading of your classes into the container and allows it to control Spring context and beans. 

### Spring Data Access and Integration Modules
Consequently, there are a lot of modules connected to access to data and integration between applications. It includes JDBC as well as ORM technologies, support of transactions, JMS and other integrational things. 

### Spring Web & Remoting Modules
It also includes web and remoting modules that serve to create components for web apps and web apps themselves. It also ensures support of basic things like realization of uploading the file to a server. Besides, it includes implementation of MVC pattern for creating web-apps and components for creating web-services, providing security, and integration with other web-technologies. 

### Spring Test Module
Naturally, there’s the Test module that allows to simplify testing Spring applications. You can create the Spring context, create beans and call their methods with its help. 

## How to connect to Spring and create a project with its help? 
To master the material in a better way, I should ask you to write the code and develop some things while reading this article and the following ones. It will give you a practical insight into how it all is functioning. So, start your favourite IDE, create the basic Maven project. Assign the following <b>com.yet.spring groupId</b> and <b>com.yet.spring.core artifactId</b>. But you can choose the different ones to your liking. 

Then add dependencies in pom.xml for the following four Spring modules:
* spring-context
* spring-context-support
* spring-tx
* spring-jdbc

We are going to use the other modules too but they’re going to be pulled automatically like dependencies. Use 5.1.2 Spring version. Alternatively, you can try the last one that you find in the repository. Connect all Spring modules of the same version.

\\Code of pom.xml 

The main dependencies here are <b>spring-context</b> and <b>spring-context-support</b>. All the lacking modules will be loaded. <b>Tx</b> is for transaction support, <b>jdbc</b> is for working with the database. 

Once you save the pom.xml, look at IDE for the libraries that have been loaded as dependencies. You’ll see <b>spring-beans</b>, <b>spring-aop</b> and other libraries there. 

## Spring Simple Example
Let’s continue learning Spring with specific examples. To gain understanding of dependency injection, we’ll start with a simple app.

Create three classes – <b>App, Client, and ConsoleEventLogger</b>. 

In <b>ConsoleEventLogger class</b> there will be only one method – <b>logEvent</b>, that will output the sent message to the console, i.e. it will do <b>System.out.println</b> and that’s it. 

The <b>Client class</b> will only have <b>id, name, getters and setters</b>. 

The <b>App class</b> will have the client and eventLogger that will be created there as well. This class will also have the <b>logEvent method</b> that will receive the string and redirect it to eventLogger for further processing. And, of course, there will be <b>public static void main method</b> that will launch the app. With this method we will call <b>logEvent method</b> and in this way we will imitate the system for processing log-in requests. 

Let’s apply simple logic to App class logEvent method: if the client’s id is present in the message msg, then you change the id to full name. Send the newly formed String to consoleEventLogger to be brought to the console. 

Just a little logic to make the app do something rather than just type at the console. 

Start the app to check that everything’s working as expected. 

Let’s have a critical look at our application. It’s good. It serves its purpose: it receives the events and logs them somewhere at the console so far. So, what kind of problems does it have? 


## Application Problems 
### Modification is problematic
One of the main problems is that *the app is difficult to modify*. You’ll say, ‘Why is that?’ You just retype the code, and it’s over. The matter is that the data of the app are embedded in the code. 

For instance, take the client. We’ve created it in the application and assigned the name with id. If we want another client, we’ll have to recompile. For sure, this problem is not difficult to solve extract the data to the property-file – but anyway we have to deal with it. 

### Scaling is impossible
The other problem of our app is that it is impossible to scale. What does it mean? Imagine that we are, let’s say, logging events, one per second so far. It’s working fine, everything’s on track. What if events start appearing very often and we will want to change their processing, add caching or even change the logic of processing. 

The thing is that we’ve created one copy of the logger. We’ve hard-coded it. We’ve written 'new logger' etc. The matter is that we don’t have the simple possibility to change the logic in any way, add new functionality without recompiling the app. 

### Testing is hard
Another problem to consider is the testing of the app. ConsoleEventLogger is not difficult to test by implementing the unit-test and checking that the console contains the assigned data. This is good. 

Still, how can we test the App class and the logEvent method that changes the id to the name? 
When you develop a unit-test for it, it will appear that you are testing the ConsoleEventLogger indirectly. Clearly enough, we can solve every problem separately. However, all these three problems come up for the same reason. This is a *strong coupling between parts of the app*: when classes create each other; when data is embedded; the logic that processes the data creates the classes as well – when all of these take place that means that there is a strong coupling within the app and, therefore, the above listed problems, if not more than that, can arise. 

How can we solve it all? The implementation of the *app with low coupling between its modules will help us*. We are going to talk about it in the next article. See you! 