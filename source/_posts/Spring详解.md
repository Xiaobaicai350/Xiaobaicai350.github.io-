---
title: Spring详解
date: 2023-05-12 02:20:06
tags:
---

# Spring

面试题：IoC和DI的关系是什么？

> 首先，先回答IoC和DI的是什么：
> 
> - IoC： Inversion of Control，控制反转，将Bean的创建权由**原来程序**反转给**第三方**
> - DI：Dependency Injection，依赖注入，**某个Bean的完整创建依赖于其他Bean**（或普通参数）**的注入**

其次，在回答IoC和DI的关系：

> - 第一种观点：IoC强调的是Bean创建权的反转，而DI强调的是Bean的依赖关系
> - 第二种观点：IoC强调的是Bean创建权的反转，而DI强调的是通过注入的方式反转Bean的创建权，认为DI是IoC的其中一种实现方式

感受下下面两种创建工厂的区别

```java
//DefaultListableBeanFactory是BeanFactory的一个具体实现
//XmlBeanFactory是DefaultListableBeanFactory的子类，添加了Xml读取的功能，也就是不用自己创建XmlBeanDefinitionReader了
BeanFactory beanFactory = new DefaultListableBeanFactory();
//利用Resource加载配置文件
ClassPathResource resource = new ClassPathResource("applicationContext.xml");
//添加Reader到工厂
XmlBeanDefinitionReader xmlBeanDefinitionReader = new XmlBeanDefinitionReader(beanFactory);
//把配置文件添加到工厂中
xmlBeanDefinitionReader.loadBeanDefinitions(resource);
Object bean = beanFactory.getBean("user");
```

```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
Object u = applicationContext.getBean("u");
```

面试题：BeanFactory与ApplicationContext的关系
1）BeanFactory是Spring的早期接口，称为Spring的Bean工厂，ApplicationContext是后期更高级接口，称之为Spring 容器；
2）ApplicationContext在BeanFactory基础上对功能进行了扩展，例如：监听功能、国际化功能等。BeanFactory的API更偏向底层，ApplicationContext的API大多数是对这些底层API的封装；

> ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682162224300-c5a16a4a-0388-4b25-b4df-1c60818d6805.png#averageHue=%23302d2d&clientId=u157164f7-20a8-4&from=paste&height=412&id=ud20e4ff2&originHeight=515&originWidth=1544&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=43191&status=done&style=shadow&taskId=ubabaf8d5-4487-40ad-bb18-6efba4b0a98&title=&width=1235.2)

3）Bean创建的主要逻辑和功能都被封装在BeanFactory中，ApplicationContext不仅继承了BeanFactory，而且ApplicationContext**内部还维护着BeanFactory的引用**，所以，ApplicationContext与BeanFactory既有继承关系，又有融合关系。

> ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682162469216-50411ba7-a7c5-4146-8807-39808646ab34.png#averageHue=%23393e42&clientId=u157164f7-20a8-4&from=paste&height=261&id=u2a6b5829&originHeight=326&originWidth=1452&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=60760&status=done&style=shadow&taskId=u986c4bbf-ab53-45f7-8ffa-93734cfe731&title=&width=1161.6)

4）Bean的初始化时机不同，原始BeanFactory是在首次调用getBean时（懒加载，用到哪个加载哪个）才进行Bean的创建，而ApplicationContext则是在**容器一创建（也就是在你启动你的项目的时候就会创建了）就将Bean都实例化并初始化好**。

总的来说就是这个关系： ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682162578695-e3f6539a-0734-4292-9608-9a055731f4d0.png#averageHue=%23d1d0cd&clientId=u157164f7-20a8-4&from=paste&height=486&id=u300a286b&originHeight=608&originWidth=930&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=108642&status=done&style=shadow&taskId=ue3137f73-c888-4559-8d9a-cf2898025d6&title=&width=744) **ApplicationContext是对BeanFactory的进一步封装，并且提供了更多的功能**

ApplicationContext文件结构： ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682163314023-cf785b9a-f526-4841-805a-55687c19496c.png#averageHue=%233e464d&clientId=u157164f7-20a8-4&from=paste&height=409&id=u5fad5055&originHeight=511&originWidth=936&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=105334&status=done&style=shadow&taskId=u48e99cba-43fa-4783-8bce-9a4ab99d69d&title=&width=748.8)

**如果闻到bean标签的scope属性的回答：** 当只是一个简单的spring项目时，只有singleton和prototype两种，一种是单例，用到的事件直接去singletonObjects（单例池）中去拿就可以了，另一种是每次创建都会创建一个新对象
但是当在web-MVC环境下，就会多出两个： ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682164663836-6bd27c9d-95d8-4786-a34b-d03e5c0ea066.png#averageHue=%23f5f0d0&clientId=u157164f7-20a8-4&from=paste&height=226&id=u144ca6f4&originHeight=282&originWidth=1371&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=234318&status=done&style=shadow&taskId=u3bf13c1c-183d-4474-8a73-cae71d3085e&title=&width=1096.8) 知道这个就行了

bean标签配置延迟加载： ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682164734229-3fe98404-d551-4a01-b6aa-f037baea0b48.png#averageHue=%23e8eedc&clientId=u157164f7-20a8-4&from=paste&height=76&id=udf365fac&originHeight=95&originWidth=1668&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=84339&status=done&style=shadow&taskId=u7a2f0e4d-6851-4058-a5da-3d4dd29a7c5&title=&width=1334.4) 这个在使用ApplicationContext的情况下可以用。但是我们知道，如果使用BeanFactory（工厂）创建bean，都是延迟加载，也就是说如果使用BeanFactory，配置这个就没用了！！！！

## Spring的实例化

Spring的实例化方式主要如下两种：

- 构造方式实例化：底层通过构造方法对Bean进行实例化 (一般默认我们都是进行无参创建....所以这个没啥奇怪的，也没啥细说的)

- 工厂方式实例化：底层通过调用自定义的工厂方法对Bean进行实例化
  
  ### 工厂实例化
  
  工厂实例化和@Bean注解的性质一样，可以创建不是我们写的代码的bean
  工厂方式实例化Bean，又分为如下三种：
  ⚫ 静态工厂方法实例化Bean
  ⚫ 实例工厂方法实例化Bean
  ⚫ 实现FactoryBean规范延迟实例化Bean
  
  #### 静态工厂实例化
  
  ```xml
  <bean id="myUser" class="com.haohao.factory.MyFactory" factory-method="createUserByFactory">
  <!--      如果这个方法需要传入参数，这里可以用这个标签传递参数-->
  <!--      <constructor-arg name="" value=""/>-->
  </bean>
  ```
  
  ```java
  package com.haohao.factory;
  ```

import com.haohao.Entity.User;

//静态工厂实例化
public class MyFactory {
 //注意这里的static，这里就是和实例工厂的区别
 public static User createUserByFactory(){
 //这里还可以创建一些非我们自己写的Bean
 return new User();
 }
}

```
```java
public void test3(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
    Object u = applicationContext.getBean("myUser");
    System.err.println(u);
}
```

#### 实例工厂实例化

```xml
  <!--        先创建myFactory对象-->
  <bean id="myFactory" class="com.haohao.factory.MyFactory"/>
  <!--        在这里通过factory-bean="myFactory" 进行引用-->
  <bean id="myUser" class="com.haohao.factory.MyFactory"  factory-bean="myFactory" factory-method="createUserByFactory">
    <!--        如果这个方法需要传入参数，这里可以用这个标签传递参数-->
    <!--        <constructor-arg name="" value=""/>-->
  </bean>
```

```java
package com.haohao.factory;

import com.haohao.Entity.User;

//静态工厂实例化
public class MyFactory {
    //注意这里没有static了，这里就是和静态工厂的区别，所以我们要先创建MyFactory这个bean，才能创建User对象
    public  User createUserByFactory(){
        //这里还可以创建一些非我们自己写的Bean
        return new User();
    }
}
```

```java
    public void test3(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        Object u = applicationContext.getBean("myUser");
        System.err.println(u);
    }
```

#### FactoryBean

```xml
<!--  注意，这里配置的是类MyFactory，但是获取对象的却是user对象-->
<bean id="myUser" class="com.haohao.factory.MyFactory"/>
```

```java
package com.haohao.factory;

import com.haohao.Entity.User;
import org.springframework.beans.factory.FactoryBean;

//FactoryBean实例化对象
public class MyFactory implements FactoryBean {
    //这里创建的对象，只有用到的时候（也就是getBean的时候才会创建），
    //但是在applicationContext创建之后，会默认创建MyFactory对象，所以在getBean之前，这个bean还一直是MyFactory
    @Override
    public Object getObject() throws Exception {
        return new User();
    }

    @Override
    public Class<?> getObjectType() {
        return null;
    }
}
```

```java
public void test3(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
    Object u = applicationContext.getBean("myUser");
    System.err.println(u);
}
```

## spring的标签

Spring 的 xml 标签大体上分为两类，一种是默认标签，一种是自定义标签
⚫ 默认标签：就是不用额外导入其他命名空间约束的标签，例如 标签
⚫ 自定义标签：就是需要**额外引入其他命名空间约束**，并通过前缀引用的标签，例如 context:property-placeholder/ 标签

### 默认标签

![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682173595423-2809cfb1-6789-47d5-8f1e-07bcddc9191d.png#averageHue=%23bccbe1&clientId=u157164f7-20a8-4&from=paste&height=146&id=uab4d9086&originHeight=182&originWidth=965&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=47898&status=done&style=shadow&taskId=uc57d0401-a46b-48ce-bacf-7838b9e8b8b&title=&width=772)

### 自定义标签

![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682173621294-130979a8-ef2f-4df5-9234-d2f8a30420aa.png#averageHue=%23e9eeda&clientId=u157164f7-20a8-4&from=paste&height=254&id=u9196524b&originHeight=317&originWidth=969&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=91444&status=done&style=shadow&taskId=u7e18a23a-f507-4250-8b18-ebd4c516b86&title=&width=775.2)

## Spring的Bean实例实例化的基本流程

先来看两个关键的类 ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682180428467-b15d1ca4-a54a-4257-9ae3-58d0f1928bb3.png#averageHue=%23342e2d&clientId=u157164f7-20a8-4&from=paste&height=268&id=u0e49e6ca&originHeight=335&originWidth=1505&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=74779&status=done&style=shadow&taskId=u51708441-004c-443f-9dc9-edb3b15e63c&title=&width=1204) ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682180496848-871105a7-bdf0-4d74-b54c-9db294f97542.png#averageHue=%23312d2c&clientId=u157164f7-20a8-4&from=paste&height=338&id=u1f05b10a&originHeight=422&originWidth=1437&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=84741&status=done&style=shadow&taskId=u137ac2d0-6f5c-46a7-a3f7-9eea593e0f8&title=&width=1149.6) ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682180519482-a4e82e35-cda8-48f3-88b1-4b2acddc5f19.png#averageHue=%23312d2c&clientId=u157164f7-20a8-4&from=paste&height=338&id=u13e9ec44&originHeight=422&originWidth=1437&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=84741&status=done&style=shadow&taskId=u0dd8833d-df28-4829-ae9f-031e60471dd&title=&width=1149.6) ** Bean 实例化的基本流程 ：**
Spring容器在进行初始化时，会将xml配置的的信息封装成一个个**BeanDefinition（一个bean标签对应一个BeanDefinition对象）**对象，所有的 BeanDefinition存储到一个名为**beanDefinitionMap**的Map集合中去。在容器初始化阶段，Spring框架对该Map进行遍历，使用**反射**创建Bean实例对象，创建好的Bean对象存储在一个名为**singletonObjects**的Map集合中，当调用getBean方法时则最终从**singletonObjects**中取出Bean实例对象返回 
在容器的初始化阶段，Spring框架会取出beanDefinitionMap中的每个BeanDefinition信息，通过反射构造方法或调用指定的工厂方法 生成Bean实例对象，所以只要将BeanDefinition注册到beanDefinitionMap这个Map中，Spring就会进行对 应的Bean的实例化操作（也就是说我们可以进行手动添加！） 
之后：
 beanDefinitionMap中的**单例的Bean实例的**BeanDefinition会被转化成对应的Bean实例对象，存储到单例池singletonObjects中去

### 总结流程

**Bean 实例化的基本流程！！！** ⚫ 加载xml配置文件，解析获取配置中的每个（标签）的信息，封装成一个个的BeanDefinition对象;
⚫ 将BeanDefinition存储在一个名为beanDefinitionMap的Map<String,BeanDefinition>中;
⚫ ApplicationContext底层遍历beanDefinitionMap，创建Bean实例对象;
⚫ 创建好的Bean实例对象，被存储到一个名为singletonObjects的Map<String,Object>中;
⚫ 当执行applicationContext.getBean(beanName)时，从singletonObjects去匹配Bean实例返回。 ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682181172445-14098d6e-c1e0-4259-aa31-4b3fbabaac20.png#averageHue=%23eae7e7&clientId=u157164f7-20a8-4&from=paste&height=454&id=u1bcea425&originHeight=567&originWidth=1122&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=121224&status=done&style=shadow&taskId=u5c7b1a92-b6eb-43bc-ac04-f2049abafe6&title=&width=897.6)

## spring的后置处理器

- Spring的后处理器
  Spring的后处理器是Spring对外开发的重要扩展点，允许我们介入到Bean的整个实例化流程中来，以达到动态注册（也就是把创建好的BeanDefinition放到BeanDefinitionMap中去）BeanDefinition，动态修改BeanDefinition，以及动态修改Bean的作用。Spring主要有两种后处理器：
  ⚫ BeanFactoryPostProcessor：Bean工厂后处理器，在BeanDefinitionMap填充完毕（也就是说这个玩意他只会执行**一次**，只在BeanDefinitionMap填充完毕之后执行），Bean实例化之前执行；
  ⚫ BeanPostProcessor：Bean后处理器，一般在Bean实例化之后（也就是说这个玩意会执行**多次**，每一个bean进行实例化后都会执行），填充到单例池singletonObjects之前执行。

### BeanFactoryPostProcessor

```java
package com.haohao.postProcessor;

