### 推荐查看方式：
- [传送门](https://github.com/WhyWhatHow/Software/blob/master/JavaEE/lab6.md)
- 源码： [ssmPrj](../.local/static/2019/5/0/SSMPrj.1560088918350.zip)
- sql 文件：
[homework](../.local/static/2019/5/0/homework.1560089102479.sql)
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
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	 http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
	http://www.springframework.org/schema/context
	 http://www.springframework.org/schema/context/spring-context-4.0.xsd
	http://www.springframework.org/schema/aop
	 http://www.springframework.org/schema/aop/spring-aop-4.0.xsd 
	 http://www.springframework.org/schema/tx 
	 http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
	http://www.springframework.org/schema/util 
	http://www.springframework.org/schema/util/spring-util-4.0.xsd">
	<!-- @Service包扫描器 -->
	<context:component-scan base-package="com.sdut.ssm.service"/>
</beans>

```
- applicationContext-trans.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	 http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
	http://www.springframework.org/schema/context
	 http://www.springframework.org/schema/context/spring-context-4.0.xsd
	http://www.springframework.org/schema/aop
	 http://www.springframework.org/schema/aop/spring-aop-4.0.xsd
	  http://www.springframework.org/schema/tx
	   http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
	http://www.springframework.org/schema/util 
	http://www.springframework.org/schema/util/spring-util-4.0.xsd">

	<!-- 事务管理器 -->
	<bean id="transactionManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<!-- 数据源 -->
		<property name="dataSource" ref="dataSource" />
	</bean>

	<!-- 通知 -->
	<tx:advice id="txAdvice" transaction-manager="transactionManager">
		<tx:attributes>
			<!-- 传播行为 -->
			<tx:method name="save*" propagation="REQUIRED" />
			<tx:method name="insert*" propagation="REQUIRED" />
			<tx:method name="delete*" propagation="REQUIRED" />
			<tx:method name="update*" propagation="REQUIRED" />
			<tx:method name="find*" propagation="SUPPORTS" read-only="true" />
			<tx:method name="get*" propagation="SUPPORTS" read-only="true" />
			<tx:method name="query*" propagation="SUPPORTS" read-only="true" />
		</tx:attributes>
	</tx:advice>

	<!-- 切面 -->
	<aop:config>
		<aop:advisor advice-ref="txAdvice"
			pointcut="execution(* com.sdut.ssm.service.*.*(..))" />
	</aop:config>

</beans>

```

##### springmvc 配置： springmvc.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans
	 	http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/mvc 
        http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-4.0.xsd">

	<!-- 配置@controller扫描包 -->
	<context:component-scan base-package="com.sdut.ssm.controller" />
	
	
	<!-- 配置注解驱动，相当于同时使用最新处理器映射跟处理器适配器,对json数据响应提供支持 -->
	<mvc:annotation-driven />
	<!-- 使用自定义转换器 -->
<!-- 	<mvc:annotation-driven conversion-service="MyConvert" /> -->
	
	<!-- 定义转换器 -->
<!-- 	<bean id="MyConvert" class="org.springframework.format.support.FormattingConversionServiceFactoryBean"> -->
<!-- 		<property name="converters"> -->
<!-- 			<set> -->
<!-- 				<bean class="com.itheima.springmvc.utils.DateConvert" /> -->
<!-- 			</set> -->
<!-- 		</property> -->
<!-- 	</bean> -->
	
	<!-- 配置视图解析器 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/jsp/"/>
		<property name="suffix" value=".jsp"/>
	</bean>
	
	<!-- 配置全局异常处理器 -->
<!-- 	<bean class="com.itheima.springmvc.exception.CustomerExceptionResolver"/> -->
	
	<!-- 配置多媒体处理器 -->
	<!-- 注意：这里id必须填写：multipartResolver -->
<!-- 	<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"> -->
		<!-- 最大上传文件大小 -->
<!-- 		<property name="maxUploadSize" value="8388608" /> -->
<!-- 	</bean> -->
	
<!-- 	<mvc:interceptors> -->
<!-- 		<mvc:interceptor> -->
			<!-- /**拦截所有请求，包括二级以上目录 -->
<!-- 			<mvc:mapping path="/**"/> -->
<!-- 			<bean class="com.itheima.springmvc.interceptor.MyInterceptor" /> -->
<!-- 		</mvc:interceptor> -->
<!-- 		<mvc:interceptor> -->
			<!-- /**拦截所有请求，包括二级以上目录 -->
<!-- 			<mvc:mapping path="/**"/> -->
<!-- 			<bean class="com.itheima.springmvc.interceptor.MyInterceptor2" /> -->
<!-- 		</mvc:interceptor> -->
<!-- 		<mvc:interceptor> -->
			<!-- /**拦截所有请求，包括二级以上目录 -->
<!-- 			<mvc:mapping path="/**"/> -->
			<!-- 配置不拦截目录 -->
<!-- 			<mvc:exclude-mapping path="/user/**"/> -->
<!-- 			<bean class="com.itheima.springmvc.interceptor.LoginInterceptor" /> -->
<!-- 		</mvc:interceptor> -->
<!-- 	</mvc:interceptors> -->
	
</beans>

```
 
##### mybatis 配置： mybatis-config.xml
``xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
</configuration>
``

####（4）在WebContent下创建 jsp文件 如下：
- studentList.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt"  prefix="fmt"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>查询学生信息 </title>
</head>
<body> 
<span>${ msg }</span>
<br>	
	<button><a href="${pageContext.request.contextPath }/preAddStudent.action">添加一个学生</a></button>
<br>
<br>
<form action="${pageContext.request.contextPath }/queryStudent.action" method="post">
查询条件：
<table width="100%" border=1>
<tr>
<td>学生姓名:<input type="text" name="name" ></td>
<td>学生年龄:<input type="text" name="age"  ></td>
<td><input type="submit" value="查询"/></td>
</tr>
<tr>
</tr>
</table>
<br>

学生列表：
<table width="100%" border=1>
<tr>
	<td>选择</td>
	<td>学生姓名</td>
	<td>学生年龄</td>
	<td colspan="2">操作</td>
</tr>
<c:forEach items="${list }" var="stu" varStatus="status">
<tr>
	<td><input type="checkbox" name="ids" value="${stu.id}"></td>
	<td>
	<input type="hidden" name="items[${ status.index }].id" value="${stu.id}">
	<input type="text" name="items[${ status.index }].name" value="${stu.name }"></td>
	<td><input type="text" name="items[${ status.index }].age" value="${stu.age }"></td>
	<td><a href="${pageContext.request.contextPath }/deleteStudent.action?id=${stu.id}">删除</a></td>
	<td><a href="${pageContext.request.contextPath }/editStudent.action?id=${stu.id}">修改</a></td>
</tr>
</c:forEach>

</table>
</form>
</body>

</html>
```
- studentEdit.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt"  prefix="fmt"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>1. 
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>修改信息</title>

</head>
<body> 
<span>${ msg }</span>
	<!-- 上传图片是需要指定属性 enctype="multipart/form-data" -->
	<!-- <form id="stuForm" action="" method="post" enctype="multipart/form-data"> -->
	<form id="student"	action="${pageContext.request.contextPath }/updateStudent.action" 
	method="post">
		<input type="hidden" name="id" value="${stu.id }" /> 修改学生个人信息：
		<table width="100%" border=1>
			<tr>
				<td>学生姓名</td>
				<td><input type="text" name="name" value="${stu.name }" /></td>
			</tr>
			<tr>
				<td>学生年龄</td>
				<td><input type="text" name="age" value="${stu.age }" /></td>
			</tr>
			
			
			<tr>
				<td colspan="2" align="center"><input type="submit" value="提交" />
				</td>
			</tr>
		</table>

	</form>
</body>

</html>
```
- addStudent.jsp
```jsp 
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/fmt"  prefix="fmt"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>添加學生</title>

</head>
<body> 
	<!-- 上传图片是需要指定属性 enctype="multipart/form-data" -->
	<!-- <form id="stuForm" action="" method="post" enctype="multipart/form-data"> -->
	<form id="student"	action="${pageContext.request.contextPath }/addStudent.action" 
	method="post">
		<input type="hidden" name="id" value="${stu.id }" /> 添加學生个人信息：
		<table width="100%" border=1>
			<tr>
				<td>学生姓名</td>
				<td><input type="text" name="name"  /></td>
			</tr>
			<tr>
				<td>学生年龄</td>
				<td><input type="text" name="age"  /></td>
			</tr>
			
			
			<tr>
				<td colspan="2" align="center"><input type="submit" value="提交" />
				</td>
			</tr>
		</table>

	</form>
</body>

</html>
```
####（5）com.sdut.ssm.mapper 下创建dao ：StudentMapper接口：
- studentMapper.java
``` java
package com.sdut.ssm.mapper;

import java.util.List;

import com.sdut.ssm.pojo.Student;

public interface StudentMapper {

		/**   
		 * @Description:  返回所有学生信息， 用于进入信息
		 * @return：
		 * @throws：
		 * @version: v1.0.0
		 * @author: WhyWhatHow
		 * @date: 2019年6月9日  
		 * Modification History:
		 * Date         Author          Version            Description
		 *---------------------------------------------------------*
		 * 2019年6月9日     John Nash           v1.0.0               修改原因
		 */
		List<Student> getAllStudents();
		
		/**   
		 * @Description: Dao 根据学生信息查询学生（模糊查询等）
		 * @param stu
		 * @return
		 * @return： 相关学生信息列表 
		 * @throws：
		 * @version: v1.0.0
		 * @author: WhyWhatHow
		 * @date: 2019年6月9日  
		 * Modification History:
		 * Date         Author          Version            Description
		 *---------------------------------------------------------*
		 * 2019年6月9日     John Nash           v1.0.0               修改原因
		 */
		List<Student> getStudentsByVo(Student stu);
		
		int deleteStudentById(Integer id );
		
		int updateByStudent(Student stu);
		
		int insertStudent(Student stu);

		Student getStudentById(Integer id);
}

