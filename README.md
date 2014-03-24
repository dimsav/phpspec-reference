PHPSpec reference guide
=======================

A guide containing phpspec snippets for different use cases

* [Mocks](#mocks)

## Mocks

### Injecting mocks in constructor

Inject a mock in the constructor for use in your specs:

```php
// spec\Acme\HandlerSpec.php

function let(Acme\Dependency $dependency)
{
    $this->beConstructedWith($dependency)
}

// Alternative syntax
function let($dependency)
{
    $dependency->beADoubleOf('Acme\Dependency');
    $this->beConstructedWith($dependency)
}

```

Pass in the spec the same mock variable name:

```php
// Our spec
function it_sends_the_message($dependency)
{
    // $depency is now the mock we created in let()
}

```
