# 物件繼承 (Inheritance)

> http://php.net/manual/zh/language.oop5.inheritance.php

## 基礎

````
class ExtendClass extends SimpleClass
{
  public function getItem()
  {
    $var = parent::getItem();
    return $var . 'I am the item.';
  }
}

class SimpleClass
{
  public function getTitle()
  {
    return 'I am the title.';
  }

  public function getItem()
  {
    return 'I am the parent item.';
  }
}

$class = new ExtendClass();

# 取得 parent 方法
echo $class->getTitle();

# 取得 parent 方法並改寫
echo $class->getItem();
````