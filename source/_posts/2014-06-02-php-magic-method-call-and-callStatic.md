title: PHP中的魔术方法（一） __call()和__callStatic()
date: 2014-02-26 22:31:00
tags: 
 - php 
 - magic-mathod
categories: 
 - 技术 
---

php5.3.0中新增了两个魔术方法:**__call()**和**__callStatic()**
用这两个方法可以做很多神奇的事情

## 定义 ##

这两个函数的定义如下：
```php
public mixed __call ( string $name , array $arguments )
public static mixed __callStatic ( string $name , array $arguments )
```
`$name` 参数是要调用的方法名称。`$arguments` 参数是一个枚举数组，包含着要传递给方法 `$name` 的参数。

<!-- more -->

## 特性 ##

1. 在对象中调用一个不可访问方法时，**__call()** 会被调用。
1. 用静态方式中调用一个不可访问方法时，**__callStatic()** 会被调用。

即当访问一个类的未定义方法，或者没有权限的方法（如私有方法）时，php会自动去调用**__call()**函数**__callStatic()**函数。

## 使用 ##

利用这两个魔术方法可以实现如多函数公用一段逻辑等功能：
``` php
<?php
class UrlHelper
{
    private $url = 'www.haharazer.tk';
    public function __callStatic($name, $arguments)
    {
        return $this->url + $this->$name($arguments);
    }

    private function getAdminUrl()
    {
        return '/admin';
    }
    private function getTestUrl()
    {
        return '/test';
    }
}
$helper = new UrlHelper();
echo $helper->getAdminUrl();  // 输出'www.haharazer.tk/admin'
echo $helper->getTestUrl();  // 输出'www.haharazer.tk/test'
?>
```
参考：
[1] http://www.php.net/manual/zh/language.oop5.overloading.php#object.call
[2] http://blog.csdn.net/fdipzone/article/details/8581125