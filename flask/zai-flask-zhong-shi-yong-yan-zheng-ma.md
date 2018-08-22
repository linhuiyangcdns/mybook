# 在flask中使用验证码

Flask博客的登录页面加个验证码。使用了PIL模块生成验证码图片，并通过Flask的session机制，进行验证码验证

## **1、生成验证码**

使用string模块：string.ascii\_letters+string.digits构造了验证码字符组合。使用的PIL模块，构建了图形对象，并进行划线和高斯模糊处理。字体文件可单独保存到工程里。绘制字符串时，draw.text的前两个参数为字符的位置，可以设置为随机数，使验证码各字符的位置不固定，并且相邻字符略有重合。get\_verify\_code返回了图形对象和字符串。

```
import random
import string
from PIL import Image, ImageFont, ImageDraw, ImageFilter


def rndColor():
    '''随机颜色'''
    return (random.randint(32, 127), random.randint(32, 127), random.randint(32, 127))

def gene_text():
    '''生成4位验证码'''
    return ''.join(random.sample(string.ascii_letters+string.digits, 4))

def draw_lines(draw, num, width, height):
    '''划线'''
    for num in range(num):
        x1 = random.randint(0, width / 2)
        y1 = random.randint(0, height / 2)
        x2 = random.randint(0, width)
        y2 = random.randint(height / 2, height)
        draw.line(((x1, y1), (x2, y2)), fill='black', width=1)

def get_verify_code():
    '''生成验证码图形'''
    code = gene_text()
    # 图片大小120×50
    width, height = 120, 50
    # 新图片对象
    im = Image.new('RGB',(width, height),'white')
    # 字体
    font = ImageFont.truetype('app/static/arial.ttf', 40)
    # draw对象
    draw = ImageDraw.Draw(im)
    # 绘制字符串
    for item in range(4):
        draw.text((5+random.randint(-3,3)+23*item, 5+random.randint(-3,3)),
                  text=code[item], fill=rndColor(),font=font )
    # 划线
    draw_lines(draw, 2, width, height)
    # 高斯模糊
    im = im.filter(ImageFilter.GaussianBlur(radius=1.5))
    return im, code
```

## **2、表单类**

为LoginForm增加一个verify\_code字段，用来输入验证码。

```
class LoginForm(FlaskForm):
    email = StringField('Email', validators=[DataRequired(), Length(1, 64),
                                             Email()],)
    password = PasswordField('Password', validators=[DataRequired()])
    verify_code = StringField('VerifyCode', validators=[DataRequired()])
    remember_me = BooleanField('Keep me logged in')
    submit = SubmitField('Log In')
```

## **3、视图函数 **

使用io.BytesIO对象将验证码图片转化为二进制形式，直接作为响应返回前端。设置首部字段的内容格式，这样二进制的内容就能以图形形式在页面中显示。验证码字符串保存在flask.session对象中，对session的操作就像处理字典一样。程序内部使用设置中的SECRET\_KEY对session数据加密后，存储在cookie中。

```
from io import BytesIO
@auth.route('/code')
def get_code():
    image, code = get_verify_code()
    # 图片以二进制形式写入
    buf = BytesIO()
    image.save(buf, 'jpeg')
    buf_str = buf.getvalue()
    # 把buf_str作为response返回前端，并设置首部字段
    response = make_response(buf_str)
    response.headers['Content-Type'] = 'image/gif'
    # 将验证码字符串储存在session中
    session['image'] = code
    return response
```

## 4.登陆View

```
# 登陆
@auth.route('/login', methods=["POST", "GET"])
def login():
    form = LoginForm()
    if form.validate_on_submit():
        user = User.query.filter_by(email=form.email.data).first()
        if session.get('image') != form.verify_code.data:
            flash('验证码错误','yzm')
        if user is not None and user.verify_password(form.password.data):
            login_user(user, form.remember_me.data)
            return redirect(request.args.get('next') or url_for('operation.index'))
        else:
            flash('用户名或者密码不正确','zhmm')
            return redirect(url_for('auth.login'))
    return render_template('auth/login.html', form=form)
```

## 5.html

```
                    <div class="form-group marb8  captcha1 ">
                            <label>验&nbsp;证&nbsp;码</label>
                        {{ form.verify_code }}
                        #{% for msg in get_flashed_messages(category_filter=['yzm']) %}
                        #{{ msg }}
                        #{% endfor %}
                         <!--点击图片重新获取验证码-->
                   <img width="70" height="36" src="{{ url_for('auth.get_code') }}" onclick="this.src='{{ url_for('auth.get_code') }}?'+ Math.random()">
                   </div>
```



