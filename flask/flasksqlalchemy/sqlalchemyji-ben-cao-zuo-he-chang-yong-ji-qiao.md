# relationship之lazy属性

```

class Student(db.Model):
    __tablename__ = 'students'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64))
    class_id = db.Column(db.Integer, db.ForeignKey('classes.id'))
    def __repr__(self):
        return '<Student: %r>' %self.name
 
class Class(db.Model):
    __tablename__ = 'classes'
    id = db.Column(db.Integer, primary_key=True)
    students = db.relationship('Student', backref='_class', lazy="select")
    name = db.Column(db.String(64))
    def __repr__(self):
        return '<Class: %r>' %self.name
```

Class里面的students是relationship，里面的lazy我选择了select，也就是默认值

手动插入数据进数据库，注意Allen和Tina选择的是同一门课，class1.

import sqlite3

 

conn = sqlite3.connect\("e:\\flasky1\data-dev.sqlite"\)

 

cur = conn.cursor\(\)

 

cur.execute\("insert into students values\(1,'Allen',1\)"\)

cur.execute\("insert into students values\(2,'Bob',2\)"\)

cur.execute\("insert into students values\(3,'Tina',1\)"\)

 

cur.execute\("insert into classes values\(1,'class1'\)"\)

cur.execute\("insert into classes values\(2,'class2'\)"\)

 

cur.close\(\)

conn.commit\(\)

conn.close\(\)



c1代表第一门课程，class1，然后通过他的students属性，可以看到有多少学生选了这门课

可以看到，当Class的lazy属性是select的时候，他就是直接查找出了对象，并由对象为元素组成了一个list

```
s1=Student.query.first()
>>c1=Class.query.first()
>>c1.students
[<Stuent:'Allen'>,<Student:'Tina'>]
```

然后，用s1这个实例对象，通过Class里面的backref=\_class属性，也能访问Class内容，因为backref=\_class默认也是select属性，所以，他也是直接导出结果

```
>>s1 = Student.query.first()
>>s1._class
<Class:'class1'>
>>>>
```

改成dynamic

```
students = db.relationship('Student', backref='_class', lazy="dynamic")
```

可以看到，c1.students变成了一个查询对象，而不是直接生成列表，你可以在他上面进行操作，比如filter等

**lazy="dynamic"只可以用在一对多和多对多关系中，不可以用在一对一和多对一中**

