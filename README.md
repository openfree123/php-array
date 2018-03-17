<?php
/**
 * Created by IntelliJ IDEA.
 * User: niyang
 * Date: 18/3/17
 * Time: 15:11
 */

/*
 * 1、多维数组排序
$items = array(
	array('http://www.abc.com/a/', 100, 120),
	array('http://www.abc.com/b/index.php', 50, 80),
	array('http://www.abc.com/a/index.html', 90, 100),
	array('http://www.abc.com/a/?id=12345', 200, 33),
	array('http://www.abc.com/c/index.html', 10, 20),
	array('http://www.abc.com/abc/', 10, 30)
);
变成如下的样子：
array (
  array ( 'http://www.abc.com/a/', 390,253),
  array ('http://www.abc.com/b/', 50,80),
  array ('http://www.abc.com/c/', 10,20),
  array ('http://www.abc.com/abc/', 10,30)
)
 */

$items = array(
    array('http://www.abc.com/a/', 100, 120),
    array('http://www.abc.com/b/index.php', 50, 80),
    array('http://www.abc.com/a/index.html', 90, 100),
    array('http://www.abc.com/a/?id=12345', 200, 33),
    array('http://www.abc.com/c/index.html', 10, 20),
    array('http://www.abc.com/abc/', 10, 30)
);

$urls = [];

foreach ($items as $item)
{
    $url = $item[0];
    $url_arr = explode('/', $url);
    array_pop($url_arr);
    array_push($url_arr, '');
    $url = join('/', $url_arr);

    if (isset($urls[$url])) {
        $urls[$url][0] = $url;
        $urls[$url][1] += $item[1];
        $urls[$url][2] += $item[2];
    } else {
        $urls[$url] = $item;
    }
}
#var_dump($urls);



//2、一群猴子排成一圈，按1，2，...，n依次编号。然后从第1只开始数，数到第m只,把它踢出圈，从它后面再开始数，再数到第m只，在把它踢出去...，如此不停的进行下去，直到最后只剩下一只猴子为止，那只猴子就叫做大王。要求编程模拟此过程，输入m、n, 输出最后那个大王的编号。
//$m = [1, 2, 3];  4
// 2

function king($n, $m){
    $range = range(1, $n);
    $i = 0;
    while (count($range) > 1 ) {
        $i++;
        $head = array_pop($range);
        if ($i % $m != 0) {
            array_push($range, $head);
        }
    }
    return $range;
}
$king = king(2, 19);
var_dump($king);

/**3、得分计算，已知道选题提交的答案是
$commits= 'A,B,B,A,C,C,D,A,B,C,D,C,C,C,D,A,B,C,D,A';
实际的答案是：
$answers= 'A,A,B,A,D,C,D,A,A,C,C,D,C,D,A,B,C,D,C,D'
每题得分是5分，那么这个同学得分是多少？*/

$commits = 'A,B,B,A,C,C,D,A,B,C,D,C,C,C,D,A,B,C,D,A';
$answers = 'A,A,B,A,D,C,D,A,A,C,C,D,C,D,A,B,C,D,C,D';

$commits_arr = explode(',', $commits);
$answers_arr = explode(',', $answers);

$intersect = array_intersect_key($commits_arr, $answers_arr);

$score = count($intersect) * 5;

#var_dump($intersect, $score);die;


//4、应用：使用php://input接收post提交的参数，从db中获取数据，并使用var_export写入文件缓存，下次访问从文件中获取数据。

/*$input = file_get_contents('php://input');
$params = parse_url($input);
$resource = mysqli_connect($host = '', $user = '', $password = '', $database = '');
mysqli_query($resource, "SET NAMES UTF8");
$q = mysqli_query($resource, "SELECT * FROM DEMO WHERE id = {$params['id']} LIMIT 1");
$row = mysqli_fetch_row($q);
$conf = var_export($row, true);
file_put_contents('web.conf', "<?php\nreturn " .$str);

$conf = include 'web.conf';*/



//5、实现一个对象的数组式访问接
class arrayinterface implements arrayaccess
{
    public $age = [];

    public function offsetExists($offset)
    {
        echo 123;
    }

    public function offsetGet($offset)
    {
        return $this->age[$offset];
    }

    public function offsetSet($offset, $value)
    {
        $this->age[$offset] = $value;
        return true;
    }

    public function offsetUnset($offset)
    {

    }
}

$arrayinterface = new arrayinterface();
$arrayinterface[0] = 123;

#var_dump($arrayinterface, isset($arrayinterface[1]));


//6、有1000瓶水，其中有一瓶有毒，小白鼠只要尝一点带毒的水24小时后就会死亡，问至少要多少只小白鼠才能在24小时鉴别出哪瓶水有毒？






//7、使用serialize序列化一个对象，并使用__sleep和__wakeup方法。
class demo {

    private   $name = '李四';
    public    $age  = 20;
    protected $sex  = '男';

    static    $height = 180;

    //当一个对象被串行化,PHP会调用__sleep方法
    public function __sleep()
    {
        return ['name','age', 'sex'];
    }

    //反串行化一个对象后,PHP 会调用__wakeup方法
    public function __wakeup()
    {
        $this->name = '张三';
    }
}

$demo = new demo();
$str = serialize($demo);

$obj = unserialize($str);
#var_dump($str, $obj);


//8、利用数组栈实现翻转字符串功能
$str = '0123456789';
$arr = str_split($str, 1);
$res = '';
foreach ($arr as $v)
{
    $res .= array_pop($arr);
}
#var_dump($res);


//9、从m个数中选出n个数来 ( 0 < n <= m) ，要求n个数之间不能有重复，其和等于一个定值k，求一段程序，罗列所有的可能。
//例如备选的数字是：11, 18, 12, 1, -2, 20, 8, 10, 7, 6 ，和k等于：18
