title: Dozer总结
date: 2013-12-07 14:38:21
categories: [Java进阶]
tags: [java,Dozer]
---
##**介绍**
Dozer 是一个对象转换工具。 

`Dozer`可以在JavaBean到JavaBean之间进行递归数据复制,并且这些JavaBean可以是不同的复杂的类型。 
所有的mapping，Dozer将会很直接的将名称相同的fields进行复制，如果field名不同，或者有特别的对应要求，则可以在xml中进行定义。<!-- more -->

官网：[http://dozer.sourceforge.net/documentation/about.html](http://dozer.sourceforge.net/documentation/about.html)
Github:[https://github.com/DozerMapper/dozer](https://github.com/DozerMapper/dozer)

##**为什么使用Dozer**
分析多层架构的J2EE系统，经常存在JavaBean直接的拷贝。比如我们在DAO层，通过Do取得业务层需要的数据，将这些数据传递给 Service层的VO。Do与VO就存在典型的值拷贝。 

典型的解决方案就是手动拷贝，弊端很明显，代码中充斥大量Set 和Get方法，真正的业务被埋藏值与值的拷贝之中。另一种方案就是使用BeanUtil，但BeanUtil不够很好的灵活性，又时候还不得不手动拷贝。Dozer可以灵活的对对象进行转换，且使用简单。 

##**Dozer 支持的转换类型**

Primitive 基本数据类型 , 后面带 Wrapper 是包装类 Complex Type 是复杂类型 

•	Primitive to Primitive Wrapper 
•	Primitive to Custom Wrapper 
•	Primitive Wrapper to Primitive Wrapper 
•	Primitive to Primitive 
•	Complex Type to Complex Type 
•	String to Primitive 
•	String to Primitive Wrapper 
•	String to Complex Type if the Complex Type contains a String constructor 
•	String 到复杂类型 , 如果复杂类型包含一个 String 类型的构造器 
•	String to Map 
•	Collection to Collection 
•	Collection to Array 
•	Map to Complex Type 
•	Map to Custom Map Type 
•	Enum to Enum 
•	Each of these can be mapped to one another: java.util.Date, java.sql.Date, java.sql.Time, java.sql.Timestamp, java.util.Calendar, java.util.GregorianCalendar 
•	String to any of the supported Date/Calendar Objects. 
•	Objects containing a toString() method that produces a long representing time in (ms) to any supported Date/Calendar object. 

##**dozer使用分类**

根据有无映射文件和文件的多少，有三种方式： 
第一种：该方式用于数据类型为基本类型，名称相同的对象映射 

Java代码  
{%codeblock lang:java%}
Mapper mapper = new DozerBeanMapper();  
SourceObject sourceObject = new SourceObject();  
DestinationObject destObject = (DestinationObject) mapper.map(sourceObject, DestinationObject.class);  
    //  or  
DestinationObject destObject = new DestinationObject();  
mapper.map(sourceObject, destObject);  
{%endcodeblock%}

第二种：该方式用于数据类型不一致，或者名称不相同或者有级联关系等情况下的映射，该方式可以添加多个配置文件dozerBeanMapping.xml、someOtherDozerBeanMappings.xml 等 

Java代码  
{%codeblock lang:java%}
List myMappingFiles = new ArrayList();  
myMappingFiles.add("dozerBeanMapping.xml");  
//myMappingFiles.add("someOtherDozerBeanMappings.xml");  
DozerBeanMapper mapper = new DozerBeanMapper();  
SourceObject sourceObject = new SourceObject();  
mapper.setMappingFiles(myMappingFiles);  
DestinationObject stObject=  
(DestinationObject) mapper.map(sourceObject, DestinationObject.class);  
{%endcodeblock%}

第三种：该方式用于数据类型不一致，或者名称不相同或者有级联关系等情况下的映射，配置文件只有一个映射文件叫dozerBeanMapping.xml且在根目录下 

Java代码  
{%codeblock lang:java%}
Mapper mapper = DozerBeanMapperSingletonWrapper.getInstance();  
SourceObject sourceObject = new SourceObject();  
DestinationObject destObject = (DestinationObject) mapper.map(sourceObject, DestinationObject.class);  
//or  
//Mapper mapper = DozerBeanMapperSingletonWrapper.getInstance();  
//DestinationObject destObject = new DestinationObject();  
mapper.map(sourceObject, destObject);  
{%endcodeblock%}

##**参考文献**
1.[Dozer 使用总结，也许对你有帮助](http://seyaa.iteye.com/blog/762494)
