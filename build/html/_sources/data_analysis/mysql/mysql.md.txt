# 简介

数据库操作语法笔记

# 数据库操作

创建数据库

```mysql
CREATE DATABASE dbname
```

查看数据库

```mysql
show databases
```

选择数据库

```mysql
USE dbname
```

查看数据库中的表

```mysql
show tables
```

删除数据库

```mysql
drop database dbname
```

# 数据表操作

创建数据表

```python
data = np.random.rand(500, 10)  # 500 entities, each contains 10 features
label = np.random.randint(2, size=500)  # binary target
train_data = lgb.Dataset(data, label=label)
```



