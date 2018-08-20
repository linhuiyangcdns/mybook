# 程序经常需要配置多个配置，如开发，测试和生产环境使用不同的数据库。不在使用hello.py的简单的**字典状结构配置**，而使用**层次结构的配置类**。

**flasky/config.py**：程序的配置

```
import os
basedir = os.path.abspath(os.path.dirname(__file__))
###得到程序根目录的位置（去掉最底层文件名）
###基类Config包含通用配置，子类分别定义专用的配置。
class Config:
    SECRET_KEY = os.environ.get('SECRET_KEY') or 'hard to guess string'
    ###某些配置如敏感信息可以从环境变量中导入，系统也可提供一个默认值，以防环境中没有定义。
    SQLALCHEMY_COMMIT_ON_TEARDOWN = True
    ###配置对象中还有一个很有用的选项，即SQLALCHEMY_COMMIT_ON_TEARDOWN键，将其设为True时，每次请求结束后都会自动提交数据库中的变动。
    FLASKY_MAIL_SUBJECT_PREFIX = '[Flasky]'
    ###flask_mail的主题前缀设置
    FLASKY_MAIL_SENDER = ''
    ###flask_mail的发送者邮件设置
    FLASKY_ADMIN = os.environ.get('FLASKY_ADMIN')
    ###从环境中设置管理员的邮件地址，用于接收通知新用户注册邮件，这里面的发送者sender，接受者ADMIN都是自己。

    @staticmethod
    ###一般来说，要使用某个类的方法，需要先实例化一个对象再调用方法。 
    ###而使用'@staticmethod'或'@classmethod'，就可以不需要实例化，直接类名.方法名()来调用。
    ###详细请见另一个博客。
    def init_app(app):
        pass
    ###配置类可以定义init_app()方法类，其参数是程序实例。
    ###在这个方法中。可以执行对当前环境的配置初始化。
    ###基类Config中的init_app()方法为空。
class DevelopmentConfig(Config):
    DEBUG = True
    ###调试模式开启，不懂什么意思。
    MAIL_SERVER = 'smtp.qq.com'
    MAIL_PORT = 25
    MAIL_USE_TLS = True
    MAIL_USERNAME = os.environ.get('MAIL_USERNAME')
    MAIL_PASSWORD = os.environ.get('MAIL_PASSWORD')
    SQLALCHEMY_DATABASE_URI = os.environ.get('DEV_DATABASE_URL') or \
        'sqlite:///' + os.path.join(basedir, 'data-dev.sqlite')

class TestingConfig(Config):
    TESTING = True
    SQLALCHEMY_DATABASE_URI = os.environ.get('TEST_DATABASE_URL') or \
        'sqlite:///' + os.path.join(basedir, 'data_test.sqlite')

class ProductionConfig(Config):
    SQLALCHEMY_DATABASE_URI = os.environ.get('DATABASE_URL') or \
        'sqlite:///' + os.environ.get(basedir, 'data.sqlite')

config = {
    'development': DevelopmentConfig
    'testing': TestingConfig
    'production': ProductionConfig

    'default': DevelopmentConfig
}
###在这个配置脚本末尾，config字典中注册了不同的配置环境，而且还注册了一个默认配置。
```

```
import os 
basedir = os.path.abspath(os.path.dirname(__file__)) 

class Config: 
    SECRET_KEY = os.environ.get('SECRET_KEY') or 'hard to guess string' 
    SQLALCHEMY_COMMIT_ON_TEARDOWN = True 
    FLASKY_MAIL_SUBJECT_PREFIX = '[Flasky]' 
    FLASKY_MAIL_SENDER = 'Flasky Admin <flasky@example.com>' 
    FLASKY_ADMIN = os.environ.get('FLASKY_ADMIN') 

    @staticmethod 
    def init_app(app): 
        pass 

class DevelopmentConfig(Config): 
    DEBUG = True 
    MAIL_SERVER = 'smtp.googlemail.com'
    MAIL_PORT = 587 
    MAIL_USE_TLS = True 
    MAIL_USERNAME = os.environ.get('MAIL_USERNAME') 
    MAIL_PASSWORD = os.environ.get('MAIL_PASSWORD') 
    SQLALCHEMY_DATABASE_URI = os.environ.get('DEV_DATABASE_URL') or \ 
        'sqlite:///' + os.path.join(basedir, 'data-dev.sqlite') 

class TestingConfig(Config): 
    TESTING = True 
    SQLALCHEMY_DATABASE_URI = os.environ.get('TEST_DATABASE_URL') or \ 
        'sqlite:///' + os.path.join(basedir, 'data-test.sqlite') 

class ProductionConfig(Config): 
    SQLALCHEMY_DATABASE_URI = os.environ.get('DATABASE_URL') or \ 
        'sqlite:///' + os.path.join(basedir, 'data.sqlite') 

config = { 
    'development': DevelopmentConfig, 
    'testing': TestingConfig, 
    'production': ProductionConfig, 

    'default': DevelopmentConfig 
}
```

app/\_\__init\_\__

```
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from config import config
from flask_bootstrap import Bootstrap
from flask_mail import Mail

import pymysql
pymysql.install_as_MySQLdb()

bootstrap = Bootstrap()
db = SQLAlchemy()
email = Mail()




def create_app(config_name):
    app = Flask(__name__)
    app.config.from_object(config[config_name])
    config[config_name].init_app(app)

    db.init_app(app)
    bootstrap.init_app(app)
    email.init_app(app)

    from .operation import operation as operation_blueprint
    app.register_blueprint(operation_blueprint)


    return app
```

manage.py

```
import os
from app import create_app, db
from flask_script import Manager, Shell
from flask_migrate import Migrate, MigrateCommand





app = create_app(os.getenv('FLASK_CONFIG') or 'default')
manager = Manager(app)
migrate = Migrate(app, db)


def make_shell_context():
    return dict(app=app, db=db,)


manager.add_command("shell",Shell(make_context=make_shell_context))
manager.add_command('db', MigrateCommand)

if __name__ == '__main__':

    manager.run()
```



