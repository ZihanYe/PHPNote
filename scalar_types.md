# Scalar types

## boolean

**display:**

```php
<?php
$var1 = TRUE;
$var2 = FALSE;

echo $var1; // Will display the number 1
echo $var2; //Will display nothing

echo (int)$var2; //This will display the number 0 for false.
```

**precedence:**

```php
<?php
$x=TRUE;
$y=FALSE;
$z=$y OR $x; // this is equivalent to ($z=$y) OR $x, so is FALSE
$z=$y || $x; // this is equivalent to $z=($y OR $x), so is TRUE
?>
```

**conversion to boolean:**

```php
<?php
var_dump((bool) "");        // bool(false)
var_dump((bool) 1);         // bool(true)
var_dump((bool) -2);        // bool(true)
var_dump((bool) "foo");     // bool(true)
var_dump((bool) 2.3e5);     // bool(true)
var_dump((bool) array(12)); // bool(true)
var_dump((bool) array());   // bool(false)
var_dump((bool) "false");   // bool(true)

// or can use !! to convert to bool:
$s = !!"";        // This will === false;
$a = !!array();   // This will === false;
?>
```

**implicit conversion:**

```php
// someKey is a boolean true
$array = array('someKey'=>true);

// in the following 'false' string gets converted (implicitly) to a boolean true
if($array['someKey'] != 'false')
    echo 'The value of someKey is '.$array['someKey'];

// nothing will printed
// true == 'false' is true
```

## integer



## float



## string
