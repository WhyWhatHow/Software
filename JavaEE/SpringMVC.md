### （1）创建Java Project，项目名称为：SpringMybatisPrj。

### （2）加载JAR包：
```shell
aopalliance-1.0.jar                 slf4j-api-1.7.5.jar
asm-3.3.1.jar                       slf4j-log4j12-1.7.5.jar
aspectjweaver-1.6.11.jar            spring-aop-4.2.4.RELEASE.jar
cglib-2.2.2.jar                     spring-aspects-4.2.4.RELEASE.jar
commons-dbcp-1.2.2.jar              spring-beans-4.2.4.RELEASE.jar
commons-logging-1.1.1.jar           spring-context-4.2.4.RELEASE.jar
commons-pool-1.3.jar                spring-context-support-4.2.4.RELEASE.jar
javassist-3.17.1-GA.jar             spring-core-4.2.4.RELEASE.jar
jstl-1.2.jar                        spring-expression-4.2.4.RELEASE.jar
junit-4.9.jar                       spring-jdbc-4.2.4.RELEASE.jar
log4j-1.2.17.jar                    spring-jms-4.2.4.RELEASE.jar
log4j-api-2.0-rc1.jar               spring-messaging-4.2.4.RELEASE.jar
log4j-core-2.0-rc1.jar              spring-test-4.2.4.RELEASE.jar
mybatis-3.2.7.jar                   spring-tx-4.2.4.RELEASE.jar
mybatis-spring-1.2.2.jar            spring-web-4.2.4.RELEASE.jar
mysql-connector-java-5.1.7-bin.jar
```
### (3) mybatis 配置文件：mybatis-config.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>	
</configuration>

```
### （4）spring 配置文件 ApplicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd">

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
		<property name="typeAliasesPackage" value="com.sdut.pojo" />
	</bean>

	<!-- 动态代理Dao开发，第一种方式，包扫描器(推荐使用) -->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<!-- basePackage：配置映射包装扫描，多个包时用","或";"分隔 -->
		<property name="basePackage"
			value="com.sdut.mapper" />
	</bean>

	<!-- 动态代理，第二种方式：包扫描（推荐）： -->
<!-- 	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer"> -->
	<!-- basePackage多个包用","分隔 --> 
<!-- 		<property name="basePackage" value="com.sdut.mapper" /> -->
<!-- 	</bean> -->
</beans>

```
### （5）. 复制jdbc.properties配置文件到新工程
### （6）. 复制log4j.properties配置文件到新工程
### （7） com.sdut.pojo 下创建 Student.java 

###  (8)  com.sdut.mapper 下创建StudentMapper.xml,StudentMapper.java 
#### StudentMapper.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.sdut.mapper.StudentMapper">

