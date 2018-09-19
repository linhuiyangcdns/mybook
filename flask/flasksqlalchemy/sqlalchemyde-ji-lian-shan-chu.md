# sqlalchemy的级联删除

```
class MyClass(Base):
    __tablename__ = 'mytable'
    id = Column(Integer, primary_key=True)
    children = relationship("MyOtherClass",
                    cascade="all, delete-orphan",
                    passive_deletes=True)

class MyOtherClass(Base):
    __tablename__ = 'myothertable'
    id = Column(Integer, primary_key=True)
    parent_id = Column(Integer,
                ForeignKey('mytable.id', ondelete='CASCADE')
                    )
```

外键约束有以下几项：

1.`RESTRICT`：父表数据被删除，会阻止删除。默认就是这一项。

2.`NO ACTION`：在MySQL中，同`RESTRICT`。

3.`CASCADE`：级联删除。

```
cascade='all,delete,delete-orphan'
```

4.`SET NULL`：父表数据被删除，子表数据会设置为NULL。

relationship关联时要加上passive\_deletes=True

外键要加上ondelete='CASCADE'

否则sqlalchemy不能联级删除



官方文档

```
from sqlalchemy import Table, Column, Integer, String, ForeignKey
from sqlalchemy.orm import relationship, backref
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

'''关联表删除实验'''


class User(Base):
    __tablename__ = 'users'

    id = Column(Integer, primary_key=True)
    name = Column(String)
    password = Column(String)

    addresses = relationship("Address", 
                            back_populates='user', 
                            cascade="all, delete, delete-orphan")

    def __repr__(self):
       return "<User(name='%s', fullname='%s', password='%s')>" % ( self.name, self.fullname, self.password)


class Address(Base):
    __tablename__ = 'addresses'
    id = Column(Integer, primary_key=True)
    email_address = Column(String, nullable=False)
    user_id = Column(Integer, ForeignKey('users.id'))
    
    user = relationship("User", 
                        back_populates="addresses")

    def __repr__(self):
        return "<Address(email_address='%s')>" % self.email_address


from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker


DB_CONNECT_STRING = 'sqlite://' # 'sqlite:///:memory:'
engine = create_engine(DB_CONNECT_STRING, echo=False)
DB_Session = sessionmaker(bind=engine)
session = DB_Session()

# 1. 创建表
Base.metadata.create_all(engine)

# 2. 插入数据
some_users = [User(id=1, name='张三', password='111111'),
              User(id=2, name='李四', password='222222'),
              User(id=3, name='王五', password='333333'),
              User(id=4, name='赵六', password='444444')]
session.add_all(some_users)

some_addresses = [Address(id=1, email_address='zhang@163.com', user_id=1),
                  Address(id=2, email_address='zhang@qq.com', user_id=1),
                  Address(id=3, email_address='li@163.com', user_id=2),
                  Address(id=4, email_address='wang@163.com', user_id=3),
                  Address(id=5, email_address='zhao@163.com', user_id=4)]
session.add_all(some_addresses)

session.commit()

#关联表删除实验
zhangsan = session.query(User).get(1)

# 删除!!
del zhangsan.addresses[1]

res = session.query(Address).filter(Address.email_address.in_(['zhang@163.com', 'zhang@qq.com'])).count()
print(res) # 结果为 1

# 删除!!
session.delete(zhangsan)

res = session.query(User).filter_by(name='张三').count()
print(res) # 结果为 0

res = session.query(Address).filter(Address.email_address.in_(['zhang@163.com', 'jzhang@qq.com'])).count()
print(res) # 结果为 0


```



