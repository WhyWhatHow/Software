### Src: [springmvc-01](../.local/static/2019/5/0/springmvc-01.1559480190887.zip)
### Result :
 
#### 　false : 
![false](../.local/static/2019/5/0/false.png)

![false-result](../.local/static/2019/5/0/false_result.1559480643785.png)
#### success : 
![jee-input](../.local/static/2019/5/0/jee-input.1559480743513.png)

![jee-success](../.local/static/2019/5/0/jee-succ.1559480785973.png)

### （1）创建WEB Project，项目名称为：SPringmvc-01。


### （2）加载JAR包：
```java
commons-logging-1.1.1.jar         spring-core-4.2.4.RELEASE.jar
jstl-1.2.jar                      spring-expression-4.2.4.RELEASE.jar
spring-aop-4.2.4.RELEASE.jar      spring-web-4.2.4.RELEASE.jar
spring-beans-4.2.4.RELEASE.jar    spring-webmvc-4.2.4.RELEASE.jar
spring-context-4.2.4.RELEASE.jar

```


并将JAR包配置到类路径。


### （3）在com.sdut.pojo包里创建Student，代码如下：
```java
package com.sdut.pojo;

public class Student {

	private Integer id;
	private String name;
	private Integer age;

	public void setId(Integer id) {
		this.id = id;
	}

	public void setAge(Integer age) {
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public Student() {
		super();
		// TODO Auto-generated constructor stub
	}

	public Integer getId() {
		return id;
	}

	public Integer getAge() {
		return age;
	}

	public Student(Integer id, String name, Integer age) {
		super();
		this.id = id;
		this.name = name;
		this.age = age;
	}

	@Override
	public String toString() {
		return "Student [id=" + id + ", name=" + name + ", age=" + age + "]";
	}

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + ((age == null) ? 0 : age.hashCode());
		result = prime * result + ((id == null) ? 0 : id.hashCode());
		result = prime * result + ((name == null) ? 0 : name.hashCode());
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Student other = (Student) obj;
		if (age == null) {
			if (other.age != null)
				return false;
		} else if (!age.equals(other.age))
			return false;
		if (id == null) {
			if (other.id != null)
				return false;
		} else if (!id.equals(other.id))
			return false;
		if (name == null) {
			if (other.name != null)
				return false;
		} else if (!name.equals(other.name))
			return false;
		return true;
	}


}

```

### （4）com.sdut.controller 下创建　StudentController 

```java
package com.sdut.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import com.sdut.pojo.Student;

@Controller
public class StudentController {

	@RequestMapping("student")
	public String student(Student stu, Model model) {
		System.out.println(stu);

		if (stu.getAge() != null || stu.getId() != null || stu.getName() != null && stu.getName() != "") {
			model.addAttribute("stu", stu);
			return "studentInfo";

		} else {
			// 模拟查询商品列表
			return "success";

		}
	}
}

```

### （5）config 文件夹下创建 springmvc.xml :
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

	<!-- 配置controller扫描包 -->
	<context:component-scan
		base-package="com.sdut.controller" />

	<!-- 配置处理器映射器 -->
	<!-- <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/> -->
	<!-- 配置处理器适配器 -->
	<!-- <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/> -->

	<!-- 配置注解驱动，相当于同时使用最新处理器映射跟处理器适配器,对json数据响应提供支持 -->
	<mvc:annotation-driven />

	<!-- 配置视图解析器 -->
	<bean
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/jsp/" />
		<property name="suffix" value=".jsp" />
	</bean>
</beans>

```

