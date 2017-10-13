---
title: 基于spring的aop实现多数据源动态切换
date: 2017-07-21 16:13:23
tags: 技术
---

### 一、 多数据源动态切换原理

项目中我们经常会遇到多数据源的问题，尤其是数据同步或定时任务等项目更是如此；又例如：读写分离数据库配置的系统。

#### 1、 多数据源设置：

1）静态数据源切换：
一般情况下，我们可以配置多个数据源，然后为每个数据源写一套对应的sessionFactory和dao层代码(以hibernate为例，mybatis同理)，——我们称之为静态数据源配置。

2）动态数据源切换：
可看出在Dao层代码中写死了两个SessionFactory，这样日后如果再多一个数据源，还要改代码添加一个SessionFactory，显然这并不符合开闭原则。比较好的做法是，配置多个数据源，只对应一套sessionFactory，数据源之间可以动态切换。

#### 2、动态数据源切换时，如何保证数据库的事务：

目前事务最灵活的方式，是使用spring的声明式事务，本质是利用了spring的aop，在执行数据库操作前后，加上事务处理。

spring的事务管理，是基于数据源的，所以如果要实现动态数据源切换，而且在同一个数据源中保证事务是起作用的话，就需要注意二者的顺序问题，即：在事物起作用之前就要把数据源切换回来。

举一个例子：web开发常见是三层结构：controller、service、dao。一般事务都会在service层添加，如果使用spring的声明式事物管理，在调用service层代码之前，spring会通过aop的方式动态添加事务控制代码，所以如果要想保证事物是有效的，那么就必须在spring添加事务之前把数据源动态切换过来，也就是动态切换数据源的aop要至少在service上添加，而且要在spring声明式事物aop之前添加.根据上面分析:

- 最简单的方式是把动态切换数据源的aop加到controller层，这样在controller层里面就可以确定下来数据源了。不过，这样有一个缺点就是，每一个controller绑定了一个数据源，不灵活。对于这种：一个请求，需要使用两个以上数据源中的数据完成的业务时，就无法实现了。
- 针对上面的这种问题，可以考虑把动态切换数据源的aop放到service层，但要注意一定要在事务aop之前来完成。这样，对于一个需要多个数据源数据的请求，我们只需要在controller里面注入多个service实现即可。但这种做法的问题在于，controller层里面会涉及到一些不必要的业务代码，例如：合并两个数据源中的list…
- 此外，针对上面的问题，还可以再考虑一种方案，就是把事务控制到dao层，然后在service层里面动态切换数据源。

### 二、实例：

本例子中，对不同数据源分包（package）管理，同一包下的代码使用了同一数据源。

1、写一个DynamicDataSource类继承AbstractRoutingDataSource，并实现determineCurrentLookupKey方法：

``` java
import org.springframework.jdbc.datasource.lookup.AbstractRoutingDataSource;
public class DynamicDataSource extends AbstractRoutingDataSource {
	@Override
	protected Object determineCurrentLookupKey() {
		return CustomerContextHolder.getCustomerType();
	}
}
```

2、利用ThreadLocal解决线程安全问题：

``` java
public class CustomerContextHolder {
	public static final String DATA_SOURCE_A = "dataSource";
	public static final String DATA_SOURCE_B = "dataSource2";
	private static final ThreadLocal<String> contextHolder = new ThreadLocal<String>();
	public static void setCustomerType(String customerType) {
		contextHolder.set(customerType);
	}
	public static String getCustomerType() {
		return contextHolder.get();
	}
	public static void clearCustomerType() {
		contextHolder.remove();
	}
}
```

3、定义一个数据源切面类，通过aop来控制数据源的切换：

``` java
import org.aspectj.lang.JoinPoint;

public class DataSourceInterceptor {

	public void setdataSourceMysql(JoinPoint jp) {
		DatabaseContextHolder.setCustomerType("dataSourceMySql");
	}
	
	public void setdataSourceOracle(JoinPoint jp) {
		DatabaseContextHolder.setCustomerType("dataSourceOracle");
	}
}
```

4、在spring的application.xml中配置多个dataSource：

``` xml
<!-- 数据源1 -->
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
	       <property name="driverClassName" value="net.sourceforge.jtds.jdbc.Driver"></property>
		<property name="url" value="jdbc:jtds:sqlserver://10.82.81.51:1433;databaseName=standards"></property>
		<property name="username" value="youguess"></property>
		<property name="password" value="youguess"></property>
</bean>
<!-- 数据源2 -->
<bean id="dataSource2" class="org.apache.commons.dbcp.BasicDataSource">
	       <property name="driverClassName" value="net.sourceforge.jtds.jdbc.Driver"></property>
		<property name="url" value="jdbc:jtds:sqlserver://10.82.81.52:1433;databaseName=standards"></property>
		<property name="username" value="youguess"></property>
		<property name="password" value="youguess"></property>
</bean>

<bean id="dataSource" class="com.core.DynamicDataSource">
	<property name="targetDataSources">
		<map key-type="java.lang.String">
			<entry key="dataSourceMySql" value-ref="dataSourceMySql" />
			<entry key="dataSourceOracle" value-ref="dataSourceOracle" />
		</map>
	</property>
	<property name="defaultTargetDataSource" ref="dataSourceMySql" />
</bean>

<!-- 动态数据源切换aop 先与事务的aop  -->
<bean id="dataSourceInterceptor" class="com.core.DataSourceInterceptor" />
<aop:config>
	<aop:aspect id="dataSourceAspect" ref="dataSourceInterceptor">
		<aop:pointcut id="dsMysql" expression="execution(* com.service.mysql..*.*(..))" />
		<aop:pointcut id="dsOracle" expression="execution(* com.service.oracle..*.*(..))" />
		<aop:before method="setdataSourceMysql" pointcut-ref="dsMysql"/>
		<aop:before method="setdataSourceOracle" pointcut-ref="dsOracle"/>
	</aop:aspect>
</aop:config>

<!-- 事物aop -->
。。。
```

>https://lanjingling.github.io/2016/02/15/spring-aop-dynamicdatasource/