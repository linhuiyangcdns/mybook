# 最常用的SQLAlchemy列表型

| 类型名 | Python类型 | 说明 |
| :--- | :--- | :--- |
| Integer | int | 普通整数，一般32位 |
| SmallInteger | int | 取值范围小的整数，一般是16位 |
| BigInteger | int或long | 不限制精度的整数 |
| Float | float | 浮点数 |
| Numeric | decimal.Decimal | 定点数 |
| String | str | 变长字符串 |
| Text | str | 变长字符串，对较长或不限长度的字符串做了优化 |
| Unicode | unicode | 变长Unicode字符串 |
| UnicodeText | unicode | 变长Unicode字符串，对较长或不限长度的字符串做了优化 |
| Boolean | bool | 布尔值 |
| Date | datetime.date | 日期 |
| Time | datetime.time | 时间 |
| DateTime | datetime.datetime | 日期和时间 |
| Interval | datetime.timedelta | 时间间隔 |
| Enum | str | 一组字符串 |
| PickleType | 如何Pthon对象 | 自动使用Pickle序列化 |
| LargeBinary | str | 二进制文件 |

# 最常使用的SQLAlchemy列选项

| 选项名 | 说明 |
| :--- | :--- |
| primary\_key | 如果设为True，这列就是表的主键 |
| unique | 如果设为True,这列不允许出现重复的值 |
| index | 如果设为True，为这列创建索引，提升查询效率 |
| nullable | 如果设为True,这列允许使用空值；如果设为False，这列不允许使用空值 |
| default | 为这列定义默认值 |

# 最常用的SQLAlchemy关系选项

| 选项名 | 说明 |
| :--- | :--- |
| backref | 在关系的另一个模型中添加反向引用 |
| primaryjoin | 明确指定两个模型之间使用的联结条件。只在模棱两可的关系中需要指定 |
| lazy | 指定如何加载相关记录。可选值有select（首次访问时按需加载）、imediate（源对象加载后就加载），joiner\(加载记录，但使用联结）、subquery（立即加载，但使用子查询），noload（永不加载）和dynamic\(不加载记录，但提供加载记录的查询） |
| uselist | 如果设为Fales，不使用列表，而使用标量值 |
| order\_by | 指定关系中记录的排序方式 |
| secondary | 指定多对多记录的排序方式 |
| secondaryjoin | SQLAlchemt无法自行决定时，指定多对多关系中的二级联结条件 |



