# 通过get\_FOO\_display 查找模型中的choice值

```
class Course(models.Model):
    DEGREE_CHOICES = (
        ('cj',u'初级'),
        ('zj', u'中级'),
        ('gj', u'高级')
    )
    degree = models.CharField(max_length=2,choices=DEGREE_CHOICES)
```

如果我们想要在HTML中显示choice中的字段值，采用`{{ obj.level }}`得到的只是数字

cj,zj,gj，如果我们想要的到文字’、等字段，需要采用`get_FOO_display`方法：

