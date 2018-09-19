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

`import sqlite3`

`conn = sqlite3.connect("e:\flasky1\data-dev.sqlite")`

`cur = conn.cursor()`

`cur.execute("insert into students values(1,'Allen',1)")`

`cur.execute("insert into students values(2,'Bob',2)")`

`cur.execute("insert into students values(3,'Tina',1)")`

`cur.execute("insert into classes values(1,'class1')")`

`cur.execute("insert into classes values(2,'class2')")`

`cur.close()`

`conn.commit()`

`conn.close()`

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

![](/assets/1)

前面所用到的backref属性，由于是一对多的关系，所以用的是默认的select，如果想要为backref改成dynamic属性呢？

首先我们要把模型改成多对多关系，来进行测试

Class里面secondary这个属性，就是代表使用了关联表，我们将backref反向引用的属性改成了dynamic.

```
registrations = db.Table('registrations',
                         db.Column('student_id', db.Integer, db.ForeignKey('students.id')),
                         db.Column('class_id', db.Integer, db.ForeignKey('classes.id')))
 
class Student(db.Model):
    __tablename__ = 'students'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64))
    # class_id = db.Column(db.Integer, db.ForeignKey('classes.id')) 这里需要注释，不需要外键了
    def __repr__(self):
        return '<Student: %r>' %self.name
 
class Class(db.Model):
    __tablename__ = 'classes'
    id = db.Column(db.Integer, primary_key=True)
    students = db.relationship('Student', secondary=registrations, backref=db.backref('_class',lazy="dynamic"), lazy="dynamic")#这里指定关联表
    name = db.Column(db.String(64))
    def __repr__(self):
        return '<Class: %r>' %self.name

```

s1.\_class就不是直接生成查询结果了，而是一个查询对象

![](/assets/2)

![](/assets/3)最后，我要把backref的lazy属性改成**joined**了，这个joined属性我看下来非常之奇妙，因为他**不是直接改变**\_class这个backref生成的结果，他反而是改变了这个relationship的属性的结果\(也就是**students**属性\)

先来2个比较图

![](/assets/4)

![](/assets/5)通过上面2个对比图我们可以看到，当backref="\_class" 时和backref=db.backref\("\_class",lazy="joined"\)时，

s1.\_class生成的结果和type类型，都是一样的！

而c1.students虽然type结果一样，是一个dynamic查询结果

**但是！！！你打印c1.students，就会发现不同**

backref="\_class"时：他只是从students和registrations里面进行SELECT和FROM

而backref=db.backref\("\_class",lazy="joined"\)时，他还同时在classes里面进行了SELECT，因为，他在后面进行了JOIN操作！！

根据下图，得出的结果应该是

![](/assets/6)  
**关于什么是JOIN（联结操作）**

这里有2个关于联结的不错的文章，可以参考

http://blog.jobbole.com/40443/

http://www.cnblogs.com/albert1017/archive/2012/07/27/2611898.html

----------------------------------------------------------------------------------

**这里插播下SQL 语句测试**

-------------------------------------------------------------

![](/assets/7)我用JOIN联结操作进行了测试，使用了相同的顺序FROM----&gt;JOIN，都是用registrations 去JOIN students

但是在设置ON条件的时候，虽然等式两边一样，但是我换了个顺序，想看看生成的表格的形式和FROM---&gt;JOIN有关系还是和ON的顺序有关系

事实证明是和FROM--------&gt;JOIN的顺序有关系

--------------------------------------------------------------------------------------插播结束------------------------------------------------------------------------

```
class Registration(db.Model):
    '''关联表'''
    __tablename__ = 'registrations'
    student_id = db.Column(db.Integer, db.ForeignKey('students.id'), primary_key=True)
    class_id = db.Column(db.Integer, db.ForeignKey('classes.id'), primary_key=True)
    create_at = db.Column(db.DateTime, default=datetime.utcnow)
 
class Student(db.Model):
    __tablename__ = 'students'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(64))
    _class = db.relationship('Registration', foreign_keys=[Registration.student_id],
                             backref=db.backref('student', lazy="joined"), lazy="dynamic")
    def __repr__(self):
        return '<Student: %r>' %self.name
        
class Class(db.Model):
    __tablename__ = 'classes'
    id = db.Column(db.Integer, primary_key=True)
    students = db.relationship('Registration', foreign_keys=[Registration.class_id],
                               backref=db.backref('_class', lazy="joined"), lazy="dynamic")
    name = db.Column(db.String(64))
    def __repr__(self):
        return '<Class: %r>' %self.name

```

这里将Registrations加入models，看别人文章说如果不加入models，程序无法访问，registrations表示在sqlalchemy控制下的，搞不明白什么意思，回头再说。

![](/assets/8)加入模型后，他就返回的是Registrations对象了，而不是Student或者Class对象了！！！

**这里在插一句，Python2的时候，map函数直接返回list对象，而在Python3里面，他是返回map object，你需要在前面加一个list函数，才能显示出来**

```
>>> s1=Student.query.first()
>>> c1=Class.query.first()
>>> s1._class.all()
[<app.models.Registration object at 0x00000000046BC9E8>, <app.models.Registration object at 0x00000000046BC860>]
>>> c1.students.all()
[<app.models.Registration object at 0x00000000046BC9E8>, <app.models.Registration object at 0x00000000046D0940>, 
<app.models.Registration object at 0x00000000046D09B0>]
>>> map(lambda x: x._class,s1._class.all())
<map object at 0x00000000046D0518>
>>> list(map(lambda x: x._class,s1._class.all()))
[<Class: 'class1'>, <Class: 'class2'>]

```

这里就要说到重点了，当你设置为joined的时候，他在查询的时候已经把Class和Student一并查询过了

在From里面就可以看到，他和students和classes表都发生了JOIN操作

```
>>> print (s1._class)
SELECT registrations.student_id AS registrations_student_id, registrations.class_id AS registrations_class_id, 
registrations.create_at AS registrations_create_at, students_1.id AS students_1_id, 
students_1.name AS students_1_name, classes_1.id AS classes_1_id, classes_1.name AS classes_1_name
 
FROM registrations LEFT OUTER JOIN students AS students_1 ON students_1.id = registrations.student_id 
LEFT OUTER JOIN classes AS classes_1 ON classes_1.id = registrations.class_id
 
WHERE :param_1 = registrations.student_id

```

而如果我把lazy改成select

结果只是查询了registrations表，在你要真的查询结果的时候，他还需要去查询一遍classes和students

```
>>> s1=S.query.first()
>>> print (s1._class)
SELECT registrations.student_id AS registrations_student_id, registrations.class_id AS registrations_class_id,
registrations.create_at AS registrations_create_at
 
FROM registrations
WHERE :param_1 = registrations.student_id
 
>>> map(lambda x : x._class , s1._class)
[<Class: u'class1'>, <Class: u'class2'>]

```

虽然结果一样，但是，他在查询的时候，会要额外去查一次classes和students表，对于数量庞大了以后的情况，这个相当不利！

这个，就是joined参数厉害的地方！

