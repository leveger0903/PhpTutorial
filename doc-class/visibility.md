# 可視層級 (Visibility)

> http://php.net/manual/zh/language.oop5.visibility.php

- 公開層級
- 受保護層級
- 私有層級
- var

## 公開層級

````
class SimpleClass
{
  public $content = 'public visibility.';
}

$class = new SimpleClass();
echo $class->content;
````

## 受保護層級

````
class ExtendClass extends SimpleClass
{
  public function getContent()
  {
    return $this->content;
  }
}

class SimpleClass
{
  protected $content = 'protected visibility.';
}

$class = new ExtendClass();
echo $class->getContent();
````

## 私有層級

````
class SimpleClass
{
  private $content = 'private visibility.';
  
  public function getContent()
  {
    return $this->content;
  }
}

$class = new SimpleClass();
echo $class->getContent();
````

## var

var 被視為公開層級, 未使用可視層級修飾子也視為公開層級

````
class SimpleClass
{
  var $content = 'public visibility.';
  
  function getContent()
  {
    return $this->content;
  }
}

$class = new SimpleClass();
echo $class->getContent();
````