# 发送到邮箱注册

### 注册邮箱号码

### 在setting.py设置相关参数

在settings.py加上如下代码：

```
    EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'

    EMAIL_USE_TLS = False
    EMAIL_HOST = 'smtp.163.com'
    EMAIL_PORT = 25
    EMAIL_HOST_USER = '邮箱账户'
    EMAIL_HOST_PASSWORD = '设置的密码'（有些是授权码）
    DEFAULT_FROM_EMAIL = '邮箱账户'
```





