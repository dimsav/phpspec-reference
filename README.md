PHPSpec reference guide
=======================

A guide containing phpspec snippets for different use cases

* [Mocks](#mocks)

## Mocks

### Creating mocks

#### Injecting mocks in constructor

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

#### Injecting mocks in spec

```php
function it_sends_the_message(Acme\Dependency $dependency)
{
    // $depency is now a new mock of Acme\Dependency
}

```

### Using mocks

#### Returning values from mock methods

Imagine you have a Messenger class, containing a `send(Message $message)` method. Let's spec that:

```php
function it_sends_the_message(Acme\Message $message)
{
    $message->getContents()->willReturn('abc');
    $this->send($message);
}
```

#### Specify that a mock method should be called

```php
function it_sends_the_message(Acme\Message $message)
{
    $message->getContents()->shouldBeCalled()->willReturn('abc');
    $this->send($message);
}
```



