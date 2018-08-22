# flask flash消息闪现



```
{% for message in get_flashed_messages() %} 
 {{ message }} 
{% endfor %}

```

# 多个flash如何确定是否需要的消息

view

```
flash('添加成功','ok')
```

templates

```
{% for msg in get_flashed_messages(category_filter=['ok']): %}
    {{msg}}
{% endfor %}    
```

### category\_filter {#为什么要用categoryfilter}

```
可能一个视图flash中有很多msg需要处理，如何区分这次到底需要哪个闪现消息？就要添加参数category_filter=[‘ok’]加以区分。
```



