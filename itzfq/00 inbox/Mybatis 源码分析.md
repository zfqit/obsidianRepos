## Mybatis 基本源码演示图

![Mybatis.png](https://raw.githubusercontent.com/zfqit/images/main/img/202310301439329.png)

## 源码解析

Configuration

1. 读取 `mybatis-config.xml` 配置文件配置并转换为属性
2. 创建 MappedStatement
	1. 解析 `mapper.xml` 配置文件中`select` `insert` `update` `delete` 等标签并转换为属性
3. 创建后续需要的对象
	1. ParameterHandler
	2. ResultSetHandler
	3. StatementHandler
	4. Executor

执行流程

1. SqlSession 开始调用 selectOne ...方法
	1. 默认实现 DefaultSqlSession
2. Executor 执行器
	1. 主要作用
		1. 事务控制
		2. 执行 `query` `update` 操作, 主要是调用 StatementHandler 来实现
	2. 实现类
		1. BatchExecutor 批处理执行器
		2. CachingExecutor 缓存执行器
		3. ReuseExecutor 复用执行器
		4. SimpleExecutor 简单执行器 (常用)
3. StatementHandler 
	1. 实现类 参考 JDBC 中的 Statement 类型
		1. SimpleStatementHandler 数据库通用访问使用
		2. CallableStatementHandler 存储过程使用
		3. PreparedStatementHandler 预编译的 Statement
4. ParamenterHandler
	1. 把 `mapper.xml` 文件的 #{} 转换为 jdbc中的 ?  占位符
5. ResultSetHandler
	1. 处理返回结果集