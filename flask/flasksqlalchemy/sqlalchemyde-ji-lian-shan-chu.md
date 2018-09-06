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

