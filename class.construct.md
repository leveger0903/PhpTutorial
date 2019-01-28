# 建構方法 (Construct) / 解構方法 (Destruct)

> http://php.net/manual/zh/language.oop5.decon.php

- 建構方法
- 解構方法

## 建構方法

````
class SimpleClass
{
  function __construct()
  {
    echo 'initialized class.';
  }
}

$class = new SimpleClass();
````

## 解構方法

````
class SimpleClass
{
  function start()
  {
    echo 'echo.';
  }

  function __destruct()
  {
    echo 'terminated class.';
  }
}

$class = new SimpleClass();
$class->start();
````

