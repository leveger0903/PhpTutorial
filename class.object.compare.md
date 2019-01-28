# 物件比較 (Object Comparison)

> http://php.net/manual/zh/language.oop5.object-comparison.php

## 兩個相同實例的類別比較

````
function bool2str($bool)
{
  if ($bool === false)
    return 'FALSE';
  else
    return 'TRUE';
}

function compareObject(&$o1, &$o2)
{
  echo "o1 == o2:" . bool2str($o1 == $o2) . "\n";
  echo "o1 != o2:" . bool2str($o1 != $o2) . "\n";
  echo "o1 === o2:" . bool2str($o1 === $o2) . "\n";
  echo "o1 !== o2:" . bool2str($o1 !== $o2) . "\n";
}

class Foo
{
  public $flag;
  function Flag($flag = true)
  {
    $this->flag = $flag;
  }
}

$o = new Foo;
$p = new Foo;
compareObject($o, $p);
````

結果

````
o1 == o2:TRUE
o1 != o2:FALSE
o1 === o2:FALSE
o1 !== o2:TRUE
````

## 實例化賦值傳遞比較

````
function bool2str($bool)
{
  if ($bool === false)
    return 'FALSE';
  else
    return 'TRUE';
}

function compareObject(&$o1, &$o2)
{
  echo "o1 == o2:" . bool2str($o1 == $o2) . "\n";
  echo "o1 != o2:" . bool2str($o1 != $o2) . "\n";
  echo "o1 === o2:" . bool2str($o1 === $o2) . "\n";
  echo "o1 !== o2:" . bool2str($o1 !== $o2) . "\n";
}

class Foo
{
  public $flag;
  function Flag($flag = true)
  {
    $this->flag = $flag;
  }
}

$o = new Foo;
$p = $o;
compareObject($o, $p);
````

結果

````
o1 == o2:TRUE
o1 != o2:FALSE
o1 === o2:TRUE
o1 !== o2:FALSE
````

## 兩個不同實例的物件比較

````
function bool2str($bool)
{
  if ($bool === false)
    return 'FALSE';
  else
    return 'TRUE';
}

function compareObject(&$o1, &$o2)
{
  echo "o1 == o2:" . bool2str($o1 == $o2) . "\n";
  echo "o1 != o2:" . bool2str($o1 != $o2) . "\n";
  echo "o1 === o2:" . bool2str($o1 === $o2) . "\n";
  echo "o1 !== o2:" . bool2str($o1 !== $o2) . "\n";
}

class Foo
{
  public $flag;
  function Flag($flag = true)
  {
    $this->flag = $flag;
  }
}

class Bar
{
  public $flag;
  function Flag($flag = true)
  {
    $this->flag = $flag;
  }
}

$o = new Foo;
$p = new Bar;
compareObject($o, $p);
````

## 結果

````
o1 == o2:FALSE
o1 != o2:TRUE
o1 === o2:FALSE
o1 !== o2:TRUE
````