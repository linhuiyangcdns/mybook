1、



/\*\*

 \* 获取模版文件 格式 资源://模块@主题/控制器/操作

 \* @param string $template 模版资源地址

 \* @param string $layer 视图层（目录）名称

 \* @return string

 \*/

T\($template='',$layer=''\)

2、

/\*\*

 \* 获取输入参数 支持过滤和默认值

 \* 使用方法:

 \* &lt;code&gt;

 \* I\('id',0\); 获取id参数 自动判断get或者post

 \* I\('post.name','','htmlspecialchars'\); 获取$\_POST\['name'\]

 \* I\('get.'\); 获取$\_GET

 \* &lt;/code&gt;

 \* @param string $name 变量的名称 支持指定类型

 \* @param mixed $default 不存在的时候默认值

 \* @param mixed $filter 参数过滤方法

 \* @param mixed $datas 要获取的额外数据源

 \* @return mixed

 \*/

I\($name,$default='',$filter=null,$datas=null\)



3、

/\*\*

 \* 设置和获取统计数据

 \* 使用方法:

 \* &lt;code&gt;

 \* N\('db',1\); // 记录数据库操作次数

 \* N\('read',1\); // 记录读取次数

 \* echo N\('db'\); // 获取当前页面数据库的所有操作次数

 \* echo N\('read'\); // 获取当前页面读取次数

 \* &lt;/code&gt;

 \* @param string $key 标识位置

 \* @param integer $step 步进值

 \* @param boolean $save 是否保存结果

 \* @return mixed

 \*/

N\($key, $step=0,$save=false\) 



4、

/\*\*

 \* 实例化模型类 格式 \[资源://\]\[模块/\]模型

 \* @param string $name 资源地址

 \* @param string $layer 模型层名称

 \* @return Think\Model

 \*/

D\($name='',$layer=''\)



5、

/\*\*

 \* 实例化一个没有模型文件的Model

 \* @param string $name Model名称 支持指定基础模型 例如 MongoModel:User

 \* @param string $tablePrefix 表前缀

 \* @param mixed $connection 数据库连接信息

 \* @return Think\Model

 \*/



M\($name='', $tablePrefix='',$connection=''\)



6、

/\*\*

 \* 实例化多层控制器 格式：\[资源://\]\[模块/\]控制器

 \* @param string $name 资源地址

 \* @param string $layer 控制层名称

 \* @param integer $level 控制器层次

 \* @return Think\Controller\|false

 \*/



A\($name,$layer='',$level=0\)



7、



/\*\*

 \* 远程调用控制器的操作方法 URL 参数格式 \[资源://\]\[模块/\]控制器/操作

 \* @param string $url 调用地址

 \* @param string\|array $vars 调用参数 支持字符串和数组

 \* @param string $layer 要调用的控制层名称

 \* @return mixed

 \*/

R\($url,$vars=array\(\),$layer=''\)



8、



/\*\*

 \* URL组装 支持不同URL模式

 \* @param string $url URL表达式，格式：'\[模块/控制器/操作\#锚点@域名\]?参数1=值1&参数2=值2...'

 \* @param string\|array $vars 传入的参数，支持数组和字符串

 \* @param string\|boolean $suffix 伪静态后缀，默认为true表示获取配置值

 \* @param boolean $domain 是否显示域名

 \* @return string

 \*/

 U\($url='',$vars='',$suffix=true,$domain=false\)