```
- studentMapper.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.sdut.ssm.mapper.StudentMapper">
	<select id="getStudentById" parameterType="int" resultType="student">
		select * from student where id =#{id}	
	</select>
	<select id="getAllStudents" resultType="student">
		select * from student
	</select>
	<select id="getStudentsByVo" parameterType="student"
		resultType="student">
		select * from student
		<where>	 
		<if test="name!=null and name!=''">
			 and name like '%${name}%' 
		</if>
		<if test="age !=null  ">
			and age = #{age}  
		</if>
		</where>
	</select>
	<delete id="deleteStudentById" parameterType="int" >
		 delete from student where id = #{id}
	</delete>
	<insert id="insertStudent" parameterType="student" >
		 insert into  student values(null,#{name},#{age});
	</insert>
	<update id="updateByStudent" parameterType="student">
		update student  <set>
			 <if test="name!=null and name!=''">
			 	name = #{name} , 
			 </if>
			 <if test="age!=null">
			 age=#{age}
			 </if>
		</set>
		where id=#{id}
	</update>
</mapper>
```


####（6) com.sdut.ssm.service.impl 下创建StudentServiceImpl类
- StudentServiceImpl.java
```java
package com.sdut.ssm.service.impl;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.sdut.ssm.mapper.StudentMapper;
import com.sdut.ssm.pojo.Student;
import com.sdut.ssm.service.StudentService;


@Service
public class StudentServiceImpl implements StudentService {
	
	@Autowired
	private StudentMapper mapper;

	@Override
	public List<Student> getAllStudents() {
		return mapper.getAllStudents();
	
	}

	@Override
	public List<Student> getStudentsByVo(Student stu) {
		 return mapper.getStudentsByVo(stu);
	}
	@Override
	public int deleteStudentById(Integer id ) {

		return mapper.deleteStudentById(id);
		
	}

	@Override
	public int updateByStudent(Student stu) {
		 return mapper.updateByStudent(stu);
		 
	}

	@Override
	public int insertStudent(Student stu) {
		 return mapper.insertStudent(stu);
	}

	@Override
	public Student getStudentById(Integer id) {
		 return mapper.getStudentById(id);
		 
	}
	
	 
	
	
	
	

}
```