import com.haohao.Entity.User;
import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanDefinition;
import org.springframework.beans.factory.config.BeanFactoryPostProcessor;
import org.springframework.beans.factory.config.ConfigurableListableBeanFactory;
import org.springframework.beans.factory.support.BeanDefinitionMap;
import org.springframework.beans.factory.support.DefaultListableBeanFactory;
import org.springframework.beans.factory.support.GenericBeanDefinition;

public class MyBeanFactoryPostProcessor implements BeanFactoryPostProcessor {
    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
        System.out.println("在BeanDefinitionMap填充完毕" +
                           "（也就是说这个玩意他只会执行一次，只在BeanDefinitionMap填充完毕之后执行），" +
                           "Bean实例化之前执行,");
        System.out.println("也就是说我们可以在这里手动修改BeanDefinitionMap的信息");

        //这个类是ConfigurableListableBeanFactory的子类，有更强大的功能
        //这个类可以进行注册BeanDefinition的功能
        DefaultListableBeanFactory factory = (DefaultListableBeanFactory) beanFactory;

        //我们可以自己创建BeanDefinition
        GenericBeanDefinition beanDefinition = new GenericBeanDefinition();
        beanDefinition.setBeanClass(User.class);
        factory.registerBeanDefinition("myUser",beanDefinition);
        System.out.println("我们自己加入BeanDefinition成功");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  <!--   注册MyBeanFactoryPostProcessor-->
  <bean class="com.haohao.postProcessor.MyBeanFactoryPostProcessor"/>
</beans>
```

```java
@Test
public void test7(){
    ClassPathXmlApplicationContext c = new ClassPathXmlApplicationContext("applicationContext2.xml");
    User user = (User) c.getBean("myUser");
    System.out.println(user);
}
```

除此之外： Spring 还提供了一个BeanFactoryPostProcessor的子接口**BeanDefinitionRegistryPostProcessor**专门用于注册 BeanDefinition操作  
![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682218625761-554db12e-5163-408e-a06e-cb66821a8d32.png#averageHue=%23e8eeda&clientId=u85d420bf-4e40-4&from=paste&height=237&id=u93afbae3&originHeight=296&originWidth=1069&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=83796&status=done&style=shadow&taskId=ue10e7f67-41d5-4108-b6bd-22119dc55f2&title=&width=855.2)

图示总结： ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682221162767-383e42e3-b77b-4fa1-b179-1626cb9de125.png#averageHue=%23ede8e8&clientId=u85d420bf-4e40-4&from=paste&height=473&id=ud398ddc7&originHeight=591&originWidth=1388&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=142235&status=done&style=shadow&taskId=u41156032-c7bc-4702-9969-4fee0e4b709&title=&width=1110.4)

### BeanPostProcessor

Bean被实例化后，到最终缓存到名为singletonObjects单例池之前，中间会经过Bean的初始化过程，例如：属性的填充、初始方法init的执行等，其中有一个对外进行扩展的点BeanPostProcessor，我们称为Bean后处理。跟上面的 Bean工厂后处理器相似，它也是一个接口，实现了该接口并被容器管理的BeanPostProcessor，会在流程节点上被 Spring自动调用。  
BeanPostProcessor的接口定义如下：

```java
package org.springframework.beans.factory.config;

import org.springframework.beans.BeansException;
import org.springframework.lang.Nullable;

public interface BeanPostProcessor {

    @Nullable
    default Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }
    @Nullable
    default Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }

}
```

下面开始演示：
自定义MyBeanPostProcessor

```java
public class MyBeanPostProcessor implements BeanPostProcessor {
    /* 参数： bean是当前被实例化的Bean，beanName是当前Bean实例在容器中的名称
    返回值：当前Bean实例对象 */
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("BeanPostProcessor的before方法...");
        return bean;
    }
    /* 参数： bean是当前被实例化的Bean，beanName是当前Bean实例在容器中的名称
    返回值：当前Bean实例对象 */
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("BeanPostProcessor的after方法...");
        return bean;
    }
}
```

在核心配置文件中配置

```java
<bean class="com.itheima.processors.MyBeanPostProcessor"></bean>
```

案例 ：对Bean方法进行执行时间日志增强  
要求如下：
⚫ Bean的方法执行之前控制台打印当前时间；
⚫ Bean的方法执行之后控制台打印当前时间。
分析：
⚫ 对方法进行增强主要就是代理设计模式和包装设计模式；
⚫ 由于Bean方法不确定，所以使用动态代理在运行期间执行增强操作；
⚫ 在Bean实例创建完毕后，进入到单例池之前，使用Proxy代替真是的目标Bean
代码实现：

```java
public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
    //对Bean进行动态代理，返回的是Proxy代理对象
    Object proxyBean = Proxy.newProxyInstance(bean.getClass().getClassLoader(),
                                              bean.getClass().getInterfaces(),
                                              (Object proxy, Method method, Object[] args) -> {
                                                  long start = System.currentTimeMillis();
                                                  System.out.println("开始时间：" + new Date(start));
                                                  //执行目标方法
                                                  Object result = method.invoke(bean, args);
                                                  long end = System.currentTimeMillis();
                                                  System.out.println("结束时间：" + new Date(end));
                                                  return result;
                                              });
    //返回代理对象
    return proxyBean;
}
```

之后把这个类配置到核心配置类中
可以进行验证了，创建的每一个bean调用的每一个方法都会打印运行时间 ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682227795171-c42c375e-df62-4c98-901d-4d5f8120230b.png#averageHue=%23ede8e7&clientId=u85d420bf-4e40-4&from=paste&height=382&id=u1a840cff&originHeight=477&originWidth=1155&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=124291&status=done&style=shadow&taskId=u71f56fdc-3ac5-430c-8ff9-280f33ed7ca&title=&width=924)

## Spring的Bean的生命周期

Spring Bean的生命周期是从 Bean 实例化之后，即通过反射创建出对象之后，到Bean成为一个完整对象，最终存储到单例池中，这个过程被称为Spring Bean的生命周期。Spring Bean的生命周期大体上分为三个阶段：
⚫ Bean的实例化阶段：Spring框架会**取出BeanDefinition的信息进行判断**当前Bean的范围是否是singleton的，是否不是延迟加载的，是否不是FactoryBean等，最终将一个普通的singleton的Bean通过反射进行实例化；
⚫ Bean的初始化阶段：Bean创建之后还仅仅是个"半成品"，还需要对Bean实例的**属性进行填充（这里和suns讲的不一样）**、执行一些Aware接口方法、执行BeanPostProcessor方法、执行InitializingBean接口的初始化方法、执行自定义初始化init方法等。该阶段是Spring最具技术含量和复杂度的阶段，Aop增强功能，后面要学习的Spring的注解功能等、spring高频面试题Bean的循环引用问题都是在这个阶段体现的；
⚫ Bean的完成阶段：经过初始化阶段，Bean就成为了一个完整的Spring Bean，被存储到单例池singletonObjects中去了，即完成了Spring Bean的整个生命周期。

### 初始化阶段

由于Bean的初始化阶段的步骤比较复杂，所以着重研究Bean的初始化阶段
Spring Bean的初始化过程涉及如下几个过程：
⚫ Bean实例的属性填充
⚫ Aware接口属性注入
⚫ BeanPostProcessor的before()方法回调
⚫ InitializingBean接口的初始化方法回调
⚫ 自定义初始化方法init回调
⚫ BeanPostProcessor的after()方法回调

#### Bean实例属性填充

BeanDefinition 中有对当前Bean实体的注入信息通过属性propertyValues进行了存储，
例如UserService的属性信息如下:  
![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682237988311-a5b19546-94d9-4b7f-ba96-a38821fc53f5.png#averageHue=%23e2d5c6&clientId=u85d420bf-4e40-4&from=paste&height=266&id=u002bf305&originHeight=332&originWidth=1025&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=200137&status=done&style=shadow&taskId=uef7f1913-6b25-4c1c-bea8-8a9d81a6da1&title=&width=820) 我们可以发现，对userservice对象进行了属性填充：
但是我们不仅仅可以填充userDao对象
还可以填充其他对象，这就出现了一些问题：

Spring在进行属性注入时，会分为如下几种情况：
⚫ 注入普通属性，String、int或存储基本类型的集合时，直接通过set方法的反射设置进去；
⚫ 注入单向对象引用属性时，从容器中getBean获取后通过set方法反射设置进去，如果容器中没有，则先创建被注入对象Bean实例（完成整个生命周期）后，在进行注入操作；
⚫ 注入双向对象引用属性时，就比较复杂了，涉及了循环引用（循环依赖）问题，下面会详细阐述解决方案。

##### 循环引用问题

多个实体之间相互依赖并形成闭环的情况就叫做"循环依赖"，也叫做"循环引用"

```java
public class UserServiceImpl implements UserService{
    UserDao userDao;
    public void setUserDao(UserDao userDao) {
        this.userDao=userDao;
    }
}
public class UserDaoImpl implements UserDao{
    UserService userService;
    public void setUserService(UserService userService){
        this.userService=userService;
    }
}
```

这种情况很少出现，但是也有可能出现，有可能的话，spring这个框架就得考虑到这种情况。。。。
画图来描述这种情况： ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682238631425-637cf79e-fe82-481f-8337-09594536155c.png#averageHue=%23f5f2f0&clientId=u85d420bf-4e40-4&from=paste&height=433&id=uf62926a4&originHeight=541&originWidth=1284&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=235270&status=done&style=shadow&taskId=udca40fd2-20fc-4df5-a1dc-0536f094889&title=&width=1027.2) 所以为了解决这个问题，出现了三级缓存的概念，其实就是三个map，然后存储不同时刻的beanMap集合
Spring提供了三级缓存存储 完整Bean实例 和 半成品Bean实例 ，用于解决循环引用问题
在DefaultListableBeanFactory的上四级父类DefaultSingletonBeanRegistry中提供如下三个Map：

```java
public class DefaultSingletonBeanRegistry ... {
    //1、最终存储单例Bean成品的容器，即实例化和初始化都完成的Bean，称之为"一级缓存"
    Map<String, Object> singletonObjects = new ConcurrentHashMap(256);
    //2、早期Bean单例池，缓存半成品对象，且当前对象已经被其他对象引用了，称之为"二级缓存"
    Map<String, Object> earlySingletonObjects = new ConcurrentHashMap(16);
    //3、单例Bean的工厂池，缓存半成品对象，对象未被引用，使用时在通过工厂创建Bean，称之为"三级缓存"
    Map<String, ObjectFactory<?>> singletonFactories = new HashMap(16);
}
```

注意上面有一个`Map<String, ObjectFactory<?>> singletonFactories = new HashMap(16);` 也就是第三级缓存，这里的value是ObjectFactory ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682239564137-ff9a3c31-6847-497a-9e2b-9867b4176a51.png#averageHue=%232e2c2b&clientId=u85d420bf-4e40-4&from=paste&height=325&id=uc8fdfefa&originHeight=406&originWidth=840&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=55856&status=done&style=shadow&taskId=u7070b223-48f5-4335-baee-9c5f83b9a4e&title=&width=672)

UserService和UserDao循环依赖的过程结合上述三级缓存描述一下

- ⚫ UserService 实例化对象，但尚未初始化，将UserService存储到三级缓存；
- ⚫ UserService 属性注入，需要UserDao，从缓存中获取，没有UserDao；
- ⚫ UserDao实例化对象，但尚未初始化，将UserDao存储到到三级缓存；
- ⚫ UserDao属性注入，需要UserService，从三级缓存获取UserService，UserService从三级缓存移入二级缓存；
- ⚫ UserDao执行其他生命周期过程，最终成为一个完成Bean，存储到一级缓存，删除二三级缓存；
- ⚫ UserService 注入UserDao；
- ⚫ UserService执行其他生命周期过程，最终成为一个完成Bean，存储到一级缓存，删除二三级缓存。

[三级缓存源码剖析流程.pdf](https://www.yuque.com/attachments/yuque/0/2023/pdf/27086425/1682258666515-fe345e8f-3e20-4ef2-b01d-2eab8979a048.pdf)

#### Aware接口属性注入

Aware接口是一种框架辅助属性注入的一种思想，其他框架中也可以看到类似的接口。框架具备高度封装性，我们接触到的一般都是业务代码，一个底层功能API不能轻易的获取到，但是这不意味着永远用不到这些对象，如果用到了 ，就可以使用框架提供的类似Aware的接口，让框架给我们注入该对象。  
常用的Aware接口 ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682258807687-ba620f59-362b-4bf5-90d2-149ddc84473e.png#averageHue=%23becce1&clientId=u66d32670-5100-4&from=paste&height=300&id=ue9dc9fff&originHeight=375&originWidth=1690&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=146630&status=done&style=shadow&taskId=u0a1cafa7-5f7b-46cf-be83-36980e2b970&title=&width=1352)

## Spring IoC容器生命周期过程总结

![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682259680635-0183d46f-23a6-4921-8502-b3839a31ca38.png#averageHue=%23efeeee&clientId=u66d32670-5100-4&from=paste&height=505&id=ubf381083&originHeight=631&originWidth=1325&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=360621&status=done&style=shadow&taskId=ub4325004-dd56-4693-8270-7e0af0c912d&title=&width=1060)

## Spring整合其他框架的方式

Spring xml方式整合第三方框架
xml整合第三方框架有两种整合方案：
⚫ 不需要自定义名空间，不需要使用Spring的配置文件配置第三方框架本身内容，例如：MyBatis；
⚫ 需要引入第三方框架命名空间，需要使用Spring的配置文件配置第三方框架本身内容，例如：Dubbo。

### 不需要自定义命名空间

#### MyBatis整合Spring

Spring整合MyBatis的步骤如下：
⚫ 导入MyBatis整合Spring的相关坐标；
⚫ 编写Mapper和Mapper.xml；
⚫ 配置SqlSessionFactoryBean和MapperScannerConfigurer；
⚫ 编写测试代码

```xml
<!--配置数据源-->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
  <property name="url" value="jdbc:mysql://localhost:3306/mybatis"></property>
  <property name="username" value="root"></property>
  <property name="password" value="root"></property>
