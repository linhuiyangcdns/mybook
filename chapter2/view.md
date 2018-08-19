# 列表分页功能

github搜索django-pure-pagination

```
pip install django-pure-pagination
```

install app中添加:

```
'pure_pagination',
```

可设置参数；

```
PAGINATION_SETTINGS = {
    'PAGE_RANGE_DISPLAYED': 10,
    'MARGIN_PAGES_DISPLAYED': 2,
    'SHOW_FIRST_PAGE_WHEN_INVALID': True,
}
```

![](/assets/8KAAL61KBk.png)

`PAGE_RANGE_DISPLAYED`是总共会显示多少个page。\(包括省略号，包括两边和中间\)

`MARGIN_PAGES_DISPLAYED`是旁边会显示多少个。

`SHOW_FIRST_PAGE_WHEN_INVALID`:当输入页数不合法是否要跳到第一页

官方实例；

```
from django.shortcuts import render_to_response

from pure_pagination import Paginator, EmptyPage, PageNotAnInteger

    # 尝试获取页数参数
    try:
        page = request.GET.get('page', 1)
    except PageNotAnInteger:
        page = 1
    # objects是取到的数据
    objects = ['john', 'edward', 'josh', 'frank']

    # Provide Paginator with the request object for complete querystring generation
    # 对于取到的数据进行分页处理。每页显示5个
    p = Paginator(objects, 5，request=request)
    # 此时前台显示的就应该是我们此时获取的第几页的数据
    people = p.page(page)

    return render_to_response('index.html', {
        'people': people,
    }
```

模板页面

```
         <div class="pageturn">
            <ul class="pagelist">
            {% if all_orgs.has_previous %}
             <li class="long"><a href="?{{ all_orgs.previous_page_number.querystring }}" >上一页</a></li>
                {% endif %}
            {% for page in all_orgs.pages %}
             {% if page %}
            {% ifequal page all_orgs.number %}
             <li class="active"><a href="?{{ page.querystring }}">{{ page }}</a></li>
            {% else %}
       <li><a href="?{{ page.querystring }}">{{ page }}</a></li>
             {% endifequal %}
            {% else %}
            <li class="none"><a href="">...</a></li>
            {% endif %}
        {% endfor %}
        {% if all_orgs.has_next %}
        <li class="long"><a href="?{{ all_orgs.next_page_number.querystring }}">下一页</a></li>
        {% endif %}
            </ul>
        </div>
```

# get\_queryset, get\_context\_data和get\_object





