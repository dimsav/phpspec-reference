PHPSpec reference guide
=======================

A guide containing phpspec snippets for different use cases

* [Commands](#commands)
* [Mocks](#mocks)
* [Exceptions](#exceptions)
* [Matching types](#matching-types)
* [Dynamic matching types](#dynamic-matching-types)
* [Plugins](#plugins)
* [Templates](#templates)

## Installation

Install with composer using this [composer.json](https://gist.github.com/dimsav/9686418).

## Commands

### Describe

`phpspec describe Namespaced/Class/Name`

Describe creates the spec file for the class you want to specify.

### Run

`phpspec run`

Run executes the the specs and ends with a nice output.

![PhpSpec output](https://i.cloudup.com/EIw8gDuYrr-2000x2000.png)

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

Expect an `InvalidArgumentException` to be thrown when we call "send('bad')". Passing the exception type as string prevents the message validation.

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

Expect an `InvalidArgumentException` to be thrown when we call the constructor `__construct('bad1, 'bad2')`. The exception is passed as instance without message. Therefore we expect no message when the exeption is thrown.

```php
function it_throws_exception_during_constructor(Acme\Message $message)
{
    $this->shouldThrow(new \InvalidArgumentException)->during('__construct', array('bad1', 'bad2'));
}
```

## Matching types

### Identical

```php
$this->method()->shouldBe($result);

// Alternative syntax
$this->method()->shouldReturn($result);
$this->method()->shouldEqual($result);
$this->method()->shouldBeEqualTo($result);
```

### Equal

```php
$this->method()->shouldBeLike($result)
```

### Data types

Expect the result of a method has a specific data type.

```php
$this->method()->shouldBeBool();
$this->method()->shouldBeObject();
$this->method()->shouldBeString();
$this->method()->shouldBeInteger();
$this->method()->shouldBeDecimal();
$this->method()->shouldBeArray();
```

### Objects

All the following syntaxes expect that method() returns an instance of the given class.

```php
$this->method()->shouldHaveType('\Full\Class\Name');
$this->method()->shouldReturnAnInstanceOf('\Full\Class\Name');
$this->method()->shouldBeAnInstance('\Full\Class\Name');
$this->method()->shouldImplement('\Full\Class\Name');
```

### Count

You can use `shouldHaveCount()` on a method returning an `array` or an instance of `\Countable`.

```php
$this->getArray()->shouldHaveCount(2)
$this->getCollection()->shouldHaveCount(2)
```

```php
public function getArray() {
    return array(1, 2)
}

public function getCollection() {
    return $this->users;
}
```

You can also use `shouldHaveCount()` on `$this`, if it is an instance of `\Countable`.

```php

$this->shouldHaveCount(10);

class UsersCollection implements \Countable {

    public function count() {
        return count($this->users);
    }
}
```

## Dynamic matching types

### Boolean shouldBe*

To use the shouldBe* matching, the class should have a public method starting with "is".

```php
// User.php
public function isActive() {
    return (bool) $this->isActive;
}
```

```php
// expect isActive returns true
$this->shouldBeActive();

// expect isActive returns false
$this->shouldNotBeActive();
```

### Boolean shouldHave*

To use the shouldHave* matching, the class should have a public method starting with "has".

```php
// User.php
public function hasProfile() {
    return (bool) $this->hasProfile;
}
```

```php
// expect hasProfile returns true
$this->shouldHaveProfile();

// expect hasProfile returns false
$this->shouldNotHaveProfile();
```

### Custom types

Custom types give us the possibility to expect something using a closure. To do that we have to define a "getMatchers()" method in our spec class.

#### getMatchers()

getMatchers() should return an array with keys describing the expectations and values the closures containing the logic of the expectations. The first parameter in the closure is the output of the executed method.

```php

function getMatchers()
{
    return array(
        'haveLength' => function($result, $count) {
            return strlen($result) == $count;
        }
    );
}
```

#### Using getMatchers()

To use the above expectation we use the method `should` + `{getMatchers key}`.

```php
$this->method()->shouldHaveLenth(12);
```

## Plugins

* [Code coverage](https://github.com/dimsav/phpspec-reference/wiki/PhpSpec:-Code-coverage-plugin)

## Templates

Custom templates can be used when phpspec generates php code. There are three template types, each of which, requires a different file name:

* Specs: `specification.tpl`
* Classes: `class.tpl`
* Methods: `method.tpl`

Phpspec will look for template files in these locations with the following order:

1. `{project_directory}/.phpspec/`
2. `{user_home_directory}/.phpspec/`

To see the available template parameters visit the [parameters chapter](http://www.phpspec.net/cookbook/templates.html#parameters) in the docs.

## Sources

* https://github.com/phpspec/phpspec-docs/blob/master/en/writing-specs.rst
* http://www.slideshare.net/marcello.duarte/phpspec-bdd-for-php
* https://github.com/yvoyer/phpspec-cheat-sheet
