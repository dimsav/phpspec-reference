phpspec-reference
=================

A guide containing phpspec use cases

## Mocks

### Injecting mocks

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

### Using mocks

Pass in the spec the same mock variable name:

```php
// Our spec
function it_sends_the_message($dependency)
{
    $dependency->send()->shouldBeCalled();
    $this->someMethod();
}

```
