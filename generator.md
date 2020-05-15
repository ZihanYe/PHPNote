# generator

**example 1:**

```php
<?php
function xrange($start, $limit, $step = 1) {
    if ($start <= $limit) {
        if ($step <= 0) {
            throw new LogicException('Step must be positive');
        }

        for ($i = $start; $i <= $limit; $i += $step) {
            yield $i; // this is the key part.
        }
    } else {
        if ($step >= 0) {
            throw new LogicException('Step must be negative');
        }

        for ($i = $start; $i >= $limit; $i += $step) {
            yield $i;
        }
    }
}

// usage
echo 'Single digit odd numbers from xrange(): ';
foreach (xrange(1, 9, 2) as $number) {
    echo "$number ";
}

// above is equivalent to:
echo 'Single digit odd numbers from range():  ';
foreach (range(1, 9, 2) as $number) {
    echo "$number ";
}
echo "\n";
?>
```

**example 2: yielding by reference**

```php
<?php
function &gen_reference() { // & means by reference, same as normal function
    $value = 3;

    while ($value > 0) {
        yield $value;
    }
}

/*
 * Note that we can change $number within the loop, and
 * because the generator is yielding references, $value
 * within gen_reference() changes.
 */
foreach (gen_reference() as &$number) {
    echo (--$number).'... ';   // this decrements $number, and hence $value inside generator
}

// output: 2... 1... 0... 
?>
```

**example 3: yield from**
```php
<?php
function inner() {
    yield 1;
    yield 2;
    yield 3; 
}
function gen() {
    yield 0;
    yield from inner();
    yield 4;
}

// this gives 0,1,2,3,4,5
?>
```

**caution:**

```php
<?php
function gen(){
	$data = (yield 1); // () needed in an expression context in PHP 5
	yield; // yield a NULL
	yield $data => $fields; // yield a key/value pair
}
?>
```

Execution of a generator function is postponed until iteration over its result (the Generator object) begins.

```php
<?php
<?php

$some_state = 'initial';

function gen() {
    global $some_state;
    echo "gen() execution start\n";
    $some_state = "changed";

    yield 1;
    yield 2;
}

function peek_state() {
    global $some_state;
    echo "$some_state\n";
}

echo "calling gen()...\n";
$result = gen();
echo "gen() was called\n";		// but gen is not executed here

peek_state();					// will print "initial"

echo "iterating...\n";
foreach ($result as $val) {		// execution of gen starts in the first loop
    echo "iteration: $val\n";
    peek_state();				// will print "changed"
}

?>
```

Yield from does not reset the keys. It preserves the keys returned by the Traversable object, or array.

```php
<?php
function inner() {
    yield 1; // key 0
    yield 2; // key 1
    yield 3; // key 2
}
function gen() {
    yield 0; // key 0
    yield from inner(); // keys 0-2
    yield 4; // key 1
}


var_dump(iterator_to_array(gen())); 
/* this prints:
array(3) {
  [0]=>
  int(1)
  [1]=>
  int(4)
  [2]=>
  int(3)
} 
*/

// the second parameter of iterator_to_array is use_keys, FALSE means not using keys returned by generator
var_dump(iterator_to_array(gen(), FALSE));
// this gives array [0,1,2,3,4,5]
?>
```

