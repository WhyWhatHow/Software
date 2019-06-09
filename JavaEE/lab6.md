### 推荐查看方式：
- [传送门](https://github.com/WhyWhatHow/Software/blob/master/JavaEE/lab6.md)
### finished:
- 对学生表的增删改查(必做四个完成）
- 对学生表的模糊查询，（动态 sql）（选做3完成）

### question: 
- 关于 request scope 的问题，以及 请求参数是如何与对应的类文件形成的映射

### result：
- 默认查询： 
 
![默认查询](../.local/static/2019/5/0/normal.1560086648511.png)


- 模糊查询:
- ![title](../.local/static/2019/5/0/rule-query.1560086668460.png)
- 删除用户信息前：
- ![title](../.local/static/2019/5/0/before-delete.1560086679273.png)
- 删除用户信息后：
- ![title](../.local/static/2019/5/0/after-delete.1560086686058.png) 
- 更新用户信息前： 
- ![title](../.local/static/2019/5/0/before_update.1560086700675.png)
- 更新用户信息后： 
- ![title](../.local/static/2019/5/0/after-update.1560086707194.png)
- 插入用户信息前： 
- ![title](../.local/static/2019/5/0/add-tudent.1560086713692.png)
- 插入用户信息后： 
- ![title](../.local/static/2019/5/0/after-add.1560086717890.png)

### Step：
  
#### 1）创建WEB Project，项目名称为：SSMPrj。

#### 2）加载JAR包：
```java
aopalliance-1.0.jar            mybatis-3.2.7.jar
asm-3.3.1.jar                  mybatis-spring-1.2.2.jar
aspectjweaver-1.6.11.jar       mysql-connector-java-5.1.7-bin.jar
cglib-2.2.2.jar                slf4j-api-1.7.5.jar
commons-dbcp-1.2.2.jar         slf4j-log4j12-1.7.5.jar
commons-fileupload-1.2.2.jar   spring-aop-4.2.4.RELEASE.jar
commons-io-2.4.jar             spring-aspects-4.2.4.RELEASE.jar
commons-logging-1.1.1.jar      spring-beans-4.2.4.RELEASE.jar
commons-pool-1.3.jar           spring-context-4.2.4.RELEASE.jar
jackson-annotations-2.4.0.jar  spring-context-support-4.2.4.RELEASE.jar
jackson-core-2.4.2.jar         spring-core-4.2.4.RELEASE.jar
jackson-databind-2.4.2.jar     spring-expression-4.2.4.RELEASE.jar
javassist-3.17.1-GA.jar        spring-jdbc-4.2.4.RELEASE.jar
jstl-1.2.jar                   spring-jms-4.2.4.RELEASE.jar
junit-4.9.jar                  spring-messaging-4.2.4.RELEASE.jar
log4j-1.2.17.jar               spring-tx-4.2.4.RELEASE.jar
log4j-api-2.0-rc1.jar          spring-web-4.2.4.RELEASE.jar
log4j-core-2.0-rc1.jar         spring-webmvc-4.2.4.RELEASE.jar

```
并将JAR包配置到类路径。

####（3) spring ，springmvc， mybatis  配置（config下） ：
##### spring :
- applicationContext-dao.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
	http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-4.0.xsd
	http://www.springframework.org/schema/aop 
	http://www.springframework.org/schema/aop/spring-aop-4.0.xsd 
	http://www.springframework.org/schema/tx 
	http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
	http://www.springframework.org/schema/util 
	http://www.springframework.org/schema/util/spring-util-4.0.xsd">

	<!-- 加载配置文件 -->
	<context:property-placeholder
		location="classpath:jdbc.properties" />

	<!-- 数据库连接池 -->
	<bean id="dataSource"
		class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
		<property name="driverClassName" value="${jdbc.driver}" />
		<property name="url" value="${jdbc.url}" />
		<property name="username" value="${jdbc.username}" />
		<property name="password" value="${jdbc.password}" />
		<!-- 连接池的最大数据库连接数 -->
		<property name="maxActive" value="10" />
		<!-- 最大空闲数 -->
		
		<property name="maxIdle" value="5" />
	</bean>

	<!-- SqlSessionFactory配置 -->
	<bean id="sqlSessionFactory"
		class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource" />
		<!-- 加载mybatis核心配置文件 -->
		<property name="configLocation"
			value="classpath:mybatis-config.xml" />
		<!-- 别名包扫描 -->
		<property name="typeAliasesPackage"
			value="com.sdut.ssm.pojo" />
	</bean>

	<!-- 动态代理，第二种方式：包扫描（推荐）： -->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<!-- basePackage多个包用","分隔 -->
		<property name="basePackage"
			value="com.sdut.ssm.mapper" />
	</bean>
</beans>

```
- applicationContext-service.xml
- applicationContext-tx.xml

##### springmvc 配置：
 
##### mybatis 配置： 

（4）在。。。包里创建类。。。。。，代码如下：

（5）。。。。。。。。

（6）。。。。。。。

（7）在。。。创建XML文件，文件名为：***.xml，在其中填充代码后，文件内容为：

（8）在。。。包中创建Controller类，代码为：

（8）在。。。创建JSP页面****.jsp，代码为：