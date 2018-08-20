# Flask框架操作静态文件

```
<img src ="/static/img/a.png">
//或者
<img src = "{{ url_for('static', filename = 'img/a.png') }}>
```

启动：

python manage.py runserver -h 0.0.0.0 -p 5000