####（7）在com.sdut.ssm.controller 下创建 StudentController.java：
```java
package com.sdut.ssm.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import com.sdut.ssm.pojo.Student;
import com.sdut.ssm.service.StudentService;
/*
 * 有时候自己可能因为在eclipse 上的错误操作，导致 软件环境变量发生错误，导致项目失败
 */

@Controller
public class StudentController {

	@Autowired
	private StudentService service;

	@RequestMapping("list")
	public String getList(Model model) {
		List<Student> list = service.getAllStudents();
		System.out.println("getList+===================================");
		model.addAttribute("list", list);
		model.addAttribute("msg","默認查潤的結果。。。。。");
		for (Student student : list) {
			System.out.println(student);
		}
		return "studentList";
	}
	
	@RequestMapping("queryStudent")
	public String queryStudent(Student student,Model model ) {
		System.out.println("================================================");
		List<Student> list = service.getStudentsByVo(student);
		System.out.println(student);
		model.addAttribute("list",list);
		model.addAttribute("msg","條件查詢的結果。。。。。");
		
		return "studentList";
	
	}
	
	@RequestMapping("deleteStudent")
	public  String   deleteStudent(Integer id ) {
		service.deleteStudentById(id);
		return "redirect:list.action";		
	}
	
	@RequestMapping("editStudent")
	public  String editStudent(Integer id ,Model model ) {
			Student student = service.getStudentById(id);
			model.addAttribute("stu",student);
			return "studentEdit";
	}
	@RequestMapping("updateStudent")
	public 	String  updateByStudent( Student stu, Model model  ) {
	
		System.out.println("update by student ....."+stu);
		model.addAttribute("msg", "修改学生信息成功............");
		
		service.updateByStudent(stu);
			 
		
		return "forward:/list.action";		
	}
	@RequestMapping("preAddStudent")
	public String preAddStudent(){
		return "addStudent";
	}
	@RequestMapping("addStudent")
	public String addStudent(Student stu ,Model model )  {
		System.out.println("insert ....");
		model.addAttribute("msg","添加学生成功...........");
		service.insertStudent(stu);
		return "forward:/list.action";
	}
	
	
	

//	@RequestMapping("hello") 
//	public String getHello() {
//		System.out.println("hello , this is springmvc  ");
//		System.out.println("hello ...............");
//		return "studentList";
//	}

}


```

