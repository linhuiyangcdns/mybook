# Flask框架操作静态文件

```
<img src ="/static/img/a.png">
//或者
<img src = "{{ url_for('static', filename = 'img/a.png') }}>
```

启动：

python manage.py runserver -h 0.0.0.0 -p 5000

### CSRF保护 {#0x02-开启csrf保护}

启动CSRF保护，可以在config.py中定义变量：

```
SECRET_KEY = os.environ.get('SECRET_KEY') or 'hard to guess string'

```

SECRET\_KEY用来建立加密的令牌，用于验证Form表单提交，在自己编写应用程序时，可以尽可能设置复杂一些，这样恶意攻击者将很难猜到密钥值。在`__init__.py`文件中添加如下代码：

```
app.config.from_object(config[config_name])
或 app.config.from_object('config')
```

最后，我们需要在响应的html模板的Form表单中加上如下语句：

```
{{form.csrf_token}}
或 {{form.hidden_tag()}}
```



