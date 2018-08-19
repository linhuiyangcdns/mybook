# **Using the Django authentication system使用Django认证系统** {#using-the-django-authentication-system使用django认证系统}

本文档介绍了Django身份验证系统的默认配置。 这种配置已经演变为满足最常见的项目需求，处理相当广泛的任务，并且仔细地实现了密码和权限。 对于认证需求与默认值不同的项目，Django支持大量扩展和定制认证。

Django认证一起提供认证和授权，通常被称为认证系统，因为这些功能有些耦合。

## **LoginRequired mixin** {#loginrequired-mixin}

使用基于类的视图时，可以通过使用`LoginRequiredMixin`来实现与`login_required`相同的行为。 这个mixin应该在继承列表的最左边的位置。

`class LoginRequiredMixin`

如果视图正在使用此mixin，则所有未经身份验证的用户的请求将被重定向到登录页面，或者显示一个`HTTP 403 Forbidden`错误，具体取决于`raise_exception`参数。

您可以设置`AccessMixin`的任何参数来自定义未授权用户的处理：

```
from django.contrib.auth.mixins import LoginRequiredMixin

class MyView(LoginRequiredMixin, View):
    login_url = '/login/'
    redirect_field_name = 'redirect_to'
```



