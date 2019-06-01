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

```

- insert
```java

```
- updateByStudent
```java

```