# Mybatis

## Mybatis是什么

是一款优秀的持久层框架，一个半ORM框架，支持定制化SQL，存储过程和高级映射

## ORM

对象映射关系，Hibernate是全自动ORM映射工具，Mybatis是半自动ORM映射工具

## 传统JDBC开发存在的问题

+ 频繁创建连接，造成系统浪费影响性能，也可以通过连接池来优化，但是需要自己实现
+ sql语句硬编码，项目开发中，sql语句变化很大
+ 结果集处理比较麻烦，直接映射成java对象比较方便

## mybatis如何解决

+ 连接池可以在xml中进行配置
+ sql语句写在mapper中，与java代码分离
+ 自动将结果集转换为java对象

## mybatis的解析和运行原理

### mybatis编程的步骤

1. 创建SqlSessionFactory
2. 通过SqlSessionFactory创建SqlSession
3. 通过SqlSession执行数据库操作
4. 调用session.commit()提交事务
5. 调用session.close()关闭会话

