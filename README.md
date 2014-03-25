PHPSpec reference guide
=======================

A guide containing phpspec snippets for different use cases

* [Mocks](#mocks)
* [Exceptions](#exceptions)

## Mocks

### Creating mocks

#### Injecting mocks in constructor

Inject a mock in the constructor for use in your specs:

```php
// spec/Acme/HandlerSpec.php

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

Imagine you have a Messenger class, containing a `send(Message $message)` method. While we run `send($message)` we want to make sure $message will return 'abc' when executed.

```php
function it_sends_the_message(Acme\Message $message)
{
    $message->getContents()->willReturn('abc');
    $this->send($message);
}
```

#### Specify that a mock method should be called

Usually we want to make sure that a mock method is executed. We do that with `shouldBeCalled()`.

```php
function it_sends_the_message(Acme\Message $message)
{
    $message->getContents()->shouldBeCalled()->willReturn('abc');
    $this->send($message);
}
```

## Exceptions

### Throw exception

Expect an `InvalidArgumentException` to be thrown when we call `send('bad')`:

```php
function it_throws_exception_during_send(Acme\Message $message)
{
    $this->shouldThrow('\InvalidArgumentException')->duringSend('bad');
}
```

### Throw exception with message

Expect an `InvalidArgumentException` with message "Wrong argument" to be thrown when we call `send('bad')`:

```php
function it_throws_exception_during_send(Acme\Message $message)
{
    $exception = new InvalidArgumentException('Wrong argument');

    $this->shouldThrow($exception)->duringSend('bad');
}
```

### Throw exception in constructor

Expect an `InvalidArgumentException` to be thrown when we call the constructor `__construct('bad1, 'bad2')`:

```php
function it_throws_exception_during_constructor(Acme\Message $message)
{
    $this->shouldThrow(new \InvalidArgumentException)->during('__construct', array('bad1', 'bad2'));
}
```


## Sources

* https://github.com/phpspec/phpspec-docs/blob/master/en/writing-specs.rst
* http://www.slideshare.net/marcello.duarte/phpspec-bdd-for-php
* https://github.com/yvoyer/phpspec-cheat-sheet
