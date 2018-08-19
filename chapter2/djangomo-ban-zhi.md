## forloop

在每个\`\` {% for %}\`\`循环里有一个称为\`\` forloop\`\` 的模板变量。这个变量有一些提示循环进度信息的属性。

forloop.counter 总是一个表示当前循环的执行次数的整数计数器。 这个计数器是从1开始的，所以在第一次循环时 forloop.counter 将会被设置为1。

```
    {% for item in todo_list %}
        <p>{{ forloop.counter }}: {{ item }}</p>
    {% endfor %}

```

forloop.counter0 类似于 forloop.counter ，但是它是从0计数的。 第一次执行循环时这个变量会被设置为0。

forloop.revcounter 是表示循环中剩余项的整型变量。 在循环初次执行时 forloop.revcounter 将被设置为序列中项的总数。 最后一次循环执行中，这个变量将被置1。

forloop.revcounter0 类似于 forloop.revcounter ，但它以0做为结束索引。在第一次执行循环时，该变量会被置为序列的项的个数减1。forloop.first 是一个布尔值，如果该迭代是第一次执行，那么它被置为\`\`\`\` 在下面的情形中这个变量是很有用的：

```
Inline literal start-string without end-string.
    {% for object in objects %}
        {% if forloop.first %}<li class="first">{% else %}<li>{% endif %}
        {{ object }}
        </li>
    {% endfor %}
```

forloop.last 是一个布尔值；在最后一次执行循环时被置为True。 一个常见的用法是在一系列的链接之间放置管道符（\|）

```
    {% for link in links %}{{ link }}{% if not forloop.last %} | {% endif %}{% endfor %}
```

    上面的模板可能会产生如下的结果：

```
    Link1 | Link2 | Link3 | Link4
```

    另一个常见的用途是为列表的每个单词的加上逗号。

```
    Favorite places:
    {% for p in places %}{{ p }}{% if not forloop.last %}, {% endif %}{% endfor %}
```

forloop.parentloop 是一个指向当前循环的上一级循环的 forloop 对象的引用（在嵌套循环的情况下）。 例子在此：

```
    {% for country in countries %}
        <table>
        {% for city in country.city_list %}
        <tr>
        <td>Country #{{ forloop.parentloop.counter }}</td>
        <td>City #{{ forloop.counter }}</td>
        <td>{{ city }}</td>
        </tr>
        {% endfor %}
        </table>
    {% endfor %}
```

forloop 变量仅仅能够在循环中使用。 在模板解析器碰到{% endfor %}标签后，forloop就不可访问了。

Context和forloop变量

在一个 {% for %} 块中，已存在的变量会被移除，以避免 forloop 变量被覆盖。 Django会把这个变量移动到 forloop.parentloop 中。通常我们不用担心这个问题，但是一旦我们在模板中定义了 forloop 这个变量（当然我们反对这样做），在 {% for %} 块中它会在 forloop.parentloop 被重新命名。

# 用户收藏

        \# 向前端传值说明用户是否收藏

        has\_fav = False

        \# 必须是用户已登录我们才需要判断。

```
      from operation.models import UserFavorite
      # 向前端传值说明用户是否收藏
        has_fav_teacher = False
        # 必须是用户已登录我们才需要判断。
        if request.user.is_authenticated:
            if UserFavorite.objects.filter(user=request.user, fav_id=course_org.id, fav_type=2):
                has_fav_teacher  = True
```

在模板里添加：

```
{% if has_fav_teacher %} 已收藏{% else %}收藏{% endif %}
```

# 关闭Django模板的自动转义
Django的模板中会对HTML标签和JS等语法标签进行自动转义，原因显而易见，这样是为了安全。但是有的时候我们可能不希望这些HTML元素被转义，比如我们做一个内容管理系统，后台添加的文章中是经过修饰的，这些修饰可能是通过一个类似于FCKeditor编辑加注了HTML修饰符的文本，如果自动转义的话显示的就是保护HTML标签的源文件。为了在Django中关闭HTML的自动转义有两种方式，如果是一个单独的变量我们可以通过过滤器“|safe”的方式告诉Django这段代码是安全的不必转义。比如：

  ` <p>这行代表会被自动转义</p>: {{ data }}`


```
<p>这行代表不会被自动转义</p>: {{ data|safe }}
```


其中第二行我们关闭了Django的自动转义。
我们还可以通过{%autoescape off%}的方式关闭整段代码的自动转义，比如下面这样：



```
{% autoescape off %}
    Hello {{ name }}
{% endautoescape %}
```
# 推荐


```
        # 取出标签找到标签相同的course
        tag = course.tag
        if tag:
            # 从1开始否则会推荐自己
            relate_courses = Course.objects.filter(tag=tag)[1:2]
        else:
            relate_courses = []
```




