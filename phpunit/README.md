# PHPUnit

https://phpunit.readthedocs.io/en/9.5/

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

```php
<?php

use PHPUnit\Framework\TestCase;

final class AddonTest extends TestCase
{
    public function testGetName(): void
    {
        self::assertIsString(addon::getName());
    }

    public function testGetInvalidValue(): void
    {
        $value = addon::getValue('key4');

        self::assertNull($value);
    }

    public function testGetValidValue(): void
    {
        $value = addon::getValue('key2');

        self::assertIsString($value);
    }
}
```

## Resultat

```
 ✔ Get name [3.01 ms]
 ✔ Get invalid value [0.18 ms]
 ✔ Get valid value [0.07 ms]
```