</bean>
<!--配置SqlSessionFactoryBean-->
<bean class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource"></property>
</bean>
<!--配置Mapper包扫描-->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
  <property name="basePackage" value="com.itheima.dao"></property>
</bean>
```

```java
public interface UserMapper {
    List<User> findAll();
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.dao.UserMapper">
  <select id="findAll" resultType="com.itheima.pojo.User">
    select * from tb_user
  </select>
</mapper>
```

```java
ClassPathxmlApplicationContext applicationContext =new ClassPathxmlApplicationContext("applicationContext.xml");
UserMapper userMapper = applicationContext.getBean(UserMapper.class);
List<User> all = userMapper.findAll();
System.out.println(all);
```

##### Spring整合MyBatis的原理剖析

整合包里提供了一个SqlSessionFactoryBean和一个扫描Mapper的配置对象，SqlSessionFactoryBean一旦被实例化，就开始扫描Mapper并通过动态代理产生Mapper的实现类存储到Spring容器中。相关的有如下四个类：
⚫ SqlSessionFactoryBean：需要进行配置，用于提供SqlSessionFactory；
⚫ MapperScannerConfigurer：需要进行配置，用于扫描指定mapper注册BeanDefinition；
⚫ MapperFactoryBean：Mapper的FactoryBean，获得指定Mapper时调用getObject方法；
⚫ ClassPathMapperScanner：definition.setAutowireMode(2) 修改了自动注入状态，所以MapperFactoryBean中的setSqlSessionFactory会自动注入进去。

###### SqlSessionFactoryBean

配置SqlSessionFactoryBean作用是向容器中提供SqlSessionFactory，SqlSessionFactoryBean实现了 FactoryBean和InitializingBean两个接口，所以会自动执行getObject() 和afterPropertiesSet()方法

```java
SqlSessionFactoryBean implements FactoryBean<SqlSessionFactory>, InitializingBean{
    public void afterPropertiesSet() throws Exception {
        //创建SqlSessionFactory对象
        this.sqlSessionFactory = this.buildSqlSessionFactory();
}
    public SqlSessionFactory getObject() throws Exception {
        return this.sqlSessionFactory;
    }
}
```

###### MapperScannerConfigurer

配置MapperScannerConfigurer作用是扫描Mapper，向容器中注册Mapper对应的MapperFactoryBean， MapperScannerConfigurer实现了BeanDefinitionRegistryPostProcessor和InitializingBean两个接口，会在 postProcessBeanDefinitionRegistry方法中向容器中注册MapperFactoryBean

```java
class MapperScannerConfigurer implements BeanDefinitionRegistryPostProcessor, InitializingBean{
    public void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry) {
        ClassPathMapperScanner scanner = new ClassPathMapperScanner(registry);
        scanner.scan(StringUtils.tokenizeToStringArray(this.basePackage, ",; \t\n"));
    }
}
```

###### ClassPathMapperScanner

```java
class ClassPathMapperScanner extends ClassPathBeanDefinitionScanner {
    public Set<BeanDefinitionHolder> doScan(String... basePackages) {
        Set<BeanDefinitionHolder> beanDefinitions = super.doScan(basePackages);
        if (beanDefinitions.isEmpty()) {
        } else {
            this.processBeanDefinitions(beanDefinitions);
        }
    }
    private void processBeanDefinitions(Set<BeanDefinitionHolder> beanDefinitions) {
        //设置Mapper的beanClass是org.mybatis.spring.mapper.MapperFactoryBean
        definition.setBeanClass(this.mapperFactoryBeanClass);
        //PS：autowireMode取值：1是根据名称自动装配，2是根据类型自动装配
        definition.setAutowireMode(2); //设置MapperBeanFactory 进行自动注入
    }
}
```

###### ClassPathBeanDefinitionScanner

```java
class ClassPathBeanDefinitionScanner{
    public int scan(String... basePackages) {
        this.doScan(basePackages);
    }
    protected Set<BeanDefinitionHolder> doScan(String... basePackages) {
        //将扫描到的类注册到beanDefinitionMap中，此时beanClass是当前类全限定名
        this.registerBeanDefinition(definitionHolder, this.registry);
        return beanDefinitions;
    }
}
```

###### MapperFactoryBean

```java
UserMapper userMapper = applicationContext.getBean(UserMapper.class);
```

```java
public class MapperFactoryBean<T> extends SqlSessionDaoSupport implements FactoryBean<T> {
    public MapperFactoryBean(Class<T> mapperInterface) {
        this.mapperInterface = mapperInterface;
    }
    public void setSqlSessionFactory(SqlSessionFactory sqlSessionFactory) {
        this.sqlSessionTemplate = this.createSqlSessionTemplate(sqlSessionFactory);
    }
    public T getObject() throws Exception {
        return this.getSqlSession().getMapper(this.mapperInterface);
    }
}
```

### 需要引入第三方框架命名空间

需求：加载外部properties文件，将键值对存储在Spring容器中  
![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682307069820-4a6e9b3b-89f1-4cef-b59a-a95be309d976.png#averageHue=%23f0eacc&clientId=u9fc06465-ad3c-4&from=paste&height=414&id=uc81fe3a1&originHeight=518&originWidth=1183&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=507254&status=done&style=shadow&taskId=u8a50bbd3-1bf8-4c25-a337-c60f77350bc&title=&width=946.4) 其实，加载的properties文件中的属性最终通过Spring解析后会被存储到了Spring容器的environment中去，不仅自己定义的属性会进行存储，Spring也会把环境相关的一些属性进行存储  
![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682314348228-cfc1c4ad-52fd-4e1d-9073-50e3b9da2b1e.png#averageHue=%23e1d8cb&clientId=ub898e76a-eb6b-4&from=paste&height=355&id=u7b4d83b3&originHeight=444&originWidth=1375&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=536856&status=done&style=shadow&taskId=ue2a4763b-ec8e-460c-8706-2db97f968f5&title=&width=1100)

#### 自定义命名空间标签

下面这个部分是讲的是自定义标签（也就是有命名空间头信息的标签！虽然也没咋用过）
首先了解这个：
spring.schemas ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1680925844793-30782bd1-a20c-49bf-96b9-f00c361bd1cf.png#averageHue=%238a683a&clientId=u51daac5d-73c9-4&from=paste&height=542&id=u5124f944&originHeight=677&originWidth=1875&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=278217&status=done&style=shadow&taskId=u41ce2e52-9817-4d31-81b7-9ff3aefc8c2&title=&width=1500)

```xml
<!--
<mvc:annotation-driven>
Spring MVC用来提供Controller请求转发，
json自动转换等功能。，默认会帮我们注册默认处理请求，参数和返回值的类
-->
<!--
例如：写一个最简单的返回ModelAndView的Controller，不加annotation-driven，你会发现它是可以运行的，
但如果改成返回Json对象加上ResponseBody就会报错，这正好映证了文档中的说法
-->
```

![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1680924459562-5572e2fe-b0e5-49a0-b032-d7f9c9fefde0.png#averageHue=%232e2d2c&clientId=u51daac5d-73c9-4&from=paste&height=375&id=u94cdd570&originHeight=469&originWidth=1235&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=90247&status=done&style=shadow&taskId=u7debed27-eb5c-4746-86d2-b63989a87a7&title=&width=988) 下面这个handler的意思是如果你写了mvc这个命名空间，就会被这个handler给处理 ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1680923891721-e4795a85-910f-495b-92b8-b47a8c5d2b55.png#averageHue=%23727c50&clientId=u51daac5d-73c9-4&from=paste&height=138&id=u451bd45d&originHeight=172&originWidth=1448&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=29502&status=done&style=shadow&taskId=udec73a0d-a3c8-4d51-b781-dfaea2c351d&title=&width=1158.4) 看一下这个handler，这个handler的功能就是：根据你使用的不同标签，创建不同的解析器，我们上面就用到的是annotation-driven标签 ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1680924759865-bf10c178-1d61-4e82-a4e0-1328e66793f8.png#averageHue=%232f2e2d&clientId=u51daac5d-73c9-4&from=paste&height=515&id=u0b9249c5&originHeight=644&originWidth=1487&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=184405&status=done&style=shadow&taskId=u4abae417-fdb9-4b31-a2b3-362616b33ab&title=&width=1189.6) 看一下这个解析器具体干啥了 ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1680926030828-f3e7e84e-4eb3-4f53-a578-45abaff40c01.png#averageHue=%232e2c2b&clientId=u51daac5d-73c9-4&from=paste&height=578&id=u602d98be&originHeight=723&originWidth=1343&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=129385&status=done&style=shadow&taskId=u7fa0d060-1291-4f3f-8fe7-dc90a7dafbf&title=&width=1074.4) 总结来说就是通过handler->parser->beanDefination

ok,下面我们来**自己实现一个自定义标签**。 `<haohao:user id="" name="" password=""/>` 文件结构： ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1680933833660-a89ea77f-a5e6-493e-8d62-f62930746ee2.png#averageHue=%2349544a&clientId=u51daac5d-73c9-4&from=paste&height=574&id=udbb0cdbc&originHeight=717&originWidth=346&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=31095&status=done&style=shadow&taskId=u7b3e81c2-2d5a-44fd-b9c7-523ec604ec0&title=&width=276.8)

```xml
http\://www.haohao.com/schema/user=com.haohao.handler.UserHandler
```

```xml
http\://www.haohao.com/schema/user.xsd=META-INF/user.xsd
```

```java
public class UserHandler extends NamespaceHandlerSupport {
    @Override
    public void init() {
        registerBeanDefinitionParser("user",new MyUserParser());
    }
}
```

```java
public class MyUserParser extends AbstractSingleBeanDefinitionParser {
    @Override
    protected Class<?> getBeanClass(Element element) {
        return User.class;
    }