<select id="selectById" parameterType="int"
		resultType="student">
		select distinct * from
		student	
		where  id = #{id}
	</select>
	<select id="selectAll" resultType="student">
		select distinct * from
		student
	</select>
	<insert id="insert" parameterType="student">
		insert student(id, name ,age) values (#{id},#{name},#{age})
	</insert>
	<delete id="deleteById">
		delete from student where id =#{id} 
	</delete>
	<update id="updateByStudent" parameterType="student"> 
		 update student  
		 <set>	
		<if test="name!=null and  name != '' ">
			name =#{name},
		</if>
		<if test="age!=null and  age != 0 ">
			age =#{age},
		</if>
		 </set>
		 where id = #{id}
	</update>
</mapper>

```
#### StudentMapper.java
```java
package com.sdut.mapper;

import java.util.List;

import com.sdut.pojo.Student;

public interface StudentMapper {
//	增加学生、删除学生、修改学生、查询所有学生、根据ID查询一名学生。
	public  Student selectById(int id );	
	public List<Student> selectAll();
	public int deleteById(int id );
	public int updateByStudent(Student stu) ; 
	public int insert(Student student );
}
```
###（9）在 com.sdut.test 

（9）执行测试类，系统输出效果为：

### result : 
- selectALL
```java
DEBUG [main] - Creating a new SqlSession
DEBUG [main] - SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@34129c78] was not registered for synchronization because synchronization is not active
DEBUG [main] - Fetching JDBC Connection from DataSource
DEBUG [main] - JDBC Connection [jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8, UserName=root@localhost, MySQL-AB JDBC Driver] will not be managed by Spring
DEBUG [main] - ==>  Preparing: select distinct * from student 
DEBUG [main] - ==> Parameters: 
DEBUG [main] - <==      Total: 27
DEBUG [main] - Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@34129c78]
DEBUG [main] - Returning JDBC Connection to DataSource
Student [sid=1, sname=xingpengfei, sage=21]
Student [sid=3, sname=proiden, sage=16]
Student [sid=4, sname=sit ame, sage=15]
Student [sid=5, sname=e eu fugia, sage=14]
Student [sid=6, sname=e dolor in, sage=13]
Student [sid=7, sname=stru, sage=12]
Student [sid=8, sname=e irure, sage=11]
Student [sid=9, sname=t aliq, sage=10]
Student [sid=10, sname=nt mol, sage=9]
Student [sid=11, sname=Jhon, sage=21]
Student [sid=18, sname=wiwian, sage=21]
Student [sid=19, sname=wiwn, sage=21]
Student [sid=20, sname=Poki, sage=21]
Student [sid=21, sname=xiaowang, sage=21]
Student [sid=22, sname=xiaofei, sage=22]
Student [sid=23, sname=xiaoxin, sage=23]
Student [sid=24, sname=xiaojia, sage=22]
Student [sid=25, sname=xiaowang, sage=21]
Student [sid=26, sname=xiaofei, sage=22]
Student [sid=27, sname=xiaoxin, sage=23]
Student [sid=28, sname=xiaojia, sage=22]
Student [sid=29, sname=张三, sage=20]
Student [sid=30, sname=李四, sage=21]
Student [sid=31, sname=王五, sage=22]
Student [sid=32, sname=王华, sage=23]
Student [sid=33, sname=中华, sage=24]
Student [sid=34, sname=李华, sage=25]
```
- selectById(3)
```java
DEBUG [main] - Creating a new SqlSession
DEBUG [main] - SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@6150c3ec] was not registered for synchronization because synchronization is not active
DEBUG [main] - Fetching JDBC Connection from DataSource
DEBUG [main] - JDBC Connection [jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8, UserName=root@localhost, MySQL-AB JDBC Driver] will not be managed by Spring
DEBUG [main] - ==>  Preparing: select distinct * from student where id = ? 
DEBUG [main] - ==> Parameters: 3(Integer)
DEBUG [main] - <==      Total: 1
DEBUG [main] - Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@6150c3ec]
DEBUG [main] - Returning JDBC Connection to DataSource
Student [sid=3, sname=proiden, sage=16]
=======================================================
```
- deleteByID
```java
============================================================
DEBUG [main] - Creating a new SqlSession
DEBUG [main] - SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@34129c78] was not registered for synchronization because synchronization is not active
DEBUG [main] - Fetching JDBC Connection from DataSource
DEBUG [main] - JDBC Connection [jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8, UserName=root@localhost, MySQL-AB JDBC Driver] will not be managed by Spring
DEBUG [main] - ==>  Preparing: delete from student where id =? 
DEBUG [main] - ==> Parameters: 40(Integer)
DEBUG [main] - <==    Updates: 1
DEBUG [main] - Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@34129c78]
DEBUG [main] - Returning JDBC Connection to DataSource
```

- insert
```java
============================================================
DEBUG [main] - Creating a new SqlSession
DEBUG [main] - SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@34129c78] was not registered for synchronization because synchronization is not active
DEBUG [main] - Fetching JDBC Connection from DataSource
DEBUG [main] - JDBC Connection [jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8, UserName=root@localhost, MySQL-AB JDBC Driver] will not be managed by Spring
DEBUG [main] - ==>  Preparing: insert student(id, name ,age) values (?,?,?) 
DEBUG [main] - ==> Parameters: 40(Integer), whywahtHow(String), 21(Integer)
DEBUG [main] - <==    Updates: 1
DEBUG [main] - Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@34129c78]
DEBUG [main] - Returning JDBC Connection to DataSource
```
- updateByStudent
```java
DEBUG [main] - Creating a new SqlSession
DEBUG [main] - SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@34129c78] was not registered for synchronization because synchronization is not active
DEBUG [main] - Fetching JDBC Connection from DataSource
DEBUG [main] - JDBC Connection [jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8, UserName=root@localhost, MySQL-AB JDBC Driver] will not be managed by Spring
DEBUG [main] - ==>  Preparing: select distinct * from student where id = ? 
DEBUG [main] - ==> Parameters: 29(Integer)
DEBUG [main] - <==      Total: 1
DEBUG [main] - Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@34129c78]
DEBUG [main] - Returning JDBC Connection to DataSource
select by id 29: before change : Student [sid=29, sname=张三, sage=20]===================
DEBUG [main] - Creating a new SqlSession
DEBUG [main] - SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@5b3f61ff] was not registered for synchronization because synchronization is not active
DEBUG [main] - Fetching JDBC Connection from DataSource
DEBUG [main] - JDBC Connection [jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8, UserName=root@localhost, MySQL-AB JDBC Driver] will not be managed by Spring
DEBUG [main] - ==>  Preparing: update student SET name =?, age =? where id = ? 
DEBUG [main] - ==> Parameters: lucy(String), 25(Integer), 29(Integer)
DEBUG [main] - <==    Updates: 1
DEBUG [main] - Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@5b3f61ff]
DEBUG [main] - Returning JDBC Connection to DataSource
DEBUG [main] - Creating a new SqlSession
DEBUG [main] - SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@327af41b] was not registered for synchronization because synchronization is not active
DEBUG [main] - Fetching JDBC Connection from DataSource
DEBUG [main] - JDBC Connection [jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8, UserName=root@localhost, MySQL-AB JDBC Driver] will not be managed by Spring
DEBUG [main] - ==>  Preparing: select distinct * from student where id = ? 
DEBUG [main] - ==> Parameters: 29(Integer)
DEBUG [main] - <==      Total: 1
DEBUG [main] - Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@327af41b]
DEBUG [main] - Returning JDBC Connection to DataSource
select by id 29 : after change : Student [sid=29, sname=lucy, sage=25]================
=============================================================
```