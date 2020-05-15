# reference

1. Unlike in C, PHP references are not treated as pre-dereferenced pointers, but as complete aliases.

```php
<?php
$var = "foo";
$ref =& $var;

unset($var); // the original data will not be removed until *all* references to it have been removed.

echo $var; // Undefined variable: var
echo $ref; // "foo"


$var = "foo";
$ref =& $var;

$ref = NULL; // To remove the original data without removing all references to it, simply set it to null.

echo $var; // Value is NULL, so nothing prints
echo $ref; // Value is NULL, so nothing prints
?>
```

2. If you assign, pass, or return an undefined variable by reference, it will get created.

```php
<?php
function foo(&$var) { }

foo($a); // $a is "created" and assigned to null

$b = array();
foo($b['b']);
var_dump(array_key_exists('b', $b)); // bool(true)

$c = new StdClass;
foo($c->d);
var_dump(property_exists($c, 'd')); // bool(true)
?>
```

3. If you assign a reference to a variable declared global inside a function, the reference will be visible only inside the function.

```php
$var1 = "Example variable";
$var2 = "";

function global_references()
{
    global $var1, $var2;
    $var2 =& $var1; // visible only inside the function
}
```

4. reference of array variables

```php
<?php
/* Assignment of array variables */
$arr = array(1);
$a =& $arr[0]; //$a and $arr[0] are in the same reference set
$arr2 = $arr; //not an assignment-by-reference!
$arr2[0]++;
/* $a == 2, $arr == array(2) */
/* The contents of $arr are changed even though it's not a reference! */
?>
```

```php
<?php

$v1=0;
$arrV=array(&$v1,&$v1);
foreach ($arrV as $v) // $v in foreach is not a reference to $v1 but a copy of the object the actual element in the array was referencing to
{
  $v1++;
  echo $v."\n";
}

/*
yields
0
1
*/

$v1=0;
$arrV=array(&$v1,&$v1);
foreach ($arrV as $k=>$v)
{
    $v1++;
    echo $arrV[$k]."\n";
}

/*
yields
1
2
*/
?>
```