    @Override
    protected void doParse(Element element, BeanDefinitionBuilder builder) {
        String name = element.getAttribute("name");
        String password = element.getAttribute("password");

        builder.addPropertyValue("name",name);
        builder.addPropertyValue("password",password);
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:haohao="http://www.haohao.com/schema/user"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.haohao.com/schema/user
  http://www.haohao.com/schema/user.xsd">

  <haohao:user id="u" name="haohao" password="123"/>
</beans>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<schema xmlns="http://www.w3.org/2001/XMLSchema"
  targetNamespace="http://www.haohao.com/schema/user"
  xmlns:tns="http://www.haohao.com/schema/user"
  elementFormDefault="qualified">
  <element name="user">
    <complexType>
      <attribute name="id" type="string"/>
      <attribute name="name" type="string"/>
      <attribute name="password" type="string"/>
    </complexType>
  </element>
</schema>
```

```java
@Test
public void test3(){
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
    Object u = applicationContext.getBean("u");
    System.err.println(u);
}
```

其中有很多对应关系，我真的弄了好久好久

总结一下步骤：Spring xml方式整合第三方框架
步骤分析：

1. 确定命名空间名称、schema虚拟路径、标签名称；
2. 编写schema约束文件haohao-annotation.xsd
3. 在类加载路径下创建META目录，编写约束映射文件spring.schemas和处理器映射文件spring.handlers
4. 编写命名空间处理器 HaohaoNamespaceHandler，在init方法中注册HaohaoBeanDefinitionParser
5. 编写标签的解析器 HaohaoBeanDefinitionParser，在parse方法中注册HaohaoBeanPostProcessor
6. 编写HaohaoBeanPostProcessor
   ==========以上五步是框架开发者写的，以下是框架使用者写的===========
7. 在applicationContext.xml配置文件中引入命名空间
8. 在applicationContext.xml配置文件中使用自定义的标签

#### 总结

通过上述分析，我们清楚的了解了外部命名空间标签的执行流程，如下：
⚫ 将自定义标签的约束 与 物理约束文件与网络约束名称的约束 以键值对形式存储到一个spring.schemas文件里，该文件存储在类加载路径的 META-INF里，Spring会自动加载到;
⚫ 将自定义命名空间的名称 与 自定义命名空间的处理器映射关系 以键值对形式存在到一个叫spring.handlers文件里，该文件存储在类加载路径的 META-INF里，Spring会自动加载到;
⚫ 准备好NamespaceHandler，如果命名空间只有一个标签，那么直接在parse方法中进行解析即可，一般解析结果就是注册该标签对应的BeanDefinition。如果命名空间里有多个标签，那么可以在init方法中为每个标签都注册一个BeanDefinitionParser，在执行NamespaceHandler的parse方法时在分流给不同的BeanDefinitionParser进行解析(重写doParse方法即可)

## 基于注解的Spring应用

### bean标签属性的注解

![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682315753089-cce25889-92e6-44af-8949-3f042d73fb72.png#averageHue=%23e9efdb&clientId=ub898e76a-eb6b-4&from=paste&height=199&id=ua69e59f4&originHeight=249&originWidth=1023&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=107391&status=done&style=shadow&taskId=ue8dcd594-9f23-4692-be7e-778bb5a8ded&title=&width=818.4) ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682315828602-5e7f4019-8ca1-4c97-9c8e-1a65dee7c83b.png#averageHue=%23e9efdc&clientId=ub898e76a-eb6b-4&from=paste&height=351&id=u321944fb&originHeight=439&originWidth=1027&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=169916&status=done&style=shadow&taskId=u053e5c11-3645-4bdf-b29b-ba04282f75b&title=&width=821.6) 具体使用：

```java
@Component("userDao")
@Scope("singleton")
@Lazy(true)
public class UserDaoImpl implements UserDao{
    @PostConstruct
    public void init(){}
    @PreDestroy
    public void destroy(){}
}
```

### @Primary注解

@Primary注解用于标注相同类型的Bean优先被使用权，@Primary 是Spring3.0引入的，与@Component 和@Bean一起使用，标注该Bean的优先级更高，则在通过类型获取Bean或通过@Autowired根据类型进行注入时， 会选用优先级更高的

```java
@Component("userDao")
public class UserDaoImpl implements UserDao{}
@Component("userDao2")
@Primary
public class UserDaoImpl2 implements UserDao{}
```

```java
@Bean
public UserDao userDao01(){return new UserDaoImpl();}
@Bean
@Primary
public UserDao userDao02(){return new UserDaoImpl2();}
```

### @Profile 注解

@Profile 注解的作用同于xml配置时学习profile属性，是进行环境切换使用的

```java
<beans profile="test">
```

注解 @Profile 标注在类或方法上，标注当前产生的Bean从属于哪个环境，只有激活了当前环境，被标注的Bean才 能被注册到Spring容器里，不指定环境的Bean，任何环境下都能注册到Spring容器里

```java
@Repository("userDao")
@Profile("test")
public class UserDaoImpl implements UserDao{}
@Repository("userDao2")
public class UserDaoImpl2 implements UserDao{}
```

可以使用以下两种方式指定被激活的环境：
⚫ 使用命令行动态参数，虚拟机参数位置加载 -Dspring.profiles.active=test
⚫ 使用代码的方式设置环境变量 System.setProperty("spring.profiles.active","test");

### Spring注解的解析原理

使用@Component等注解配置完毕后，要配置组件扫描才能使注解生效  
⚫ xml配置组件扫描：

```java
<context:component-scan base-package="com.itheima"/>
```

⚫ 配置类配置组件扫描：

```java
@Configuration
@ComponentScan("com.itheima")
public class AppConfig {
}
```

#### xml配置组件扫描

使用xml方式配置组件扫描，而component-scan是一个context命名空间下的自定义标签，所以要找到对应的**命名空间处理器NamespaceHandler** 和 **解析器**，查看spring-context包下的spring.handlers文件

```properties
http\://www.springframework.org/schema/context=org.springframework.context.config.ContextNamespaceHandler
```

可以看见，这个处理器为ContextNamespaceHandler ，所以我们查看 ContextNamespaceHandler 类

```java
public void init() {
    this.registerBeanDefinitionParser("component-scan", new ComponentScanBeanDefinitionParser());
}
```

将ComponentScanBeanDefinitionParser进行了注册，对其源码进行跟踪，最终将标注的@Component的类，生成对应的BeanDefiition进行了注册

#### 使用配置类配置组件扫描

使用配置类配置组件扫描，使用AnnotationConfigApplicationContext容器在进行创建时，内部调用了如下代码， 该工具注册了几个Bean后处理器

```java
AnnotationConfigUtils.registerAnnotationConfigProcessors(this.registry);
```

![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682325584767-5ce696ba-076a-4450-bbed-badf0f11218c.png#averageHue=%23f8f6f4&clientId=ub898e76a-eb6b-4&from=paste&height=389&id=u75669ae6&originHeight=486&originWidth=1110&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=410005&status=done&style=shadow&taskId=u990a54f7-c8aa-4c6f-b81a-7941b1abfe1&title=&width=888) 其中，ConfigurationClassPostProcessor 是 一个 BeanDefinitionRegistryPostProcessor(这个就是之前我们见过的可以往beanDefinitionMap中注册bean的类。。，这个ConfigurationClassPostProcessor是BeanDefinitionRegistryPostProcessor的子类) ，经过一系列源码调用，最终也别指定到了 ClassPathBeanDefinitionScanner 的 doScan 方法（与xml方式最终终点一致）

#### 两者对比

![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682325721808-3beca5ad-cbb8-43e0-a173-444d79a21d2e.png#averageHue=%23f9f4f4&clientId=ub898e76a-eb6b-4&from=paste&height=407&id=ubaa40071&originHeight=509&originWidth=822&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=70181&status=done&style=shadow&taskId=u3b775d18-e3e4-4cc2-8a6b-6095be6d986&title=&width=657.6)

## Spring注解方式整合第三方框架

第三方框架整合，依然使用MyBatis作为整合对象，之前我们已经使用xml方式整合了MyBatis，现在使用注解方式无非就是将xml标签替换为注解，将xml配置文件替换为配置类而已，原有xml方式整合配置如下：

```xml
<!--配置数据源-->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
  <property name="url" value="jdbc:mysql://localhost:3306/mybatis"></property>
  <property name="username" value="root"></property>
  <property name="password" value="root"></property>
</bean>
<!--配置SqlSessionFactoryBean-->
<bean class="org.mybatis.spring.SqlSessionFactoryBean">
  <property name="dataSource" ref="dataSource"></property>
</bean>
<!--配置Mapper包扫描-->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
  <property name="basePackage" value="com.itheima.dao"></property>
</bean>
```

使用@Bean将DataSource和SqlSessionFactoryBean存储到Spring容器中，而MapperScannerConfigurer使用注 解@MapperScan进行指明需要扫描的Mapper在哪个包下，使用注解整合MyBatis配置方式如下：

```java
@Configuration
@ComponentScan("com.itheima")
@MapperScan("com.itheima.mapper")
public class ApplicationContextConfig {
    @Bean
    public DataSource dataSource(){
        DruidDataSource dataSource = new DruidDataSource();
        //省略部分代码
        return dataSource;}
    @Bean
    public SqlSessionFactoryBean sqlSessionFactoryBean(DataSource dataSource){
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(dataSource);
        return sqlSessionFactoryBean;
    }
}
```

注解方式，Spring整合MyBatis的原理，关键在于@MapperScan，@MapperScan不是Spring提供的注解，是 MyBatis为了整合Spring，在整合包org.mybatis.spring.annotation中提供的注解，源码如下：

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE})
@Documented
@Import({MapperScannerRegistrar.class})
@Repeatable(MapperScans.class)
public @interface MapperScan {
    String[] value() default {};
    String[] basePackages() default {};
    Class<?>[] basePackageClasses() default {};
    Class<? extends Annotation> annotationClass() default Annotation.class;
    // ... ...
}
```

重点关注一下@Import({MapperScannerRegistrar.class})，当@MapperScan被扫描加载时，会解析@Import注 解，从而加载指定的类，此处就是加载了MapperScannerRegistrar  
MapperScannerRegistrar实现了ImportBeanDefinitionRegistrar接口，Spring会自动调用 registerBeanDefinitions方法，该方法中又注册MapperScannerConfigurer类，而MapperScannerConfigurer类 作用是扫描Mapper，向容器中注册Mapper对应的MapperFactoryBean，前面讲过，此处不在赘述了：

```java
public class MapperScannerRegistrar implements ImportBeanDefinitionRegistrar, 
    ResourceLoaderAware {
    //默认执行registerBeanDefinitions方法
    void registerBeanDefinitions(AnnotationMetadata annoMeta, AnnotationAttributes annoAttrs, 
                                 BeanDefinitionRegistry registry, String beanName) {
        BeanDefinitionBuilder builder = BeanDefinitionBuilder.genericBeanDefinition(MapperScannerConfigurer.class);
        //... 省略其他代码 ...
        //注册BeanDefinition
        registry.registerBeanDefinition(beanName, builder.getBeanDefinition());
    }
}
```

Spring与MyBatis注解方式整合有个重要的技术点就是@Import，第三方框架与Spring整合xml方式很多是凭借自定义标签完成的，而第三方框架与Spring整合注解方式很多是靠@Import注解完成的。
@Import可以导入如下三种类：
⚫ 普通的配置类
⚫ 实现ImportSelector接口的类
⚫ 实现ImportBeanDefinitionRegistrar接口的类

@Import导入实现了ImportSelector接口的类

```java
@Configuration
@ComponentScan("com.itheima")
@Import({MyImportSelector.class})
public class ApplicationContextConfig {
}
```

```java
public class MyImportSelector implements ImportSelector {
    @Override
    public String[] selectImports(AnnotationMetadata annotationMetadata) {
        //返回要进行注册的Bean的全限定名数组
        return new String[]{User2.class.getName()};
    }
}
```

ImportSelector接口selectImports方法的参数AnnotationMetadata代表注解的媒体数据，可以获得当前注解修饰 的类的元信息，例如：获得组件扫描的包名

```java
public class MyImportSelector implements ImportSelector {
    @Override
    public String[] selectImports(AnnotationMetadata annotationMetadata) {
        //获得指定类型注解的全部信息
        Map<String, Object> annotationAttributes = 
            annotationMetadata.getAnnotationAttributes(ComponentScan.class.getName());
        //获得全部信息中basePackages信息
        String[] basePackages = (String[]) annotationAttributes.get("basePackages");
        //打印结果是com.itheima
        System.out.println(basePackages[0]);
        return new String[]{User2.class.getName()};
    }
}
```

@Import导入实现ImportBeanDefinitionRegistrar接口的类，实现了该接口的类的registerBeanDefinitions方法 会被自动调用，在该方法内可以注册BeanDefinition

```java
public class MyImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {
    @Override
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, 
                                        BeanDefinitionRegistry registry) {
        //使用给定的BeanDefinitionRegistry参数，手动注册BeanDefinition
        BeanDefinition beanDefinition = new RootBeanDefinition();
        beanDefinition.setBeanClassName("com.itheima.pojo.User2");
        registry.registerBeanDefinition("user2",beanDefinition);
    }
}
```

## SpringAOP

其实在之前学习BeanPostProcessor时，在BeanPostProcessor的after方法中使用动态代理对Bean进行了增强，实际存储到单例池singleObjects中的不是当前目标对象本身，而是当前目标对象的代理对象Proxy，这样在调用目标对象方法时，实际调用的是代理对象Proxy的同名方法，起到了目标方法前后都进行增强的功能。

- aspectj静态代理
  - 原理：在编译期织入代码，编译成class文件
  - 优点:可以增强任何类，任何方法，包括（final,static修饰）
  - 缺点:需要使用aspecj提供的Ajc编译器来编译Aj文件
- jdk动态代理
  - 使用jdk的动态代理来增强接口实现类
  - 原理：使用Proxy类的newProxyInstance方法运行期通过反射动态的生成代理对象
  - 优点：不需要修改具体的业务代码，动态的增强方法，降低耦合性。
  - 缺点：代理的对象必须有接口实现。
- cglib的动态代理
  - 可以对jdk的动态代理作为补充
  - 原理：以创建目标类的子类来生成动态代理对象。
  - 优点：不需要修改具体的业务代码，动态的增强方法，降低耦合性。
  - 缺点：不能对final修饰的类，final修饰的方法或static的方法进行代理

AOP（面向切面编程）是一种编程范式，它可以让开发者在不修改原有代码的情况下，通过定义切面和切点来增强程序的功能。例如，可以在方法执行前、执行后或异常时插入切面逻辑，实现日志记录、性能监控、事务管理等功能。
在Java中，AOP框架可以分为基于**代理的AOP**和**基于字节码生成的AOP**两种实现方式。
Spring AOP是基于代理的AOP框架，它可以通过**Java动态代理**或**CGLIB代理**来实现对目标类方法的代理。**Java动态代理只能代理实现接口的类，而CGLIB代理可以代理任意的类。**Spring AOP默认使用Java动态代理来实现AOP，但当目标类没有实现接口时，它会自动切换到使用CGLIB代理。Spring AOP的切面和切点可以通过注解或XML配置文件来定义。例如，可以使用@Aspect注解来定义一个切面类，并使用@Pointcut注解来定义一个切点。在定义完切面和切点后，可以使用@Before、@After、@Around等注解来定义切面逻辑，并将切面类注册到Spring容器中。
AspectJ是一个独立的AOP框架，它提供了比Spring AOP更强大和灵活的AOP能力。AspectJ支持基于编译器、类加载器和运行时三种不同的AOP实现方式。其中，基于编译器和类加载器的实现方式需要在**编译时**或加载时对Java字节码进行增强，而基于**运行时**的实现方式则可以动态地对Java类进行增强。与Spring AOP不同，AspectJ使用**注解**或**AspectJ语言（AJ）**来定义切面和切点。例如，可以使用@Aspect注解来定义一个切面类，并使用@Pointcut注解来定义一个切点。在定义完切面和切点后，可以使用@Before、@After、@Around等注解或AJ语言来定义切面逻辑。
CGLIB是一个Java字节码生成库，它可以在**运行时**动态生成Java类，并在**内存**中加载。CGLIB动态代理是基于**字节码生成**的动态代理技术，它可以**代理任意的类**，包括**没有实现接口的类**。CGLIB动态代理**通过生成目标类的子类**来创建代理对象，代理类中的方法会调用MethodInterceptor接口的intercept方法，并在其中实现切面逻辑。
JDK动态代理是Java原生支持的动态代理技术，它是通过反射和接口实现的。JDK动态代理要求目标类必须是**实现了接口的类**，它会在**运行时**创建一个实现了代理接口的代理类，代理类中的方法会调用InvocationHandler接口的invoke方法，并在其中实现切面逻辑。

需要注意的是，由于**CGLIB动态代理**是基于字节码生成的技术，它需要在**运行时**生成**代理类的字节码**，并在**内存**中加载。因此，CGLIB动态代理可能会比JDK动态代理更耗时和占用更多的内存。同时，CGLIB动态代理也无法代理final方法和final类。
总之，Spring AOP、AspectJ和CGLIB都是Java中的AOP框架，它们可以用来实现切面编程。Spring AOP是Spring框架内置的AOP框架，基于代理实现；AspectJ是一个独立的AOP框架，提供更强大和灵活的AOP能力；CGLIB是一个Java字节码生成库，用于生成代理类。

### 基于JDK动态代理实现AOP的原始案例

现在给一个需求：用JDK动态代理的方式，使用BeanPostProcessor实现对**com.itheima.service.impl包下的所有类的所有方法**实现AOP代理

```java
public class MyAdvice {
    public void beforeAdvice(JoinPoint joinPoint){
        System.out.println("前置的增强....");
    }
    public void afterAdvice(){
        System.out.println("最终的增强....");
    }
}
```

```java
public class UserServiceImpl implements UserService {
    @Override
    public void show1() {
        System.out.println("show1....");
    }
}
```

```java
public class MockAopBeanPostProcessor implements BeanPostProcessor, ApplicationContextAware {
    private ApplicationContext applicationContext;
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        //目的：对UserServiceImpl中的show1和show2方法进行增强，增强方法存在与MyAdvice中
        //问题1：筛选service.impl包下的所有的类的所有方法都可以进行增强，解决方案if-else
        //问题2：MyAdvice怎么获取到？解决方案：从Spring容器中获得MyAdvice
        if(bean.getClass().getPackage().getName().equals("com.itheima.service.impl")){
            //生成当前Bean的Proxy对象,这里使用JDK的动态代理
            Object beanProxy = Proxy.newProxyInstance(
                    bean.getClass().getClassLoader(),
                    bean.getClass().getInterfaces(),
                    (Object proxy, Method method, Object[] args) -> {
                        MyAdvice myAdvice = applicationContext.getBean(MyAdvice.class);
                        //执行增强对象的before方法
                        myAdvice.beforeAdvice();
                        //执行目标对象的目标方法
                        Object result = method.invoke(bean, args);
                        //执行增强对象的after方法
                        myAdvice.afterAdvice();
                        return result;
                    }
            );
            return beanProxy;
        }
        //如果不是这个包下面的类，就不会创建代理对象，还返回原来的对象
        return bean;
    }
    //Aware的接口就是干这个的，全局都可以获取这些对象
    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.applicationContext = applicationContext;
    }
}
```

需要注意的是，这三个类都需要注册到spring容器中才能完成这些操作

```java
ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
UserService bean = app.getBean(UserService.class);
bean.show1();
```

### 基于xml配置的AOP

#### 第一种方式：aspect

```java
public interface UserService {
    void show1();
    void show2();
}
public class UserServiceImpl implements UserService {
    public void show1() {
        System.out.println("show1...");
    }
    public void show2() {
        System.out.println("show2...");
    }
}
```

```java
public class MyAdvice {
    public void beforeAdvice(){
        System.out.println("beforeAdvice");
    }
    public void afterAdvice(){
        System.out.println("afterAdvice");
    }
}
```

```xml
<!--配置目标类,内部的方法是连接点-->
<bean id="userService" class="com.itheima.service.impl.UserServiceImpl"/>
<!--配置通知类,内部的方法是增强方法-->
<bean id=“myAdvice" class="com.itheima.advice.MyAdvice"/>
<aop:config>
  <!--配置切点表达式,对哪些方法进行增强-->
  <aop:pointcut id="myPointcut" expression="execution(void com.itheima.service.impl.UserServiceImpl.show1())"/>
  <!--切面=切点+通知-->
  <aop:aspect ref="myAdvice">
    <!--指定前置通知方法是beforeAdvice-->
    <aop:before method="beforeAdvice" pointcut-ref="myPointcut"/>
    <!--指定后置通知方法是afterAdvice-->
    <aop:after-returning method="afterAdvice" pointcut-ref="myPointcut"/>
  </aop:aspect>
</aop:config>
```

##### 通知方法的参数

通知方法（也就是上面的beforeAdvice、afterAdvice...等方法）在被调用时，Spring可以为其传递一些必要的参数  
![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682510852651-7afa750c-2f1d-4636-9195-3a79c6a3f839.png#averageHue=%23acbfd9&clientId=udd5b1be1-beac-4&from=paste&height=210&id=ud5c31847&originHeight=210&originWidth=1291&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=92496&status=done&style=shadow&taskId=ub3531813-4823-4ff7-987d-77ff712d0ff&title=&width=1291)

###### JoinPoint 对象

```java
public void 通知方法名称(JoinPoint joinPoint){
    //获得目标方法的参数
    System.out.println(joinPoint.getArgs());
    //获得目标对象
    System.out.println(joinPoint.getTarget());
    //获得精确的切点表达式信息
    System.out.println(joinPoint.getStaticPart());
}
```

###### ProceedingJoinPoint对象

```java
public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
    System.out.println(joinPoint.getArgs());//获得目标方法的参数
    System.out.println(joinPoint.getTarget());//获得目标对象
    System.out.println(joinPoint.getStaticPart());//获得精确的切点表达式信息
    Object result = joinPoint.proceed();//执行目标方法
    return result;//返回目标方法返回值
}
```

###### Throwable对象

```java
public void afterThrowing(JoinPoint joinPoint,Throwable th){
    //获得异常信息
    System.out.println("异常对象是："+th+"异常信息是："+th.getMessage());
}
```

必须配置**异常通知**

```java
<aop:after-throwing method="afterThrowing" pointcut-ref="myPointcut" throwing="th"/>
```

#### 第二种方式：advice

先看一下目录结构 ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682512589132-4d4e11af-eb50-48d7-b8c8-a3b7fec97920.png#averageHue=%23514e42&clientId=udd5b1be1-beac-4&from=paste&height=538&id=u00a73774&originHeight=538&originWidth=618&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=75475&status=done&style=shadow&taskId=ua26b4e9d-3b8e-40bb-9fae-954a8a431b8&title=&width=618)

##### MethodBeforeAdvice和AfterReturningAdvice

```java
public class MyAdvice2 implements MethodBeforeAdvice, AfterReturningAdvice {
    //注意这里实现了两个接口，一个接口是前置通知，一个接口是后置通知，然后这两个接口里面有两个需要重写的方法
    //这里和上一种的区别是上一种是只写了方法，然后指定哪个方法是前置通知或者后置通知，这个直接给规定好了前置通知和后置通知写到哪个地方
    @Override
    public void before(Method method, Object[] objects, Object o) throws Throwable {
        System.out.println("前置通知..........");
    }
    @Override
    public void afterReturning(Object o, Method method, Object[] objects, Object o1) throws Throwable {
        System.out.println("后置通知...........");
    }
}
```

```xml
<!--配置目标类-->
<bean id="userService" class="com.itheima.service.impl.UserServiceImpl"></bean>
<!--配置的通知类-->
<bean id="myAdvice2" class="com.itheima.advice.MyAdvice2"></bean>

<!--aop配置-->
<aop:config>
  <aop:pointcut id="myPointcut2" expression="execution(* com.itheima.service.impl.*.*(..))"/>
<!--   这里就不需要指定哪个是前置方法哪个是后置方法了 -->
  <aop:advisor advice-ref="myAdvice2" pointcut-ref="myPointcut2"/>
</aop:config>
```

##### MethodInterceptor

这个就相当于**around**

```java
public class MyAdvice3 implements MethodInterceptor {
    @Override
    public Object invoke(MethodInvocation methodInvocation) throws Throwable {
        System.out.println("环绕前******");
        //执行目标方法
        Object res = methodInvocation.getMethod().invoke(methodInvocation.getThis(), methodInvocation.getArguments());
        System.out.println("环绕后******");
        return res;
    }
}
```

```xml
<!--配置目标类-->
<bean id="userService" class="com.itheima.service.impl.UserServiceImpl"></bean>
<!--配置的通知类-->
<bean id="myAdvice3" class="com.itheima.advice.MyAdvice3"></bean>
<!--aop配置-->
<aop:config>
  <aop:pointcut id="myPointcut2" expression="execution(* com.itheima.service.impl.*.*(..))"/>
  <aop:advisor advice-ref="myAdvice3" pointcut-ref="myPointcut2"/>
</aop:config>
```

#### 二者区别

##### 1）配置语法不同：

```xml
<!-- 使用advisor配置 -->
<aop:config>
  <!-- advice-ref:通知Bean的id -->
  <aop:advisor advice-ref="advices" pointcut="execution(void com.itheima.aop.TargetImpl.show())"/>
</aop:config>


<!-- 使用aspect配置 -->
<aop:config>
  <!-- ref:通知Bean的id -->
  <aop:aspect ref="advices">
    <aop:before method="before" pointcut="execution(void com.itheima.aop.TargetImpl.show())"/>
  </aop:aspect>
</aop:config>
```

##### 2）通知类的定义要求不同：

advisor 需要的通知类需要实现Advice的子功能接口：

```java
public class Advices implements MethodBeforeAdvice {
    @Override
    public void before(Method method, Object[] objects, Object o) throws Throwable {
        System.out.println("This is before Advice ...");
    }
    @Override
    public void afterReturning(Object o, Method method, Object[] objects, Object o1) throws Throwable {
        System.out.println("This is afterReturn Advice ...");
    }
}
```

aspect 不需要通知类实现任何接口，在配置的时候指定哪些方法属于哪种通知类型即可，更加灵活方便：

```java
// 不需要实现接口
public class Advices {
    public void before() {
        System.out.println("This is before Advice ...");
    }
    public void afterReturning() {
        System.out.println("This is afterReturn Advice ...");
    }
}
```

##### 3）可配置的切面数量不同：

⚫ 一个advisor只能配置一个固定通知和一个切点表达式；
⚫ 一个aspect可以配置多个通知和多个切点表达式任意组合，粒度更细。
也就是说advisor只能添加一个增强方法，aspect可以配置多个增强方法

##### 4）使用场景不同：

⚫ 如果通知类型多、允许随意搭配情况下可以使用aspect进行配置；
⚫ 如果通知类型单一、且通知类中通知方法一次性都会使用到的情况下可以使用advisor进行配置；
⚫ 在通知类型已经固定，不用人为指定通知类型时，可以使用advisor进行配置，例如后面要学习的Spring事务控制的配置；
由于实际开发中，**自定义aop功能的配置大多使用aspect的配置方式**

#### xml方式AOP原理剖析

通过xml方式配置AOP时，我们引入了AOP的命名空间，根据讲解的，要去找spring-aop包下的META-INF，在去 找spring.handlers文件  
![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682571566966-27c631e5-148e-412e-a8c2-bea480b190a4.png#averageHue=%23fcfbe0&clientId=u23d3dfb3-1e24-4&from=paste&height=244&id=ue8ddba73&originHeight=305&originWidth=1100&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=72121&status=done&style=shadow&taskId=u1da60d35-5064-415b-8a74-2cbacb2cbf9&title=&width=880) 文件内容：

```xml
http\://www.springframework.org/schema/aop=org.springframework.aop.config.AopNamespaceHandler
```

最终加载的是 **AopNamespaceHandler**，该Handler的**init**方法中注册了config标签对应的**解析器**

```java
this.registerBeanDefinitionParser("config", new ConfigBeanDefinitionParser());
```

以**ConfigBeanDefinitionParser**作为入口进行源码剖析，最终会注册一个**AspectJAwareAdvisorAutoProxyCreator** 进入到Spring容器中
看一下集成体系图  
![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682571866186-3b055410-aac2-476e-a479-caeee1d29ff2.png#averageHue=%23fcfcf6&clientId=u23d3dfb3-1e24-4&from=paste&height=314&id=ue02c7582&originHeight=393&originWidth=1017&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=90694&status=done&style=shadow&taskId=ub596207f-2c33-4f08-affe-68707f1edbf&title=&width=813.6) AspectJAwareAdvisorAutoProxyCreator 的上上级父类AbstractAutoProxyCreator中的 **postProcessAfterInitialization（after后置bean处理器）**方法

```java
//参数bean：为目标对象
public Object postProcessAfterInitialization(@Nullable Object bean, String beanName) {
    if (bean != null) {
        Object cacheKey = this.getCacheKey(bean.getClass(), beanName);
        if (this.earlyProxyReferences.remove(cacheKey) != bean) {
            //如果需要被增强，则wrapIfNecessary方法最终返回的就是一个Proxy对象
            return this.wrapIfNecessary(bean, beanName, cacheKey);
        }
    }
    return bean;
}
```

通过断点方式观察，当bean是匹配切点表达式时，this.**wrapIfNecessary**(bean, beanName, cacheKey)返回的是 一个**JDKDynamicAopProxy ** ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682572166614-a991cefc-470b-44e8-a4c3-9ffb3920cacb.png#averageHue=%23ebdcd4&clientId=u23d3dfb3-1e24-4&from=paste&height=377&id=uc6730b09&originHeight=471&originWidth=1198&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=226279&status=done&style=shadow&taskId=u762bbd13-a7de-4294-ae25-5ee0cc3886f&title=&width=958.4) 对wrapIfNecessary在剖析一下，看看是不是我们熟知的通过JDK的 Proxy.newProxyInstance(ClassLoader loader, Class[]interfaces,InvocationHandler h) 的方式创建的代理对象呢？经过如下一系列源码跟踪

```java
==> this.wrapIfNecessary(bean, beanName, cacheKey)
    ==> Object proxy = this.createProxy(参数省略)
        ==> proxyFactory.getProxy(classLoader)
            ==> this.createAopProxy().getProxy(classLoader)
                 //getProxy()是一个接口方法，实现类有两个，如下截图
                ==> Proxy.newProxyInstance(classLoader, this.proxiedInterfaces, this)
```

![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682572355788-e63a7cd0-1423-427e-8517-6078c2276d43.png#averageHue=%23f5f1d9&clientId=u23d3dfb3-1e24-4&from=paste&height=166&id=u3b8f991d&originHeight=208&originWidth=671&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=57738&status=done&style=shadow&taskId=u98b5eb86-d568-4653-a6fd-70de2028f7f&title=&width=536.8) 动态代理的实现的选择，在调用getProxy() 方法时，我们可选用的 AopProxy接口有两个实现类，如上图，这两种 都是动态生成代理对象的方式，一种就是基于JDK的，一种是基于Cglib的  
![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682572509789-cbc0334f-4efa-4ad1-993f-fa8cdaca74c2.png#averageHue=%23b3c5dc&clientId=u23d3dfb3-1e24-4&from=paste&height=165&id=u059b268a&originHeight=206&originWidth=1321&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=149624&status=done&style=shadow&taskId=u2b036948-483e-4d19-a5a0-b4c1a4950b1&title=&width=1056.8) 用图来表示这种关系： ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682572560353-6c2741d0-0743-43a9-b8b5-bc7efebaa071.png#averageHue=%23faf8f8&clientId=u23d3dfb3-1e24-4&from=paste&height=444&id=u2195e0b7&originHeight=555&originWidth=1117&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=108335&status=done&style=shadow&taskId=uc2cad1d4-b3b7-4cb4-9588-7ca144413c6&title=&width=893.6)

### Cglib基于超类的动态代理

```java
Target target = new Target();//目标对象
Advices advices = new Advices();//通知对象
Enhancer enhancer = new Enhancer();//增强器对象
enhancer.setSuperclass(Target.class);//增强器设置父类
//增强器设置回调
enhancer.setCallback((MethodInterceptor )(o, method, objects, methodProxy) -> {
    advices.before();
    Object result = method.invoke(target, objects);
    advices.afterReturning();
    return result;
});
//创建代理对象
Target targetProxy = (Target) enhancer.create();
//测试
String result = targetProxy.show("haohao");
```

### 基于注解配置的AOP

Spring的AOP也提供了注解方式配置，使用相应的注解替代之前的xml配置，xml配置AOP时，我们主要配置了三 部分：**目标类被Spring容器管理**、**通知类被Spring管理**、**通知与切点的织入（切面）**，如下：

```xml
<!--配置目标-->
<bean id="target" class="com.itheima.aop.TargetImpl"></bean>
<!--配置通知-->
<bean id="advices" class="com.itheima.aop.Advices"></bean>
<!--配置aop-->
<aop:config proxy-target-class="true">
  <aop:aspect ref="advices">
    <aop:around method="around" pointcut="execution(* com.itheima.aop.*.*(..))"/>
  </aop:aspect>
</aop:config>
```

用注解表示

1. 目标类被Spring容器管理、通知类被Spring管理
   
   ```java
   @Component("target")
   public class TargetImpl implements Target{
      public void show() {
          System.out.println("show Target running...");
      }
   }
   @Component
   public class AnnoAdvice {
      public void around(ProceedingJoinPoint joinPoint) throws Throwable {
          System.out.println("环绕前通知...");
          joinPoint.proceed();
          System.out.println("环绕后通知...");
      }
   }
   ```

```
2. 配置切面（织入）

![image.png](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682579551732-8da9a7ff-6171-4552-a469-265202f2aecd.png#averageHue=%23e9eeda&clientId=u23d3dfb3-1e24-4&from=paste&height=271&id=u044a36e5&originHeight=339&originWidth=1327&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=115997&status=done&style=shadow&taskId=uffd5e657-a675-4da5-9003-4a5b32e6c82&title=&width=1061.6)

3. 


配置文件方式：
注解@Aspect、@Around需要被Spring解析，所以在Spring核心配置文件中需要配置aspectj的自动代理  
```xml
<aop:aspectj-autoproxy/>
```

注解方式：

```java
@Configuration
@ComponentScan("com.itheima.aop")
@EnableAspectJAutoProxy //第三步
public class ApplicationContextConfig {
}
```

#### 注解方式AOP原理剖析

之前在使用xml配置AOP时，是借助的Spring的外部命名空间的加载方式完成的，使用注解配置后，就抛弃了 标签，而该标签最终加载了名为AspectJAwareAdvisorAutoProxyCreator的BeanPostProcessor ， 最终，在该BeanPostProcessor中完成了代理对象的生成。
同样，在注解配置AOP时，从aspectj-autoproxy标签的解析器入手

```java
this.registerBeanDefinitionParser("aspectj-autoproxy", new AspectJAutoProxyBeanDefinitionParser());
```

而AspectJAutoProxyBeanDefinitionParser代码内部，最终也是执行了和xml方式AOP一样的代码

```java
registerOrEscalateApcAsRequired(AnnotationAwareAspectJAutoProxyCreator.class, registry, source)
```

如果使用的是核心配置类的话

```java
@Configuration
@ComponentScan("com.itheima.aop")
@EnableAspectJAutoProxy
public class ApplicationContextConfig {
}
```

查看@EnableAspectJAutoProxy源码，使用的也是@Import导入相关解析类

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Import({AspectJAutoProxyRegistrar.class})
public @interface EnableAspectJAutoProxy {
    boolean proxyTargetClass() default false;
    boolean exposeProxy() default false;
}
```

使用@Import导入的AspectJAutoProxyRegistrar源码，一路追踪下去，最终还是注册了 AnnotationAwareAspectJAutoProxyCreator 这个类 ,其实还是那个beanPostProcessor,完成创建代理对象的环节

```java
public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
    AopConfigUtils.registerAspectJAnnotationAutoProxyCreatorIfNecessary(registry);
}
public static BeanDefinition registerAspectJAnnotationAutoProxyCreatorIfNecessary(BeanDefinitionRegistry registry) {
    return registerAspectJAnnotationAutoProxyCreatorIfNecessary(registry, (Object)null);
}
public static BeanDefinition registerAspectJAnnotationAutoProxyCreatorIfNecessary(BeanDefinitionRegistry registry, @Nullable Object source) {
    return registerOrEscalateApcAsRequired(AnnotationAwareAspectJAutoProxyCreator.class, registry, source);
}
```

![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682580585527-e7fa688d-60bc-4d24-a59a-f6de2003cf86.png#averageHue=%23f7f0f0&clientId=u23d3dfb3-1e24-4&from=paste&height=451&id=uca24fe9f&originHeight=564&originWidth=1250&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=102277&status=done&style=shadow&taskId=ub1b9e57d-a4ca-4e6d-a06d-bb58ba71e01&title=&width=1000)

## Spring的基于AOP的声明式事务控制

事务是开发中必不可少的东西，
使用JDBC开发时，我们使用connnection对事务进行控制;
使用MyBatis时，我们使用SqlSession对事务进行控制;
缺点显而易见，当我们切换数据库访问技术时，事务控制的方式总会变化， Spring 就将这些技术基础上，**提供了统一的控制事务的接口**。
Spring的事务分为：**编程式事务控制** 和 **声明式事务控制**  
![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682593187549-22521cf9-2c64-4b63-aa26-6bfda7e1fab5.png#averageHue=%23b5c5dc&clientId=ua3c88113-13e5-4&from=paste&height=134&id=uf553392d&originHeight=167&originWidth=988&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=79572&status=done&style=shadow&taskId=u8490a377-f91b-44cb-a909-8cfed097c29&title=&width=790.4)

Spring事务编程相关的类主要有如下三个  
![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682593233198-792021f3-946e-45a3-93bc-6b4772f170ef.png#averageHue=%23acbed8&clientId=ua3c88113-13e5-4&from=paste&height=121&id=u77902615&originHeight=151&originWidth=972&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=78951&status=done&style=shadow&taskId=udab5cad2-0582-41d8-99a7-4e9ad4a37e0&title=&width=777.6) **编程式事务控制**对应的这些类我们需要了解一下，因为我们在通过配置的方式进行**声明式事务控制**时也会看到这些类的影子

### 基于xml声明式事务控制

可以使用**AOP**对Service的方法进行事务的增强。
⚫ 目标类：AccountServiceImpl
⚫ 切点：service业务类中的所有业务方法
⚫ 通知类：**Spring提供的**，通知方法已经定义好，**只需要配置即可**

1. 配置目标类AccountServiceImpl
   
   ```xml
   <bean id="accountService" class="com.itheima.service.impl.AccoutServiceImpl">
   <property name="accountMapper" ref="accountMapper"></property>
   </bean>
   ```

2. 使用advisor标签配置切面
   
   ```xml
   <aop:config>
   <aop:advisor advice-ref="Spring提供的通知类" pointcut="execution(* com.itheima.service.impl.*.*(..))"/>
   </aop:config>
   ```
   
   注意，这里还没配置spring提供的通知类！！！下面会配置，这里我们倒着配置

```xml
<!--Spring提供的事务通知-->
<tx:advice id="myAdvice" transaction-manager="transactionManager">
  <tx:attributes>
<!--     配置transferMoney()方法 触发事务 -->
    <tx:method name="transferMoney"/>
  </tx:attributes>
</tx:advice>
<!--平台事务管理器-->
<bean id="transactionManager" 
  class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
  <property name="dataSource" ref="dataSource"/>
</bean>
```

```xml
<aop:config>
<!--   这里才配置了Spring提供的事务通知 -->
  <aop:advisor advice-ref="myAdvice" pointcut="execution(* com.itheima.service.impl.*.*(..))"/>
</aop:config>
```

**这里对上面的配置详解：** 首先，平台事务管理器PlatformTransactionManager是Spring提供的封装事务具体操作的规范接口，封装了事务的提交和回滚方法

```java
public interface PlatformTransactionManager extends TransactionManager {
    TransactionStatus getTransaction(@Nullable TransactionDefinition var1) throws TransactionException;
    void commit(TransactionStatus var1) throws TransactionException;
    void rollback(TransactionStatus var1) throws TransactionException;
}
```

不同的持久层框架事务操作的方式有可能不同，所以不同的持久层框架有可能会有不同的平台事务管理器实现， 例如，MyBatis作为持久层框架时，使用的平台事务管理器实现是DataSourceTransactionManager（也是我们上面示例的管理器）。 Hibernate作为持久层框架时，使用的平台事务管理器HibernateTransactionManager。  
其次，事务定义信息配置，每个事务有很多特性，例如：隔离级别、只读状态、超时时间等，这些信息在开发时可以通过connection进行指定，而此处要通过配置文件进行配置

```xml
<tx:attributes>
<!--   
      其中，name属性名称指定哪个方法要进行哪些事务的属性配置，此处需要区分的是切点表达式指定的方法与此处指定的方法的区别：
                      切点表达式，是过滤哪些方法可以进行事务增强；
                      事务属性信息的name，是指定哪个方法要进行哪些事务属性的配置
 -->
  <tx:method  name="方法名称"
              isolation="隔离级别"
              propagation="传播行为"
              read-only="只读状态"
              timeout="超时时间"/>
</tx:attributes
```

#### name

![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682596980950-f6098815-7180-499d-85d3-55fbb979c946.png#averageHue=%23f5f4f4&clientId=ua3c88113-13e5-4&from=paste&height=325&id=u9e0ba2dc&originHeight=406&originWidth=900&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=124208&status=done&style=shadow&taskId=ucdf979b2-2732-48d9-b29f-bf0bce4ac56&title=&width=720) 
方法名在配置时，也可以使用 * 进行模糊匹配，例如：

```xml
<tx:advice id="myAdvice" transaction-manager="transactionManager">
  <tx:attributes>
    <!--精确匹配transferMoney方法-->
    <tx:method name="transferMoney"/>
    <!--模糊匹配以Service结尾的方法-->
    <tx:method name="*Service"/>
    <!--模糊匹配以insert开头的方法-->
    <tx:method name="insert*"/>
    <!--模糊匹配以update开头的方法-->
    <tx:method name="update*"/>
    <!--模糊匹配任意方法，一般放到最后作为保底匹配-->
    <tx:method name="*"/>
  </tx:attributes>
</tx:advice>
```

#### isolation

isolation属性：指定事务的隔离级别，事务并发存在三大问题：脏读、不可重复读、幻读/虚读。**可以通过设置事务的隔离级别来保证并发问题的出现**，常用的是READ_COMMITTED 和 REPEATABLE_READ  
![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682597087811-e7c153c3-1e64-47b3-aba9-e7cfc5c4527d.png#averageHue=%23b6c5db&clientId=ua3c88113-13e5-4&from=paste&height=230&id=u2bf83b5f&originHeight=287&originWidth=1238&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=154396&status=done&style=shadow&taskId=uc62e1d69-0239-4010-a374-d4785735d80&title=&width=990.4)

#### read-only

read-only属性：设置当前的只读状态，如果是查询则设置为true，可以提高查询性能，如果是更新（增删改）操作则设置为false

```xml
<!-- 一般查询相关的业务操作都会设置为只读模式 -->
<tx:method name="select*" read-only="true"/>
<tx:method name="find*" read-only="true"/>
```

#### timeout

timeout属性：设置事务执行的超时时间，单位是秒，如果超过该时间限制但事务还没有完成，则自动回滚事务 ，不在继续执行。默认值是-1，即没有超时时间限制

```xml
<!-- 设置查询操作的超时时间是3秒 -->
<tx:method name="select*" read-only="true" timeout="3"/>
```

#### propagation

propagation属性：设置事务的传播行为，主要解决是A方法调用B方法时，事务的传播方式问题的，例如：使用 单方的事务，还是A和B都使用自己的事务等。事务的传播行为有如下七种属性值可配置  
![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1682597231826-4f601fa5-f50e-4600-ba35-c601badd304f.png#averageHue=%23becbde&clientId=ua3c88113-13e5-4&from=paste&height=281&id=u1cd6b741&originHeight=351&originWidth=1218&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=156110&status=done&style=shadow&taskId=u97b4f2a1-abca-48c9-9907-f8158e6b088&title=&width=974.4)

### xml方式声明式事务控制的原理浅析

tx:advice标签使用的命名空间处理器是**TxNamespaceHandler**，内部注册的是解析器是**TxAdviceBeanDefinitionParser**

```java
this.registerBeanDefinitionParser("advice", new TxAdviceBeanDefinitionParser());
```

**TxAdviceBeanDefinitionParser**中指定了要注册的**BeanDefinition **

```java
protected Class<?> getBeanClass(Element element) {
    return TransactionInterceptor.class;
}
```

TxAdviceBeanDefinitionParser二级父类**AbstractBeanDefinitionParser**的parse方法将**TransactionInterceptor** 以配置的名称注册到了Spring容器中

```java
parserContext.registerComponent(componentDefinition);
```

TransactionInterceptor(**这个玩意就是我们当时学advice里面的环绕通知！！！**)中的invoke方法会被执行，跟踪invoke方法，最终会看到事务的开启和提交
⚫ 在AbstractPlatformTransactionManager的132行中开启的事务；
⚫ 在TransactionAspectSupport的242行提交了事务

### 基于注解声明式事务控制

```java
@Service("accountService")
public class AccoutServiceImpl implements AccountService {
    @Autowired
    private AccountMapper accountMapper;
    //替代的是这一行，第一个name就不用配置了，因为就写在这个方法头上
    //<tx:method name="*" isolation="REPEATABLE_READ" propagation="REQUIRED“/>
    @Transactional(isolation = Isolation.REPEATABLE_READ,propagation = Propagation.REQUIRED,readOnly = false,timeout = 5)
    public void transferMoney(String decrAccountName, String incrAccountName, int money) {
        accountMapper.decrMoney(decrAccountName,money); //转出钱
        int i = 1/0; //模拟某些逻辑产生的异常
        accountMapper.incrMoney(incrAccountName,money); //转入钱
    }
}
```

```java
@Configuration
@ComponentScan("com.itheima.service")
@PropertySource("classpath:jdbc.properties")
@MapperScan("com.itheima.mapper")
@EnableTransactionManagement
public class ApplicationContextConfig {
    @Bean
    public PlatformTransactionManager tansactionManager(DataSource dataSource){
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(dataSource);
        return transactionManager;
    }
    // ... 省略其他配置 ...
}
```

# SpringMVC

SpringMVC是一个基于Spring开发的MVC轻量级框架，Spring3.0后发布的组件，SpringMVC和Spring可以无缝整合，使用DispatcherServlet作为前端控制器，且内部提供了处理器映射器、处理器适配器、视图解析器等组件，可以简化JavaBean封装，Json转化、文件上传等操作。  
附：开发需要注意的点：
最好在开发的时候封装参数用包装类，也就是不要用int用integer，要不然可能会出错
用集合接收参数的时候必须加上@RequestParam注解，要不然也会报错的

## 三个核心组件

![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1683103938344-20bb89b6-5eff-4c62-8e27-d555dc19f433.png#averageHue=%23b3c4dc&clientId=u9f0bcf28-5dac-4&from=paste&height=272&id=uda4accf2&originHeight=272&originWidth=1567&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=133122&status=done&style=shadow&taskId=ub413dca2-8af4-4196-8c68-17724efcb3f&title=&width=1567) 用图片来表示 ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1683104247465-7a368293-f886-4cb4-b08e-5cc97b57fdd3.png#averageHue=%23f3eded&clientId=u9f0bcf28-5dac-4&from=paste&height=642&id=u8992aa32&originHeight=642&originWidth=1148&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=170077&status=done&style=shadow&taskId=u3b2369aa-89fd-4e67-8c66-c19fce2b99e&title=&width=1148)

## SpringMVC关键组件浅析

SpringMVC的默认组件，SpringMVC 在前端控制器 DispatcherServlet加载时，就会进行初始化操作，在进行初始化时，就会加载SpringMVC默认指定的一些组件，这些默认组件配置在 DispatcherServlet.properties 文件中，该文 件存在与spring-webmvc-5.3.7.jar包下的org\springframework\web\servlet\DispatcherServlet.properties

```properties
org.springframework.web.servlet.HandlerMapping=org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping,\
org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping,\
org.springframework.web.servlet.function.support.RouterFunctionMapping
org.springframework.web.servlet.HandlerAdapter=org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter,\
org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter,\
org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter,\
org.springframework.web.servlet.function.support.HandlerFunctionAdapter
org.springframework.web.servlet.ViewResolver=org.springframework.web.servlet.view.InternalResourceViewResolver
```

这些默认的组件是在DispatcherServlet中进行初始化加载的，在DispatcherServlet中存在集合存储着这些组件， SpringMVC的默认组件会在 DispatcherServlet 中进行维护，但是并没有存储在与SpringMVC的容器中

```java
public class DispatcherServlet extends FrameworkServlet {
    //存储处理器映射器
    private List<HandlerMapping> handlerMappings;
    //存储处理器适配器
    private List<HandlerAdapter> handlerAdapters;
    //存储视图解析器
    private List<ViewResolver> viewResolvers;
    // ... 省略其他代码 ...
}
```

配置组件代替默认组件，如果不想使用默认组件，可以将替代方案使用Spring Bean的方式进行配置，例如，在 spring-mvc.xml中配置RequestMappingHandlerMapping （只是有这个功能！！这个后面会用到！！！理解这个就行）

```java
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
```

当我们在Spring容器中配置了HandlerMapping，则就不会在加载默认的HandlerMapping策略了，原理比较简单， DispatcherServlet 在进行HandlerMapping初始化时，先从SpringMVC容器中找是否存在HandlerMapping，如果存在直接取出容器中的HandlerMapping，在存储到 DispatcherServlet 中的handlerMappings集合中去。

## 请求静态资源

静态资源请求失效的原因，当DispatcherServlet的映射路径配置为 / 的时候，那么就覆盖的Tomcat容器默认的缺省 Servlet，在Tomcat的config目录下有一个web.xml 是对所有的web项目的全局配置，其中有如下配置：

```xml
<servlet>
  <servlet-name>default</servlet-name>
  <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
  <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
  <servlet-name>default</servlet-name>
  <url-pattern>/</url-pattern>
</servlet-mapping>
```

url-pattern配置为 / 的Servlet我们称其为缺省的Servlet，作用是当其他Servlet都匹配不成功时，就找缺省的Servlet ，静态资源由于没有匹配成功的Servlet，所以会找缺省的DefaultServlet，该DefaultServlet具备二次去匹配静态资 源的功能。但是我们配置DispatcherServlet后就将其覆盖掉了，而DispatcherServlet会将请求的静态资源的名称当 成Controller的映射路径去匹配，即静态资源访问不成功了！

### 静态资源请求的三种解决方案：

第一种方案，可以再次激活Tomcat的DefaultServlet，Servlet的url-pattern的匹配优先级是：精确匹配>目录匹配>扩展名匹配>缺省匹配，所以可以指定某个目录下或某个扩展名的资源使用DefaultServlet进行解析：

```xml
<servlet-mapping>
<servlet-name>default</servlet-name>
<url-pattern>/img/*</url-pattern>
</servlet-mapping>
<servlet-mapping>
<servlet-name>default</servlet-name>
<url-pattern>*.html</url-pattern>
</servlet-mapping>
```

第二种方式，在spring-mvc.xml中去配置静态资源映射，匹配映射路径的请求到指定的位置去匹配资源

```xml
<!-- mapping是映射资源路径，location是对应资源所在的位置 -->
<mvc:resources mapping="/img/*" location="/img/"/>
<mvc:resources mapping="/css/*" location="/css/"/>
<mvc:resources mapping="/css/*" location="/js/"/>
<mvc:resources mapping="/html/*" location="/html/"/>
```

第三种方式，在spring-mvc.xml中去配置< mvc:default-servlet-handler >，该方式是注册了一个 DefaultServletHttpRequestHandler 处理器，静态资源的访问都由该处理器去处理，这也是开发中使用最多的

```xml
<mvc:default-servlet-handler/>
```

### 出现的问题

静态资源配置的第二第三种方式我们可以正常访问静态资源了，但是Controller又无法访问了，报错404，即找不到对应的资源  
第二种方式是通过SpringMVC去解析mvc命名空间下的resources标签完成的静态资源解析，第三种方式式通过 SpringMVC去解析mvc命名空间下的default-servlet-handler标签完成的静态资源解析，根据前面所学习的自定义命名空间的解析的知识，可以发现不管是以上哪种方式，最终都会注册SimpleUrlHandlerMapping

```java
public BeanDefinition parse(Element element, ParserContext context) {
    //创建SimpleUrlHandlerMapping类型的BeanDefinition
    RootBeanDefinition handlerMappingDef =
        //可以看到这里创建了SimpleUrlHandlerMapping，在这里和前面相呼应
        new RootBeanDefinition(SimpleUrlHandlerMapping.class);
    //注册SimpleUrlHandlerMapping的BeanDefinition
    context.getRegistry().registerBeanDefinition(beanName, handlerMappingDef);
}
```

又结合组件浅析知识点，**一旦SpringMVC容器中存在 HandlerMapping 类型的组件时，前端控制器 DispatcherServlet在进行初始化时，就会从容器中获得HandlerMapping(**SimpleUrlHandlerMapping就是**HandlerMapping)** ，不在加载 dispatcherServlet.properties 中默认处理器映射器策略，那也就意味着RequestMappingHandlerMapping（这个是处理Controller请求的，SimpleUrlHandlerMapping不可以处理Controller请求）不会被加载到了。  
所以我们手动将RequestMappingHandlerMapping也注册到SpringMVC容器中就可以了

### 注解驱动mvc:annotation-driven标签

```xml
<!-- 显示配置RequestMappingHandlerMapping -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
<!-- 显示配置RequestMappingHandlerAdapter -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
  <property name="messageConverters">
    <list>
      <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
    </list>
  </property>
</bean>
<!--配置DefaultServletHttpRequestHandler-->
<mvc:default-servlet-handler/>
```

可以简化成如下配置

```xml
<!--该标签内部会帮我们注册RequestMappingHandlerMapping、注册RequestMappingHandlerAdapter并注入Json消息转换器等-->
<!--mvc注解驱动-->
<mvc:annotation-driven/>
<!--配置DefaultServletHttpRequestHandler-->
<mvc:default-servlet-handler/>
```

## SpringMVC的拦截器

![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1683107460560-a4e020ee-d9d0-4210-9f54-ab12b12c87fb.png#averageHue=%23f4e6dc&clientId=u9f0bcf28-5dac-4&from=paste&height=591&id=u980232ab&originHeight=591&originWidth=1297&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=106910&status=done&style=shadow&taskId=u1adcf393-4336-472a-b6cf-42ca906754b&title=&width=1297) ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1683107521965-d7dac7b3-cb6a-41a3-bb4d-10717a9f1589.png#averageHue=%23c5dbdf&clientId=u9f0bcf28-5dac-4&from=paste&height=356&id=u434b6a69&originHeight=356&originWidth=1602&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=114478&status=done&style=shadow&taskId=u449196c5-bbc9-4cea-9e40-e5547e82963&title=&width=1602)

### 拦截器 Interceptor 简介

实现了HandlerInterceptor接口，且被Spring管理的Bean都是拦截器，接口定义如下：

```java
public interface HandlerInterceptor {
    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //为真说明可以往下个过滤器中传递，为假就在这个过滤器停止了
        return true;
    }
    default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {
    }
    default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {
    }
}
```

![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1683107680057-fa04cd80-d734-4354-84c1-f1d0a0077fd6.png#averageHue=%23afd0df&clientId=u9f0bcf28-5dac-4&from=paste&height=515&id=u43b57fbe&originHeight=515&originWidth=1613&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=255948&status=done&style=shadow&taskId=u0812ffd9-44dc-44e5-80ef-50ca5ad017f&title=&width=1613)

### 拦截器的执行顺序

** 拦截器执行顺序取决于 interceptor 的配置顺序 **
比如我们先配置1，再配置2，再配置3： ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1683107817746-26cbb283-9dad-46ea-9967-e9187c7b8285.png#averageHue=%23f6eeea&clientId=u9f0bcf28-5dac-4&from=paste&height=382&id=u373ce289&originHeight=382&originWidth=1634&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=121144&status=done&style=shadow&taskId=uaff70e6b-025c-4abf-a51c-ff528635904&title=&width=1634) ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1683107892637-944f4b9d-2a3b-4e59-96e2-96d757e23bce.png#averageHue=%23f9f3f0&clientId=u9f0bcf28-5dac-4&from=paste&height=477&id=u640c23f7&originHeight=477&originWidth=1650&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=125701&status=done&style=shadow&taskId=u4520a9b9-a60f-4477-bf8d-39211ce285b&title=&width=1650)

### 拦截器执行原理

请求到来时先会使用组件**HandlerMapping**去匹配Controller的方法（Handler）和符合拦截路径的Interceptor，** Handler和多个Interceptor**被封装成一个HandlerExecutionChain的对象  
HandlerExecutionChain 定义如下：

```java
public class HandlerExecutionChain {
    //映射的Controller的方法！！
    private final Object handler;
    //当前Handler匹配的拦截器集合！！
    //可能不是一个，所以是一个集合
    private final List<HandlerInterceptor> interceptorList;
    // ... 省略其他代码 ...
}
```

![](https://cdn.nlark.com/yuque/0/2023/png/27086425/1683104247465-7a368293-f886-4cb4-b08e-5cc97b57fdd3.png?x-oss-process=image%2Fresize%2Cw_1148%2Climit_0#averageHue=%23f3eded&from=url&id=MdzFW&originHeight=642&originWidth=1148&originalType=binary&ratio=1.25&rotation=0&showTitle=false&status=done&style=shadow&title=) 在DispatcherServlet的doDispatch方法中执行拦截器

```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response){
    //根据请求信息获得HandlerExecutionChain
    HandlerExecutionChain mappedHandler = this.getHandler(request);
    //获得处理器适配器
    HandlerAdapter ha = this.getHandlerAdapter(mappedHandler.getHandler());
    //执行Interceptor的前置方法，前置方法如果返回false，则该流程结束,
    //这里是正向遍历！！！
    if (!mappedHandler.applyPreHandle(request, response)) {
        return;
    }
    //执行handler，一般是HandlerMethod
    ModelAndView mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
    //执行后置方法    
    //这里是反向遍历！！！
    mappedHandler.applyPostHandle(processedRequest, response, mv);
    //执行最终方法
    //这里是反向遍历！！！
    this.triggerAfterCompletion(processedRequest, response, mappedHandler, e);
}
```

## 前端控制器原理剖析

### 前端控制器初始化

前端控制器DispatcherServlet是SpringMVC的入口，也是SpringMVC的大脑，主流程的工作都是在此完成的，梳理一下DispatcherServlet 代码。DispatcherServlet 本质是个Servlet，当配置了 load-on-startup 时，会在服务器启动时就执行创建和执行初始化init方法，每次请求都会执行service方法
DispatcherServlet 的初始化主要做了两件事：
⚫ 获得了一个 SpringMVC 的 ApplicationContext容器；
⚫ 注册了 SpringMVC 的九大组件 ![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1683114053224-c097d8f2-8a00-4644-b584-8b97e3e8ceef.png#averageHue=%23fdfbfb&clientId=u9f0bcf28-5dac-4&from=paste&height=677&id=uac901d4e&originHeight=677&originWidth=1316&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=119095&status=done&style=shadow&taskId=ued4e940a-a438-4d4e-8b2a-1e302b86f7f&title=&width=1316) SpringMVC 的ApplicationContext容器创建时机，Servlet 规范的 init(ServletConfig config) 方法经过子类重写 ，最终会调用 FrameworkServlet 抽象类的initWebApplicationContext() 方法，该方法中最终获得 一个根 Spring容器（Spring产生的），一个子Spring容器（SpringMVC产生的）  
HttpServletBean 的初始化方法

```java
public final void init() throws ServletException {
    this.initServletBean();
}
```

FrameworkServlet的initServletBean方法

```java
protected final void initServletBean() throws ServletException {
    this.webApplicationContext = this.initWebApplicationContext();//初始化ApplicationContext，这个代码再下面有！
    this.initFrameworkServlet();//模板设计模式，供子类覆盖实现，但是子类DispatcherServlet没做使用
}
```

在initWebApplicationContext方法中体现的父子容器的逻辑关系

```java
//初始化ApplicationContext是一个及其关键的代码
protected WebApplicationContext initWebApplicationContext() {
    //获得根容器，其实就是通过ContextLoaderListener创建的ApplicationContext
    //如果配置了ContextLoaderListener则获得根容器，没配置获得的是null
    WebApplicationContext rootContext = WebApplicationContextUtils.getWebApplicationContext(this.getServletContext());
    //定义SpringMVC产生的ApplicationContext子容器
    WebApplicationContext wac = null;
    if (wac == null) {
        //==>创建SpringMVC的子容器，创建同时将Spring的创建的rootContext传递了过去
        //这个方法往里面跟进！
        wac = this.createWebApplicationContext(rootContext);
    }
    //将SpringMVC产生的ApplicationContext子容器存储到ServletContext域中
    //key名是：org.springframework.web.servlet.FrameworkServlet.CONTEXT.DispatcherServlet
    if (this.publishContext) {
        String attrName = this.getServletContextAttributeName();
        this.getServletContext().setAttribute(attrName, wac);
    }
}
```

跟进创建子容器的源码

```java
protected WebApplicationContext createWebApplicationContext(@Nullable ApplicationContext parent) {
    //实例化子容器ApplicationContext
    ConfigurableWebApplicationContext wac = (ConfigurableWebApplicationContext)BeanUtils.instantiateClass(contextClass);
    //设置传递过来的ContextLoaderListener的rootContext为父容器
    wac.setParent(parent);
    //获得web.xml配置的classpath:spring-mvc.xml
    String configLocation = this.getContextConfigLocation();
    if (configLocation != null) {
        //为子容器设置配置加载路径
        wac.setConfigLocation(configLocation);
    }
    //初始化子容器(就是加载spring-mvc.xml配置的Bean)
    this.configureAndRefreshWebApplicationContext(wac);
    return wac;
}
```

子容器中的parent维护着父容器的引用  
![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1683116654011-11e5ba73-33bc-4e04-af6b-1c8eb1712144.png#averageHue=%23fbf8f6&clientId=u9f0bcf28-5dac-4&from=paste&height=620&id=uaa757494&originHeight=620&originWidth=1283&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=365549&status=done&style=shadow&taskId=u1d7d2517-1360-4ec4-ac39-6b141f1fac5&title=&width=1283) 父容器和子容器概念和关系：
⚫ 父容器：Spring 通过ContextLoaderListener为入口产生的applicationContext容器，内部主要维护的是applicationContext.xml（或相应配置类）配置的Bean信息；
⚫ 子容器：SpringMVC通过DispatcherServlet的init() 方法产生的applicationContext容器，内部主要维护的是spring-mvc.xml（或相应配置类）配置的Bean信息，且内部还通过parent属性维护这父容器的引用。
⚫ Bean的检索顺序：根据上面子父容器的概念，可以知道Controller存在与子容器中，而Controller中要注入Service时，会先从子容器本身去匹配，匹配不成功时在去父容器中去匹配，于是最终从父容器中匹配到的UserService，这样子父容器就可以进行联通了。但是父容器只能从自己容器中进行匹配，不能从子容器中进行匹配

注册 SpringMVC的九大组件，在初始化容器initWebApplicationContext方法中执行了onRefresh方法，进而执行了初始化策略initStrategies方法，注册了九个解析器组件

```java
//DispatcherServlet初始化SpringMVC九大组件
protected void initStrategies(ApplicationContext context) {
    this.initMultipartResolver(context);//1、初始化文件上传解析器
    this.initLocaleResolver(context);//2、初始化国际化解析器
    this.initThemeResolver(context);//3、初始化模板解析器
    this.initHandlerMappings(context);//4、初始化处理器映射器
    this.initHandlerAdapters(context);//5、初始化处理器适配器
    this.initHandlerExceptionResolvers(context);//6、初始化处理器异常解析器
    this.initRequestToViewNameTranslator(context);//7、初始化请求视图转换器
    this.initViewResolvers(context);//8、初始化视图解析器
    this.initFlashMapManager(context);//9、初始化lashMapManager策略组件
}
```

以 this.initHandlerMappings(context) 为例，进一步看一下初始化处理器映射器的细节：

> HandlerMapping就是controller上面的路径

```java
//定义List容器存储HandlerMapping
private List<HandlerMapping> handlerMappings;
//初始化HandlerMapping的方法
private void initHandlerMappings(ApplicationContext context) {
    this.handlerMappings = null;//初始化集合为null
    //detectAllHandlerMappings默认为true，代表是否从所有容器中(父子容器)检测HandlerMapping
    if (this.detectAllHandlerMappings) {
        //从Spring容器中去匹配HandlerMapping
        Map<String, HandlerMapping> matchingBeans = BeanFactoryUtils.beansOfTypeIncludingAncestors(
            context,
            HandlerMapping.class, 
            true, 
            false);
        //如果从容器中获取的HandlerMapping不为null就加入到事先定义好的handlerMappings容器中
        if (!matchingBeans.isEmpty()) {
            this.handlerMappings = new ArrayList(matchingBeans.values());
            AnnotationAwareOrderComparator.sort(this.handlerMappings);
        }
        //如果从容器中没有获得HandlerMapping，意味着handlerMappings集合是空的
        if (this.handlerMappings == null) {
            //加载默认的HandlerMapping，就是加载DispatcherServlet.properties文件中的键值对
            this.handlerMappings = this.getDefaultStrategies(context, HandlerMapping.class);
        } 
    } 
}
```

![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1683116872603-00504fd5-0abf-4d84-b197-19065ed9e225.png#averageHue=%23faf8f7&clientId=u9f0bcf28-5dac-4&from=paste&height=719&id=u04e6d73e&originHeight=719&originWidth=1659&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=368548&status=done&style=shadow&taskId=u849ae62b-09fa-4a7e-a731-debfc651f79&title=&width=1659)

### 前端控制器执行主流程

上面讲解了一下，当服务器启动时，DispatcherServlet 会执行初始化操作，接下来，每次访问都会执行service 方法，我们先宏观的看一下执行流程，在去研究源码和组件执行细节  
![imagepng](https://cdn.nlark.com/yuque/0/2023/png/27086425/1683116990247-a75fcba5-4c1c-4629-85c7-dd37a9cb81fb.png#averageHue=%23f2efef&clientId=u9f0bcf28-5dac-4&from=paste&height=700&id=uaf12a2f4&originHeight=700&originWidth=1245&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=171771&status=done&style=shadow&taskId=u3513daaa-bdd0-4e85-9e01-d3731b34781&title=&width=1245) FrameworkServlet 复写了service(HttpServletRequest request, HttpServletResponse response) 、 doGet(HttpServletRequest request, HttpServletResponse response)、doPost(HttpServletRequest request, HttpServletResponse response)等方法，这些方法都会调用processRequest方法

```java
protected final void processRequest(HttpServletRequest request, HttpServletResponse response){
    this.doService(request, response);
}
```

进一步调用了doService方法，该方法内部又调用了doDispatch方法，而SpringMVC 主流程最核心的方法就是 doDispatch 方法

```java
protected void doService(HttpServletRequest request, HttpServletResponse response) {
    this.doDispatch(request, response);
}
```

doDispatch方法源码

```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) {
    HttpServletRequest processedRequest = request;
    HandlerExecutionChain mappedHandler = null; //定义处理器执行链对象
    ModelAndView mv = null; //定义模型视图对象
    //匹配处理器映射器HandlerMapping，返回处理器执行链对象
    //注意这里，是一个执行链对象
    mappedHandler = this.getHandler(processedRequest);
    //匹配处理器适配器HandlerAdapter，返回处理器适配器对象
    HandlerAdapter ha = this.getHandlerAdapter(mappedHandler.getHandler());
    //执行Interceptor的前置方法preHandle
    mappedHandler.applyPreHandle(processedRequest, response);
    //HandlerAdapter处理器适配器执行控制器Handler，返回模型视图对象
    mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
    //执行Interceptor的后置方法postHandle
    mappedHandler.applyPostHandle(processedRequest, response, mv);
    //获取视图渲染视图
    this.processDispatchResult(processedRequest, response, mappedHandler, mv, (Exception)dispatchException);
}
```

# 
