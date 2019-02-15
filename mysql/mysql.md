# GRANT命令

GRANT命令用来授予MYSQL用户的权限，这些权限分为**全局/数据库/表/列**

```
GRANT privileges [columns] ON item TO user_name [IDENTIFIED BY 'password'] [REQUIRE ssl_options] [WITH [GRANT OPTION |
limit_options] ]
```

用户的权限

| 权限 | 应用于 | 描述 |
| :---: | :--- | :--- |
| SELECT | 表，列 | 允许用户从表中选择行（记录） |
| INSERT | 表，列 | 允许用户在表中插入新行 |
| UPDATE | 表，列 | 允许用户修改现存表里行中的值 |
| DELETE | 表 | 允许用户删除现存表的行 |
| INDEX | 表 | 允许用户创建和拖动特定表索引 |
| ALTER | 表 | 允许用户改变现存表的结构，列，修改列的数据类型 |
| CREATE | 数据库，表 | 允许用户创建新数据库或表。如果在GRANT中指定了一个特定的数据库或表，他们只能狗创建该数据或表，即他们呢首先删除（drop）它 |
| DROP | 数据库，表 | 运行用户拖动（删除）数据库或表 |

1

