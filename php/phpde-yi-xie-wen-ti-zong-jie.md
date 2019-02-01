# php中 -&gt; 和 =&gt; 和 ：： 的用法 以及 self 和 $this 的用法

=

```
>


数组中 用于数组的 key 和 value之间的关系


例如：


$a = array(


'0' =
>
'1',


'2' =
>
'4',


);




echo 
$a[
'0'];


echo 
$a[
'2'];






-
>


类中 用于引用类实例的方法和属性


例如：


class Test{


function 
add(){
return 
$this-
>
var++;}


    var 
$var = 0;


}




$a = new Test; //实例化对象名称


echo 
$a-
>
add();


echo 
$a-
>
var;






：：


类中 静态方法和静态属性的引用方法


例如


class Test{


    public static 
function 
test(){


    public static 
$test = 1;


   }


}




类的静态方法和静态属性可以不用实例化对象直接使用（使用的方式是 类名::静态方法名 ）




Test::
test(); 调用静态方法
test


Test::
$test;  来取得
$test静态属性的值




注：


静态方法在读到这个类或者引入这个类文件的时候，就已经实例化并存放到内存中了，非静态类则需要new一下。


静态类在内存中即使有多个实例，静态的属性也只有一份。






==== selef=== 
$this ======




self是引用静态类的类名，而
$this是引用非静态类的实例名




static 的属性和方法，只能访问static的属性和方法，不能类访问非静态的属性和方法。


因为静态属性和方法被创建时，可能还没有任何这个类的实例可以被调用。




static的属性，在内存中只有一份，为所有的实例共用。




使用self:: 关键字访问当前类的静态成员。




一个类的所有实例，共用类中的静态属性。




也就是说，在内存中即使有多个实例，静态的属性也只有一份。




下面例子中的设置了一个计数器
$count属性，设置private 和 static 修饰。


这样，外界并不能直接访问
$count属性。而程序运行的结果我们也看到多个实例在使用同一个静态的
$count 属性。


<
?php     


 class user   


 {     


    private static 
$count = 0 ; //记录所有用户的登录情况.     


    public 
function 
__construct() {     


        self::
$count = self::
$count + 1;     


    }     


    public 
function 
getCount() {       


return self::
$count;     


    }     


    public 
function 
__destruct() {     


        self::
$count = self::
$count - 1;     


    }     


 }     


$user1 = new user();     


$user2 = new user();     


$user3 = new user();     


echo 
"now here have " . 
$user1-
>
getCount() . 
" user";     


echo 
"
<
br /
>
";     


unset(
$user3);     


echo 
"now here have " . 
$user1-
>
getCount() . 
" user";     


 ?
>




静态属性直接调用


静态属性不需要实例化就可以直接使用，在类还没有创建时就可以直接使用。




使用的方式是： 类名::静态属性名




<
?php     


 class Math   


 {     


    public static => 
数组中 用于数组的 key 和 value之间的关系
例如：
$a = array(
  '0' => '1',
  '2' => '4',
);

echo $a['0'];
echo $a['2'];


-> 
类中 用于引用类实例的方法和属性
例如：
class Test{
    function add(){return $this->var++;}
    var $var = 0;
}

$a = new Test; //实例化对象名称
echo $a->add();
echo $a->var;


：：
类中 静态方法和静态属性的引用方法
例如
class Test{
    public static function test(){
    public static $test = 1;
   }
}

类的静态方法和静态属性可以不用实例化对象直接使用（使用的方式是 类名::静态方法名 ）

Test::test(); 调用静态方法test
Test::$test;  来取得$test静态属性的值

注：
静态方法在读到这个类或者引入这个类文件的时候，就已经实例化并存放到内存中了，非静态类则需要new一下。
静态类在内存中即使有多个实例，静态的属性也只有一份。


==== selef=== $this ======

self是引用静态类的类名，而$this是引用非静态类的实例名

static 的属性和方法，只能访问static的属性和方法，不能类访问非静态的属性和方法。
因为静态属性和方法被创建时，可能还没有任何这个类的实例可以被调用。

static的属性，在内存中只有一份，为所有的实例共用。

使用self:: 关键字访问当前类的静态成员。

一个类的所有实例，共用类中的静态属性。

也就是说，在内存中即使有多个实例，静态的属性也只有一份。

下面例子中的设置了一个计数器$count属性，设置private 和 static 修饰。
这样，外界并不能直接访问$count属性。而程序运行的结果我们也看到多个实例在使用同一个静态的$count 属性。
 <?php     
 class user   
 {     
    private static $count = 0 ; //记录所有用户的登录情况.     
    public function __construct() {     
        self::$count = self::$count + 1;     
    }     
    public function getCount() {       
        return self::$count;     
    }     
    public function __destruct() {     
        self::$count = self::$count - 1;     
    }     
 }     
 $user1 = new user();     
 $user2 = new user();     
 $user3 = new user();     
 echo "now here have " . $user1->getCount() . " user";     
 echo "<br />";     
 unset($user3);     
 echo "now here have " . $user1->getCount() . " user";     
 ?>    

静态属性直接调用
静态属性不需要实例化就可以直接使用，在类还没有创建时就可以直接使用。

使用的方式是： 类名::静态属性名

 <?php     
 class Math   
 {     
    public static $pi = 3.14;     
 }     
 // 求一个半径3的园的面积。     
 $r = 3;     
 echo "半径是 $r 的面积是<br />";     
 echo Math::$pi * $r * $r;     
 echo "<br /><br />";     
 //这里我觉得 3.14 不够精确，我把它设置的更精确。     
 Math::$pi = 3.141592653589793;     
 echo "半径是 $r 的面积是<br />";     
 echo Math::$pi * $r * $r;      
 ?>    


类没有创建，静态属性就可以直接使用。那静态属性在什么时候在内存中被创建？ 在PHP中没有看到相关的资料。
引用Java中的概念，来解释应该也具有通用性。静态属性和方法，在类被调用时创建。

静态方法
静态方法不需要所在类被实例化就可以直接使用。

使用的方式是 类名::静态方法名

下面我们继续写这个Math类，用来进行数学计算。我们设计一个方法用来算出其中的最大值。
既然是数学运算，如果这个方法可以拿过来就用就方便多了。
我们这只是为了演示static方法设计的这个类。在PHP提供了max()函数比较数值。

 <?php     
 class Math   
 {     
    public static function Max($num1, $num2) {     
    return $num1 > $num2 ? $num1 : $num2;     
    }          
 }     
 $a = 99;     
 $b = 88;     
 echo "显示 $a 和 $b 中的最大值是";      echo "<br />";      echo Math::Max($a, $b);      echo "<br />";    echo "<br />";    echo "<br />";      $a = 99;      $b = 100;      echo "显示 $a 和 $b 中的最大值是";      echo "<br />";      echo Math::Max($a,$b);      ?>   静态方法如何调用静态方法第一个例子，一个静态方法调用其它静态方法时，使用 self::  <?php      // 实现最大值比较的Math类。      class Math    {         public static function Max($num1, $num2) {             return $num1 > $num2 ? $num1 : $num2;         }         public static function Max3($num1, $num2, $num3) {             $num1 = self::Max($num1, $num2);             $num2 = self::Max($num2, $num3);             $num1 = self::Max($num1, $num2);                     return $num1;         }      }      $a = 99;      $b = 77;      $c = 88;      echo "显示 $a $b $c 中的最大值是";      echo "<br />";      echo Math::Max3($a, $b, $c);      ?> 静态方法调用静态属性使用self:: 调用本类的静态属性。 <?php      //       class Circle {         public static $pi = 3.14;         public static function circleAcreage($r) {         return $r * $r * self::$pi;         }      }      $r = 3;      echo " 半径 $r 的圆的面积是 " . Circle::circleAcreage($r);      ?>    self:: 和 ￥this 都不能在静态 方法 中调用非静态 属性类中有非静态方法被 self:: 调用时，系统会自动将这个方法转换为静态方法1.静态方法不能调用非静态属性 。不能使用self::调用非静态属性。 <?php      // 这个方式是错误的      class Circle {         public $pi = 3.14;         public static function circleAcreage($r) {             return $r * $r * self::pi;     //错误，不能调用非静态属性。    }      }      $r = 3;      echo " 半径 $r 的圆的面积是 " . Circle::circleAcreage($r);      ?>   2.也不能使用 $this 获取非静态属性的值。PHP5中，在静态方法中不能使用 $this 调用非静态方法。 <?php    // 实现最大值比较的Math类。      class Math {             public function Max($num1, $num2) {             echo "bad<br />";                     return $num1 > $num2 ? $num1 : $num2;         }         public static function Max3($num1, $num2, $num3) {             $num1 = $this->Max($num1, $num2);     //不能用 $this 调用非静态方法。        $num2 = $this->Max($num2, $num3);     //        $num1 = $this->Max($num1, $num2);     //                return $num1;         }      }      $a = 99;      $b = 77;      $c = 188;      echo "显示 $a $b $c 中的最大值是";      echo "<br />";      echo Math::Max3($a, $b, $c);    //同样的这个会报错     ?>   3.当一个类中有非静态 方法 被 self:: 调用时，系统会自动将这个方法转换为静态方法。 <?php      // 实现最大值比较的Math类。      class Math {             public function Max($num1, $num2) {                return $num1 > $num2 ? $num1 : $num2;         }         public static function Max3($num1, $num2, $num3) {             $num1 = self::Max($num1, $num2);             $num2 = self::Max($num2, $num3);             $num1 = self::Max($num1, $num2);                     return $num1;         }      }      $a = 99;      $b = 77;      $c = 188;      echo "显示 $a $b $c 中的最大值是";      echo "<br />";      echo Math::Max3($a, $b, $c);      ?>---------------------------------------------php中this的含义class User {    public $name;        function getName() {            echo $this->name;    }}=> 
数组中 用于数组的 key 和 value之间的关系
例如：
$a = array(
  '0' => '1',
  '2' => '4',
);

echo $a['0'];
echo $a['2'];


-> 
类中 用于引用类实例的方法和属性
例如：
class Test{
    function add(){return $this->var++;}
    var $var = 0;
}

$a = new Test; //实例化对象名称
echo $a->add();
echo $a->var;


：：
类中 静态方法和静态属性的引用方法
例如
class Test{
    public static function test(){
    public static $test = 1;
   }
}

类的静态方法和静态属性可以不用实例化对象直接使用（使用的方式是 类名::静态方法名 ）

Test::test(); 调用静态方法test
Test::$test;  来取得$test静态属性的值

注：
静态方法在读到这个类或者引入这个类文件的时候，就已经实例化并存放到内存中了，非静态类则需要new一下。
静态类在内存中即使有多个实例，静态的属性也只有一份。


==== selef=== $this ======

self是引用静态类的类名，而$this是引用非静态类的实例名

static 的属性和方法，只能访问static的属性和方法，不能类访问非静态的属性和方法。
因为静态属性和方法被创建时，可能还没有任何这个类的实例可以被调用。

static的属性，在内存中只有一份，为所有的实例共用。

使用self:: 关键字访问当前类的静态成员。

一个类的所有实例，共用类中的静态属性。

也就是说，在内存中即使有多个实例，静态的属性也只有一份。

下面例子中的设置了一个计数器$count属性，设置private 和 static 修饰。
这样，外界并不能直接访问$count属性。而程序运行的结果我们也看到多个实例在使用同一个静态的$count 属性。
 <?php     
 class user   
 {     
    private static $count = 0 ; //记录所有用户的登录情况.     
    public function __construct() {     
        self::$count = self::$count + 1;     
    }     
    public function getCount() {       
        return self::$count;     
    }     
    public function __destruct() {     
        self::$count = self::$count - 1;     
    }     
 }     
 $user1 = new user();     
 $user2 = new user();     
 $user3 = new user();     
 echo "now here have " . $user1->getCount() . " user";     
 echo "<br />";     
 unset($user3);     
 echo "now here have " . $user1->getCount() . " user";     
 ?>    

静态属性直接调用
静态属性不需要实例化就可以直接使用，在类还没有创建时就可以直接使用。

使用的方式是： 类名::静态属性名

 <?php     
 class Math   
 {     
    public static $pi = 3.14;     
 }     
 // 求一个半径3的园的面积。     
 $r = 3;     
 echo "半径是 $r 的面积是<br />";     
 echo Math::$pi * $r * $r;     
 echo "<br /><br />";     
 //这里我觉得 3.14 不够精确，我把它设置的更精确。     
 Math::$pi = 3.141592653589793;     
 echo "半径是 $r 的面积是<br />";     
 echo Math::$pi * $r * $r;      
 ?>    


类没有创建，静态属性就可以直接使用。那静态属性在什么时候在内存中被创建？ 在PHP中没有看到相关的资料。
引用Java中的概念，来解释应该也具有通用性。静态属性和方法，在类被调用时创建。

静态方法
静态方法不需要所在类被实例化就可以直接使用。

使用的方式是 类名::静态方法名

下面我们继续写这个Math类，用来进行数学计算。我们设计一个方法用来算出其中的最大值。
既然是数学运算，如果这个方法可以拿过来就用就方便多了。
我们这只是为了演示static方法设计的这个类。在PHP提供了max()函数比较数值。

 <?php     
 class Math   
 {     
    public static function Max($num1, $num2) {     
    return $num1 > $num2 ? $num1 : $num2;     
    }          
 }     
 $a = 99;     
 $b = 88;     
 echo "显示 $a 和 $b 中的最大值是";      echo "<br />";      echo Math::Max($a, $b);      echo "<br />";    echo "<br />";    echo "<br />";      $a = 99;      $b = 100;      echo "显示 $a 和 $b 中的最大值是";      echo "<br />";      echo Math::Max($a,$b);      ?>   静态方法如何调用静态方法第一个例子，一个静态方法调用其它静态方法时，使用 self::  <?php      // 实现最大值比较的Math类。      class Math    {         public static function Max($num1, $num2) {             return $num1 > $num2 ? $num1 : $num2;         }         public static function Max3($num1, $num2, $num3) {             $num1 = self::Max($num1, $num2);             $num2 = self::Max($num2, $num3);             $num1 = self::Max($num1, $num2);                     return $num1;         }      }      $a = 99;      $b = 77;      $c = 88;      echo "显示 $a $b $c 中的最大值是";      echo "<br />";      echo Math::Max3($a, $b, $c);      ?> 静态方法调用静态属性使用self:: 调用本类的静态属性。 <?php      //       class Circle {         public static $pi = 3.14;         public static function circleAcreage($r) {         return $r * $r * self::$pi;         }      }      $r = 3;      echo " 半径 $r 的圆的面积是 " . Circle::circleAcreage($r);      ?>    self:: 和 ￥this 都不能在静态 方法 中调用非静态 属性类中有非静态方法被 self:: 调用时，系统会自动将这个方法转换为静态方法1.静态方法不能调用非静态属性 。不能使用self::调用非静态属性。 <?php      // 这个方式是错误的      class Circle {         public $pi = 3.14;         public static function circleAcreage($r) {             return $r * $r * self::pi;     //错误，不能调用非静态属性。    }      }      $r = 3;      echo " 半径 $r 的圆的面积是 " . Circle::circleAcreage($r);      ?>   2.也不能使用 $this 获取非静态属性的值。PHP5中，在静态方法中不能使用 $this 调用非静态方法。 <?php    // 实现最大值比较的Math类。      class Math {             public function Max($num1, $num2) {             echo "bad<br />";                     return $num1 > $num2 ? $num1 : $num2;         }         public static function Max3($num1, $num2, $num3) {             $num1 = $this->Max($num1, $num2);     //不能用 $this 调用非静态方法。        $num2 = $this->Max($num2, $num3);     //        $num1 = $this->Max($num1, $num2);     //                return $num1;         }      }      $a = 99;      $b = 77;      $c = 188;      echo "显示 $a $b $c 中的最大值是";      echo "<br />";      echo Math::Max3($a, $b, $c);    //同样的这个会报错     ?>   3.当一个类中有非静态 方法 被 self:: 调用时，系统会自动将这个方法转换为静态方法。 <?php      // 实现最大值比较的Math类。      class Math {             public function Max($num1, $num2) {                return $num1 > $num2 ? $num1 : $num2;         }         public static function Max3($num1, $num2, $num3) {             $num1 = self::Max($num1, $num2);             $num2 = self::Max($num2, $num3);             $num1 = self::Max($num1, $num2);                     return $num1;         }      }      $a = 99;      $b = 77;      $c = 188;      echo "显示 $a $b $c 中的最大值是";      echo "<br />";      echo Math::Max3($a, $b, $c);      ?>---------------------------------------------php中this的含义class User {    public $name;        function getName() {            echo $this->name;    }}
$pi = 3.14;     


 }     


 // 求一个半径3的园的面积。     


$r = 3;     


echo 
"半径是 
$r
 的面积是
<
br /
>
";     


echo Math::
$pi * 
$r * 
$r;     


echo 
"
<
br /
>
<
br /
>
";     


 //这里我觉得 3.14 不够精确，我把它设置的更精确。     


 Math::
$pi = 3.141592653589793;     


echo 
"半径是 
$r
 的面积是
<
br /
>
";     


echo Math::
$pi * 
$r * 
$r;      


 ?
>






类没有创建，静态属性就可以直接使用。那静态属性在什么时候在内存中被创建？ 在PHP中没有看到相关的资料。


引用Java中的概念，来解释应该也具有通用性。静态属性和方法，在类被调用时创建。




静态方法


静态方法不需要所在类被实例化就可以直接使用。




使用的方式是 类名::静态方法名




下面我们继续写这个Math类，用来进行数学计算。我们设计一个方法用来算出其中的最大值。


既然是数学运算，如果这个方法可以拿过来就用就方便多了。


我们这只是为了演示static方法设计的这个类。在PHP提供了max()函数比较数值。




<
?php     


 class Math   


 {     


    public static 
function Max(
$num1, 
$num2) {     


return 
$num1 
>
$num2 ? 
$num1 : 
$num2;     


    }          


 }     


$a = 99;     


$b = 88;     


echo 
"显示 
$a 和 $b 中的最大值是";      echo "
<
br /
>
";      echo Math::Max($a, $b);      echo "
<
br /
>
";    echo "
<
br /
>
";    echo "
<
br /
>
";      $a = 99;      $b = 100;      echo "显示 $a 和 $b 中的最大值是";      echo "
<
br /
>
";      echo Math::Max($a,$b);      ?
>
   静态方法如何调用静态方法第一个例子，一个静态方法调用其它静态方法时，使用 self::  
<
?php      // 实现最大值比较的Math类。      class Math    {         public static function Max($num1, $num2) {             return $num1 
>
 $num2 ? $num1 : $num2;         }         public static function Max3($num1, $num2, $num3) {             $num1 = self::Max($num1, $num2);             $num2 = self::Max($num2, $num3);             $num1 = self::Max($num1, $num2);                     return $num1;         }      }      $a = 99;      $b = 77;      $c = 88;      echo "显示 $a $b $c 中的最大值是";      echo "
<
br /
>
";      echo Math::Max3($a, $b, $c);      ?
>
 静态方法调用静态属性使用self:: 调用本类的静态属性。 
<
?php      //       class Circle {         public static $pi = 3.14;         public static function circleAcreage($r) {         return $r * $r * self::$pi;         }      }      $r = 3;      echo " 半径 $r 的圆的面积是 " . Circle::circleAcreage($r);      ?
>
    self:: 和 ￥this 都不能在静态 方法 中调用非静态 属性类中有非静态方法被 self:: 调用时，系统会自动将这个方法转换为静态方法1.静态方法不能调用非静态属性 。不能使用self::调用非静态属性。 
<
?php      // 这个方式是错误的      class Circle {         public $pi = 3.14;         public static function circleAcreage($r) {             return $r * $r * self::pi;     //错误，不能调用非静态属性。    }      }      $r = 3;      echo " 半径 $r 的圆的面积是 " . Circle::circleAcreage($r);      ?
>
   2.也不能使用 $this 获取非静态属性的值。PHP5中，在静态方法中不能使用 $this 调用非静态方法。 
<
?php    // 实现最大值比较的Math类。      class Math {             public function Max($num1, $num2) {             echo "bad
<
br /
>
";                     return $num1 
>
 $num2 ? $num1 : $num2;         }         public static function Max3($num1, $num2, $num3) {             $num1 = $this-
>
Max($num1, $num2);     //不能用 $this 调用非静态方法。        $num2 = $this-
>
Max($num2, $num3);     //        $num1 = $this-
>
Max($num1, $num2);     //                return $num1;         }      }      $a = 99;      $b = 77;      $c = 188;      echo "显示 $a $b $c 中的最大值是";      echo "
<
br /
>
";      echo Math::Max3($a, $b, $c);    //同样的这个会报错     ?
>
   3.当一个类中有非静态 方法 被 self:: 调用时，系统会自动将这个方法转换为静态方法。 
<
?php      // 实现最大值比较的Math类。      class Math {             public function Max($num1, $num2) {                return $num1 
>
 $num2 ? $num1 : $num2;         }         public static function Max3($num1, $num2, $num3) {             $num1 = self::Max($num1, $num2);             $num2 = self::Max($num2, $num3);             $num1 = self::Max($num1, $num2);                     return $num1;         }      }      $a = 99;      $b = 77;      $c = 188;      echo "显示 $a $b $c 中的最大值是";      echo "
<
br /
>
";      echo Math::Max3($a, $b, $c);      ?
>
---------------------------------------------php中this的含义class User {    public $name;        function getName() {            echo $this-
>
name;    }}=> 
数组中 用于数组的 key 和 value之间的关系
例如：
$a = array(
  '0' => '1',
  '2' => '4',
);

echo $a['0'];
echo $a['2'];


-> 
类中 用于引用类实例的方法和属性
例如：
class Test{
    function add(){return $this->var++;}
    var $var = 0;
}

$a = new Test; //实例化对象名称
echo $a->add();
echo $a->var;


：：
类中 静态方法和静态属性的引用方法
例如
class Test{
    public static function test(){
    public static $test = 1;
   }
}

类的静态方法和静态属性可以不用实例化对象直接使用（使用的方式是 类名::静态方法名 ）

Test::test(); 调用静态方法test
Test::$test;  来取得$test静态属性的值

注：
静态方法在读到这个类或者引入这个类文件的时候，就已经实例化并存放到内存中了，非静态类则需要new一下。
静态类在内存中即使有多个实例，静态的属性也只有一份。


==== selef=== $this ======

self是引用静态类的类名，而$this是引用非静态类的实例名

static 的属性和方法，只能访问static的属性和方法，不能类访问非静态的属性和方法。
因为静态属性和方法被创建时，可能还没有任何这个类的实例可以被调用。

static的属性，在内存中只有一份，为所有的实例共用。

使用self:: 关键字访问当前类的静态成员。

一个类的所有实例，共用类中的静态属性。

也就是说，在内存中即使有多个实例，静态的属性也只有一份。

下面例子中的设置了一个计数器$count属性，设置private 和 static 修饰。
这样，外界并不能直接访问$count属性。而程序运行的结果我们也看到多个实例在使用同一个静态的$count 属性。
 <?php     
 class user   
 {     
    private static $count = 0 ; //记录所有用户的登录情况.     
    public function __construct() {     
        self::$count = self::$count + 1;     
    }     
    public function getCount() {       
        return self::$count;     
    }     
    public function __destruct() {     
        self::$count = self::$count - 1;     
    }     
 }     
 $user1 = new user();     
 $user2 = new user();     
 $user3 = new user();     
 echo "now here have " . $user1->getCount() . " user";     
 echo "<br />";     
 unset($user3);     
 echo "now here have " . $user1->getCount() . " user";     
 ?>    

静态属性直接调用
静态属性不需要实例化就可以直接使用，在类还没有创建时就可以直接使用。

使用的方式是： 类名::静态属性名

 <?php     
 class Math   
 {     
    public static $pi = 3.14;     
 }     
 // 求一个半径3的园的面积。     
 $r = 3;     
 echo "半径是 $r 的面积是<br />";     
 echo Math::$pi * $r * $r;     
 echo "<br /><br />";     
 //这里我觉得 3.14 不够精确，我把它设置的更精确。     
 Math::$pi = 3.141592653589793;     
 echo "半径是 $r 的面积是<br />";     
 echo Math::$pi * $r * $r;      
 ?>    


类没有创建，静态属性就可以直接使用。那静态属性在什么时候在内存中被创建？ 在PHP中没有看到相关的资料。
引用Java中的概念，来解释应该也具有通用性。静态属性和方法，在类被调用时创建。

静态方法
静态方法不需要所在类被实例化就可以直接使用。

使用的方式是 类名::静态方法名

下面我们继续写这个Math类，用来进行数学计算。我们设计一个方法用来算出其中的最大值。
既然是数学运算，如果这个方法可以拿过来就用就方便多了。
我们这只是为了演示static方法设计的这个类。在PHP提供了max()函数比较数值。

 <?php     
 class Math   
 {     
    public static function Max($num1, $num2) {     
    return $num1 > $num2 ? $num1 : $num2;     
    }          
 }     
 $a = 99;     
 $b = 88;     
 echo "显示 $a 和 $b 中的最大值是";      echo "<br />";      echo Math::Max($a, $b);      echo "<br />";    echo "<br />";    echo "<br />";      $a = 99;      $b = 100;      echo "显示 $a 和 $b 中的最大值是";      echo "<br />";      echo Math::Max($a,$b);      ?>   静态方法如何调用静态方法第一个例子，一个静态方法调用其它静态方法时，使用 self::  <?php      // 实现最大值比较的Math类。      class Math    {         public static function Max($num1, $num2) {             return $num1 > $num2 ? $num1 : $num2;         }         public static function Max3($num1, $num2, $num3) {             $num1 = self::Max($num1, $num2);             $num2 = self::Max($num2, $num3);             $num1 = self::Max($num1, $num2);                     return $num1;         }      }      $a = 99;      $b = 77;      $c = 88;      echo "显示 $a $b $c 中的最大值是";      echo "<br />";      echo Math::Max3($a, $b, $c);      ?> 静态方法调用静态属性使用self:: 调用本类的静态属性。 <?php      //       class Circle {         public static $pi = 3.14;         public static function circleAcreage($r) {         return $r * $r * self::$pi;         }      }      $r = 3;      echo " 半径 $r 的圆的面积是 " . Circle::circleAcreage($r);      ?>    self:: 和 ￥this 都不能在静态 方法 中调用非静态 属性类中有非静态方法被 self:: 调用时，系统会自动将这个方法转换为静态方法1.静态方法不能调用非静态属性 。不能使用self::调用非静态属性。 <?php      // 这个方式是错误的      class Circle {         public $pi = 3.14;         public static function circleAcreage($r) {             return $r * $r * self::pi;     //错误，不能调用非静态属性。    }      }      $r = 3;      echo " 半径 $r 的圆的面积是 " . Circle::circleAcreage($r);      ?>   2.也不能使用 $this 获取非静态属性的值。PHP5中，在静态方法中不能使用 $this 调用非静态方法。 <?php    // 实现最大值比较的Math类。      class Math {             public function Max($num1, $num2) {             echo "bad<br />";                     return $num1 > $num2 ? $num1 : $num2;         }         public static function Max3($num1, $num2, $num3) {             $num1 = $this->Max($num1, $num2);     //不能用 $this 调用非静态方法。        $num2 = $this->Max($num2, $num3);     //        $num1 = $this->Max($num1, $num2);     //                return $num1;         }      }      $a = 99;      $b = 77;      $c = 188;      echo "显示 $a $b $c 中的最大值是";      echo "<br />";      echo Math::Max3($a, $b, $c);    //同样的这个会报错     ?>   3.当一个类中有非静态 方法 被 self:: 调用时，系统会自动将这个方法转换为静态方法。 <?php      // 实现最大值比较的Math类。      class Math {             public function Max($num1, $num2) {                return $num1 > $num2 ? $num1 : $num2;         }         public static function Max3($num1, $num2, $num3) {             $num1 = self::Max($num1, $num2);             $num2 = self::Max($num2, $num3);             $num1 = self::Max($num1, $num2);                     return $num1;         }      }      $a = 99;      $b = 77;      $c = 188;      echo "显示 $a $b $c 中的最大值是";      echo "<br />";      echo Math::Max3($a, $b, $c);      ?>---------------------------------------------php中this的含义class User {    public $name;        function getName() {            echo $this->name;    }}
```



