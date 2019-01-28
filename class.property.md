# 屬性 (Property)

> http://php.net/manual/zh/language.oop5.properties.php

## 類別屬性 (attribute)

````
class MyClass
{
  public $foo = 'myConstant';
  public $bar = [true, false];
  public $mix = <<<EOD
hello world
EOD;
}

# 存取屬性方法(靜態)
echo MyClass::$mix;

# 存取屬性方法
$class = new MyClass();
echo $class->mix;
````