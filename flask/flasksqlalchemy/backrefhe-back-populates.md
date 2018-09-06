# SQLAlchemy的backref和back\_populates区别

backref和back\_populates在表示两个表之间的关系

查看[backref](http://docs.sqlalchemy.org/en/latest/orm/backref.html#relationships-backref)的文档

```
from sqlalchemy import Integer, ForeignKey, String, Column
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import relationship

Base = declarative_base()

class User(Base):
    __tablename__ = 'user'
    id = Column(Integer, primary_key=True)
    name = Column(String)

    addresses = relationship("Address", backref="user")
    
    def __repr__(self):
        return u'<user id={0}, name={1}>'.format(self.id, self.name).encode('utf-8')

class Address(Base):
    __tablename__ = 'address'
    id = Column(Integer, primary_key=True)
    email = Column(String)
    user_id = Column(Integer, ForeignKey('user.id'))
    
    def __repr__(self):
        return '<address id={0}, email={1} user_id={2}>'.format(self.id, self.email, self.user_id)

```

之后在命令行中，可以得到如下结果

```
>>> u = User(name='Bob')
>>> db.session.add(u)
>>> db.session.commit()
>>> u.addresses
[]
>>> a = Address(email='bob@163.com', user_id=u.id)
>>> db.session.add(a)
>>> db.session.commit()
>>> u.addresses
[<address id=2, email=bob@163.com user_id=2>]
>>> a.user
<user id=2, name=Bob>
```

即通过u.addresses可以访问到用户的addresses, 而a.user可以访问到用户。注意到u.addresses返回的是列表，而a.user返回的是单个元素，即User与Address是一对多的关系。

也可以使用[back\_populates](http://docs.sqlalchemy.org/en/latest/orm/tutorial.html#building-a-relationship)实现相同的功能，

```
class User(db.Model):
    __tablename__ = 'users'
    id = db.Column(Integer, primary_key=True)
    name = db.Column(String(10))
    addresses = relationship("Address", back_populates = "user")

    def __repr__(self):
        return u'<user id={0}, name={1}>'.format(self.id, self.name).encode('utf-8')


class Address(db.Model):
    __tablename__ = 'addresses'
    id = db.Column(Integer, primary_key=True)
    email = db.Column(String(64))
    user_id = db.Column(Integer, ForeignKey('users.id'))
    user = relationship("User", back_populates="addresses")

    def __repr__(self):
        return '<address id={0}, email={1} user_id={2}>'.format(self.id, self.email, self.user_id)

```

在交互环境中，得到如下结果

```
>>> u = User(name='Clack')
>>> db.session.add(u)
>>> db.session.commit()
>>> u.addresses
[]
>>> a = Address(emial='Clack@163.com', user_id=u.id)
>>> a = Address(email='Clack@163.com', user_id=u.id)
>>> db.session.add(a)
>>> db.session.commit()
>>> u.addresses
[<address id=3, email=Clack@163.com user_id=3>]
>>> a.user
<user id=3, name=Clack>
```

从文档中得知，back\_populates是用来取代backref的，虽然backref也是一直支持使用。倾向于使用back\_populates, 因为它比backref更直接明了。

