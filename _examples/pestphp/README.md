# PestPHP

https://pestphp.com/docs/

### Klasse

```php
<?php

class addon
{
    public static function getName(): string
    {
        return 'addon';
    }

    public static function getValue(string $key)
    {
        $value_collection = [
            'key1' => 1,
            'key2' => 2,
            'key3' => 3,
        ];

        if (!array_key_exists($key, $value_collection)) {
            return null;
        }

        return $key;
    }
}
```

### Tests

`tests/unit/example_test.php`

```php
<?php

test('expect to get a name', function ()
{
    expect(addon::getName())->toBeString();
});

test('expect value to be null', function ()
{
    $value = addon::getValue('key4');

    expect($value)->toBeNull();
});

test('expect value to be a string', function ()
{
    $value = addon::getValue('key2');

    expect($value)->toBeString();
});
```

## Resultat

```
 ✔ Expect to get a name [4.82 ms]
 ✔ Expect value to be null [0.17 ms]
 ✔ Expect value to be a string [0.04 ms]
```