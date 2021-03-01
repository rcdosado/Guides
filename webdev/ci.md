Codeigniter Stuffs
==========



## Testing using phpunit

### Example case of mocking your CI Model class, using phpunit

This guide assumes phpunit is installed on your CI and phpunit is in the path of the terminal

1.   you want to mock a Person class model, saved to <ci-project>/application/models folder
```php
<?php
class Person
{
    public function greeting()
    {
        return 'Hello World';
    }
}
```
example class usage:
```php
$person = new Person();
echo $person->greeting(); // prints Hello World
```
2. create a folder name tests to <ci-project>/application folder
3. create a file called PersonTest.php inside the tests folder and contents like this:
```php
require __DIR__ . '/../models/Person.php';
use PHPUnit\Framework\TestCase;

class MockPerson extends TestCase
{
    public function test_greeting()
    {
        $test = new Person();
        $this->assertEquals('Hello World', $test->greeting());
    }
}
```
4. executing phpunit from <ci-project>/application location in the terminal, run `phpunit tests` (note that there are )
```cmd
PHPUnit 9.5.2 by Sebastian Bergmann and contributors.

.                                                                   1 / 1 (100%)

Time: 00:00.002, Memory: 18.00 MB

OK (1 test, 1 assertion)
```

### Example case of mocking your CI Model class that uses a database resource
The scenario, you have a database class:

```php
<?php
class Database
{
    public function getPersonByID($id)
    {
        // do some stuff in the db to get a person by their ID
        return sql("select * from person where id = $id limit 1;")[0];
    }
}
```
and you this class is Person class
``` php
<?php
    
class Person
{
    public $db = null;
    
	function __construct($db) 
    {
        $this->db = $db;
    }
	public function greeting($id)
    {
        $friend = $this->db->getPersonByID($id);
        $friendName = $friend->name;
        return "Hello $friendName";
    }
}

require __DIR__ . '/Database.php';
$db = new Database();
$person = new Person($db);

// in our database we have a person with an ID of 2 and a name of "Bob"
echo $person->greeting(2);
// result should be "Hello Bob"
```


The Usual Test (uses database resource to test a model)

```php
<?php
require __DIR__ . '/../models/Person.php';
require __DIR__ . '/../models/Database.php';
use PHPUnit\Framework\TestCase;
class MockPerson extends TestCase
{
    public function test_greeting()
    {
        $db = new Database();
        $test = new Person($db);
        $this->assertEquals('Hello Bob', $test->greeting(2));
    }
}
```


The Mocked Test (connecting to database is not required just to test a CI model)

```php
<?php

require __DIR__ . '/../models/Person.php';
require __DIR__ . '/../models/Database.php';
use PHPUnit\Framework\TestCase;

class MockTest extends TestCase
{
    public function test_greeting()
    {
        $dbMock = $this->getMockBuilder(Database::class)->setMethods(['getPersonByID'])->getMock();
		$mockPerson = new stdClass();
		$mockPerson->name = 'Bob';
        $dbMock->method('getPersonByID')->willReturn($mockPerson);
		$test = new Person($dbMock);
        $this->assertEquals('Hello Bob', $test->greeting(2));
    }
}
```
credits to [Richard Miles](https://blog.nona.digital/mocking-in-phpunit/) 