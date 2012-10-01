Test your code like a pro – PHPUnit in practice
================================================

Usage guide
-----------

### Step by step guide

To follow the tutorial step by step just clone the repository from the master branch (which is empty) and then start merging in each exercise, ie:

```
$ git clone git://github.com/proofek/phpnw12-tutorial.git
$ cd phpnw12-tutorial.git
$ git pull origin exercise-1
$ git pull origin exercise-2
$ git pull origin exercise-3
```

See the commit messages that provide a walkthough guide for every step.

### Checking out specific exercise

To quickly jump to a specific scenario, clone the repository from the master branch (which is empty) and then create a local branch for the specific exercise:

```
$ git clone git://github.com/proofek/phpnw12-tutorial.git
$ cd phpnw12-tutorial.git
$ git checkout -b exercise-1 origin/exercise-1
$ git checkout -b exercise-2 origin/exercise-2
$ git checkout -b exercise-3 origin/exercise-3
```

### Explore on github

You can easily follow every step by just looking at the commit history on github by simply switching between branches from github's UI

Exercise 1: Simple test case
----------------------------

### First test case

We will start with a very simple thing, a function that returns a string. Following the TDD principles we will start with implementing the test first and then implementing the functionality afterwards. Our class will be called Tutorial, it will be in *PhpNw12\Workshop* namespace and our method called *greetings()* will return string “Hello everybody at 'Test your code like a pro – PHPUnit in practice' tutorial”.

So we know now what the code is supposed to do, so let’s write our test first. The test file will be called *TutorialTest.php* and will be in *tests/Workshop* subdirectory:

```php
// tests/Workshop/TutorialTest.php
<?php
/**
 * PHPNW12 Workshop Basic unit test
 *
 * @author Sebastian Marek <proofek@gmail.com>
 */
namespace PhpNw12\Tests\Workshop;


use PhpNw12\Workshop\Tutorial;

/**
 * Tutorial class test case
 *
 * @author Sebastian Marek <proofek@gmail.com>
 */
class TutorialTest extends \PHPUnit_Framework_TestCase
{
	/**
	 * Very simple test verifying the welcome message text
	 */
	public function testGreetingsReturnWelcomeMessage()
	{
		$tutorial = new Tutorial();
		$result = $tutorial->greetings();

		$expectedMessage = "Hello everybody at 'Test your code like a pro – PHPUnit in practice' tutorial";
		$this->assertEquals($expectedMessage, $result);
	}
}
```

Now. let's run phpunit:

```
$ phpunit tests/Workshop/TutorialTest.php
PHPUnit 3.7.1 by Sebastian Bergmann.


Fatal error: Class 'PhpNw12\Workshop\Tutorial' not found in /Users/smarek/Google Drive/phpnw12-workshop/tests/Workshop/TutorialTest.php on line 24

Call Stack:
    0.0003     639072   1. {main}() /usr/local/bin/phpunit:0
    0.0047    1263248   2. PHPUnit_TextUI_Command::main() /usr/local/bin/phpunit:46
    0.0048    1264264   3. PHPUnit_TextUI_Command->run() /usr/local/share/pear/PHPUnit/TextUI/Command.php:130
    0.0307    4108560   4. PHPUnit_TextUI_TestRunner->doRun() /usr/local/share/pear/PHPUnit/TextUI/Command.php:177
    0.0361    4683816   5. PHPUnit_Framework_TestSuite->run() /usr/local/share/pear/PHPUnit/TextUI/TestRunner.php:325
    0.0363    4684648   6. PHPUnit_Framework_TestSuite->runTest() /usr/local/share/pear/PHPUnit/Framework/TestSuite.php:746
    0.0363    4684648   7. PHPUnit_Framework_TestCase->run() /usr/local/share/pear/PHPUnit/Framework/TestSuite.php:776
    0.0363    4684648   8. PHPUnit_Framework_TestResult->run() /usr/local/share/pear/PHPUnit/Framework/TestCase.php:770
    0.0376    4784808   9. PHPUnit_Framework_TestCase->runBare() /usr/local/share/pear/PHPUnit/Framework/TestResult.php:649
    0.0392    5027072  10. PHPUnit_Framework_TestCase->runTest() /usr/local/share/pear/PHPUnit/Framework/TestCase.php:825
    0.0392    5029200  11. ReflectionMethod->invokeArgs() /usr/local/share/pear/PHPUnit/Framework/TestCase.php:967
    0.0392    5029256  12. PhpNw12\Tests\Workshop\TutorialTest->testGreetingsReturnWelcomeMessage() /usr/local/share/pear/PHPUnit/Framework/TestCase.php:967
```

That’s obviously an expected behavior. We have not implement the class yet. Let’s do this then now.

### Implementing the code to make the test pass

We exactly know what the class name and method name needs to be:

```php
// src/Workshop/Tutorial.php
<?php
/**
 * PHPNW12 Workshop
 *
 * @author Sebastian Marek <proofek@gmail.com>
 */
namespace PhpNw12\Workshop;

/**
 * Tutorial class
 *
 * @author Sebastian Marek <proofek@gmail.com>
 */

class Tutorial
{
	/**
	 * Returns a greetings message
	 *
	 * @return string
	 */
	public function greetings()
	{
	}
}
```

We just need to include it in our test class:

```php
// tests/Workshop/TutorialTest.php
<?php
// (...)

namespace PhpNw12\Tests\Workshop;

require_once __DIR__ . '/../../src/Workshop/Tutorial.php';

use PhpNw12\Workshop\Tutorial;

// (...)
```

and re-run the test:

```
$ phpunit tests/Workshop/TutorialTest.php
PHPUnit 3.7.1 by Sebastian Bergmann.

F

Time: 0 seconds, Memory: 5.75Mb

There was 1 failure:

1) PhpNw12\Tests\Workshop\TutorialTest::testGreetingsReturnWelcomeMessage
Failed asserting that null matches expected 'Hello everybody at 'Test your code like a pro – PHPUnit in practice' tutorial'.

/Users/smarek/Google Drive/phpnw12-workshop/tests/Workshop/TutorialTest.php:29
/usr/local/bin/phpunit:46

FAILURES!
Tests: 1, Assertions: 1, Failures: 1.
```

Ha! Now we’re getting somewhere. The test is failing because our function returns nothing so far! Let’s change it then!

```php
// src/Workshop/Tutorial.php
<?php

// (...)

public function greetings()
{
	return "Hello everybody at 'Test your code like a pro – PHPUnit in practice' tutorial";
}

// (...)
```

and see if it works:

```
$ phpunit tests/Workshop/TutorialTest.php
PHPUnit 3.7.1 by Sebastian Bergmann.

.

Time: 0 seconds, Memory: 5.50Mb

OK (1 test, 1 assertion)
```

And here it is – our first successful test!

### Using @test annotation

There is another way of marking methods in your test case as tests, and this is with @test annotation. Let have a try.
This time we need a method that returns us a list of attendees. Let’s assume that the method’s name will be getAttendees().
As before we start with the test first:

```php
// tests/Workshop/TutorialTest.php
<?php
// (...)

/**
 * @test
 *
 * A test making sure getAttendees returns an array
 */
public function GetAttendeesReturnsListOfNames()
{
	$tutorial = new Tutorial();
	$result = $tutorial->getAttendees();
	$this->assertInternalType('array', $result);
}

// (...)
```

Obviously at this point in time we expect this test to fail (or rather crash) as the code (method) doesn’t exists yet:

```
$ phpunit tests/Workshop/TutorialTest.php
PHPUnit 3.7.1 by Sebastian Bergmann.

.
Fatal error: Call to undefined method PhpNw12\Workshop\Tutorial::getAttendees() in /Users/smarek/Google Drive/phpnw12-workshop/tests/Workshop/TutorialTest.php on line 40

Call Stack:
    0.0003     639072   1. {main}() /usr/local/bin/phpunit:0
    0.0046    1263248   2. PHPUnit_TextUI_Command::main() /usr/local/bin/phpunit:46
    0.0046    1264264   3. PHPUnit_TextUI_Command->run() /usr/local/share/pear/PHPUnit/TextUI/Command.php:130
    0.0355    4124320   4. PHPUnit_TextUI_TestRunner->doRun() /usr/local/share/pear/PHPUnit/TextUI/Command.php:177
    0.0388    4700024   5. PHPUnit_Framework_TestSuite->run() /usr/local/share/pear/PHPUnit/TextUI/TestRunner.php:325
    0.0453    5379000   6. PHPUnit_Framework_TestSuite->runTest() /usr/local/share/pear/PHPUnit/Framework/TestSuite.php:746
    0.0453    5379000   7. PHPUnit_Framework_TestCase->run() /usr/local/share/pear/PHPUnit/Framework/TestSuite.php:776
    0.0454    5379000   8. PHPUnit_Framework_TestResult->run() /usr/local/share/pear/PHPUnit/Framework/TestCase.php:770
    0.0454    5380536   9. PHPUnit_Framework_TestCase->runBare() /usr/local/share/pear/PHPUnit/Framework/TestResult.php:649
    0.0457    5436456  10. PHPUnit_Framework_TestCase->runTest() /usr/local/share/pear/PHPUnit/Framework/TestCase.php:825
    0.0458    5438576  11. ReflectionMethod->invokeArgs() /usr/local/share/pear/PHPUnit/Framework/TestCase.php:967
    0.0458    5438632  12. PhpNw12\Tests\Workshop\TutorialTest->GetAttendeesReturnsListOfNames() /usr/local/share/pear/PHPUnit/Framework/TestCase.php:967
```

Let’s fix it quickly, and for a time being create this method and make it return an empty array:

```php
// src/Workshop/Tutorial.php
<?php

// (...)

/**
 * Returns a list of tutorial attendees
 *
 * @return array
 */
public function getAttendees()
{
	return array();
}

// (...)
```

And re-run the tests:

```
$ phpunit tests/Workshop/TutorialTest.php
PHPUnit 3.7.1 by Sebastian Bergmann.

..

Time: 0 seconds, Memory: 5.50Mb

OK (2 tests, 2 assertions)
```

Exercise 2: Fixing the first failure
------------------------------------

Let’s work on the *getAttendees()* method, as it doesn’t return anything yet. 
We will se the power of unit tests when used during refactoring. What we want to do first is to return some real data –
we will create a private property called *$_attendees* that will be used as the value returned by *getAttendees()* method.

```php
// src/Workshop/Tutorial.php
<?php

// (...)

/**
 * List of people attending the tutorial
 *
 * @var array
 */
private $_attendees;

// (...)

/**
 * Returns a list of tutorial attendees
 *
 * @return array
 */
public function getAttendees()
{
	return $this->_attendees;
}

// (...)
```

And re-run the tests to see whether we broke anything:

```
$ phpunit tests/Workshop/TutorialTest.php
PHPUnit 3.7.1 by Sebastian Bergmann.

.F

Time: 0 seconds, Memory: 5.75Mb

There was 1 failure:

1) PhpNw12\Tests\Workshop\TutorialTest::GetAttendeesReturnsListOfNames
Failed asserting that null is of type "array".

/Users/smarek/Google Drive/phpnw12-workshop/tests/Workshop/TutorialTest.php:41
/usr/local/bin/phpunit:46

FAILURES!
Tests: 2, Assertions: 2, Failures: 1.
```

Well, that obviously makes sense, our test is making sure that *getAttendees()* method always returns an array. As we have modified the code and now return uninitialized variable (which by default is null) we get the error.
So, let’s fix it then. It seems like just initializing the property in the constructor should solve the problem. In fact, if we make the constructor accept an array of attendees we will be able then to return some data without actually hardcoding it in the *getAttendees()* method.

```php
// src/Workshop/Tutorial.php
<?php

// (...)

/**
 * Constructor initiates the list of attendees
 *
 * @return void
 */
public function __construct(array $attendees)
{
	$this->_attendees = $attendees;
}

// (...)
```

And checking the tests again:

```
$ phpunit tests/Workshop/TutorialTest.php
PHPUnit 3.7.1 by Sebastian Bergmann.

EE

Time: 0 seconds, Memory: 5.25Mb

There were 2 errors:

1) PhpNw12\Tests\Workshop\TutorialTest::testGreetingsReturnWelcomeMessage
Argument 1 passed to PhpNw12\Workshop\Tutorial::__construct() must be an array, none given, called in /Users/smarek/Google Drive/phpnw12-workshop/tests/Workshop/TutorialTest.php on line 25 and defined

/Users/smarek/Google Drive/phpnw12-workshop/src/Workshop/Tutorial.php:30
/Users/smarek/Google Drive/phpnw12-workshop/tests/Workshop/TutorialTest.php:25
/usr/local/bin/phpunit:46

2) PhpNw12\Tests\Workshop\TutorialTest::GetAttendeesReturnsListOfNames
Argument 1 passed to PhpNw12\Workshop\Tutorial::__construct() must be an array, none given, called in /Users/smarek/Google Drive/phpnw12-workshop/tests/Workshop/TutorialTest.php on line 39 and defined

/Users/smarek/Google Drive/phpnw12-workshop/src/Workshop/Tutorial.php:30
/Users/smarek/Google Drive/phpnw12-workshop/tests/Workshop/TutorialTest.php:39
/usr/local/bin/phpunit:46

FAILURES!
Tests: 2, Assertions: 0, Errors: 2.
```

Now we broke both of the tests, because we have changed the constructor definition (by simply introducing it), but have not changed the tests itself to reflect it!

We have to options here, either changing all the tests an initiate the attendees input array, or change the constructor and re-define the input array so if it’s not supplied it will default to an empty array. We will go for the second option.

Well done, finally green again:

```
$ phpunit tests/Workshop/TutorialTest.php
PHPUnit 3.7.1 by Sebastian Bergmann.

..

Time: 0 seconds, Memory: 5.50Mb

OK (2 tests, 2 assertions)
```

Exercise 3: PHPUnit CLI options
-------------------------------

PHPUnit experience can be greatly enhanced with some of the command line options. Full documentation is available in [PHPUnit manual](http://www.phpunit.de/manual/current/en/textui.html#textui.clioptions)

### --colors

Default PHPUnit output is quite dull. You can greatly enhance your experience with colors. *--colors* options react to what happens during runtime, and uses colors to get your attention. Green for passing tests, yellow for incomplete,skipped or general warnings and red when something goes horribly wrong.

```
$ phpunit --colors tests/Workshop/TutorialTest.php 
PHPUnit 3.7.1 by Sebastian Bergmann.

..

Time: 1 second, Memory: 5.50Mb

OK (2 tests, 2 assertions)
```

### --testdox

You can also make PHPUnit speak to you with a bit more english language and make it to self document the tests itself. This option basically converts function names written in camelcase convention into sentences. So a method called *testNoMoreThenTenAttendeedCanBeInTheRoom* is being displayed as *No more then ten attendeed can be in the room*

```
$ phpunit --testdox tests/Workshop/TutorialTest.php
PHPUnit 3.7.1 by Sebastian Bergmann.

PhpNw12\Tests\Workshop\Tutorial
 [x] Greetings return welcome message
 [x] Get attendees returns list of names
```

### --debug

In debug mode PHPUnit outputs a bit more information during runtime to show you what executes and in what order. It can be a really useful option when it comes to diagnosing problems with your tests.

```
$ phpunit --debug tests/Workshop/TutorialTest.php
PHPUnit 3.7.1 by Sebastian Bergmann.


Starting test 'PhpNw12\Tests\Workshop\TutorialTest::testGreetingsReturnWelcomeMessage'.
.
Starting test 'PhpNw12\Tests\Workshop\TutorialTest::GetAttendeesReturnsListOfNames'.
.

Time: 0 seconds, Memory: 5.50Mb

OK (2 tests, 2 assertions)
```

### --filter

If you are working on a specific test, or you have a big test suite and for whetever reason you don't want to run the whole suite you can use *--filter* option to run tests which name matches given pattern. The pattern can be either the name of a  test or a regular expression that matches multiple test names.

```
$ phpunit --filter=testGreetingsReturnWelcomeMessage tests/Workshop/TutorialTest.php
PHPUnit 3.7.1 by Sebastian Bergmann.

.

Time: 1 second, Memory: 5.50Mb

OK (1 test, 1 assertion)
```

Exercise 4: More assertions
---------------------------

There is a big set of asserts that you can use. Using the right one for the job will make your life easier and also will self document the code. Tests can grow with time and can become quite nasty and hard to maintain if not written right. It is important to be aware of the available asserts and use the one that suits the given situation the best. 

Full list of asserts is documented in PHPUnit manual. See http://www.phpunit.de/manual/current/en/writing-tests-forphpunit.html#writing-tests-for-phpunit.assertions for more details.

### testing booleans with assertFalse() and assertTrue()

Let’s start with testing boolean values. Being able to add attendees to the tutorial we will most probably need a way to find whether we can more attendees to it without breaking the room capacity. So a function called *arePlacesLeft()* would be really useful. Let’s assume for now that maximum room capacity is 3 and write first test that will make sure that if we have more then 3 people in the room there is no more places left. Using *assertFalse()* and *assertTrue()* will help us out here.

```php
// tests/Workshop/TutorialTest.php
<?php
// (...)

/**
 * Checks that if there are more then 3 attendees there are no places left
 */
public function testTutorialHasNoPlacesLeftWhenMoreThen3Attendees()
{
	$attendees = array(
		"Sebastian Marek",
		"Tom",
		"Martha",
		"John"
	);
	
	$tutorial = new Tutorial($attendees);
	$this->assertFalse($tutorial->arePlacesLeft());
}

// (...)
```

The test suite will obviously fail at this moment in time as we have not written the method yet.

```
$ phpunit tests/Workshop/TutorialTest.php
PHPUnit 3.7.1 by Sebastian Bergmann.

..
Fatal error: Call to undefined method PhpNw12\Workshop\Tutorial::arePlacesLeft() in /Users/smarek/Google Drive/phpnw12-workshop/tests/Workshop/TutorialTest.php on line 57

Call Stack:
    0.0004     639072   1. {main}() /usr/local/bin/phpunit:0
    0.2050    1263248   2. PHPUnit_TextUI_Command::main() /usr/local/bin/phpunit:46
    0.2050    1264264   3. PHPUnit_TextUI_Command->run() /usr/local/share/pear/PHPUnit/TextUI/Command.php:130
    0.3670    4141536   4. PHPUnit_TextUI_TestRunner->doRun() /usr/local/share/pear/PHPUnit/TextUI/Command.php:177
    0.3738    4717360   5. PHPUnit_Framework_TestSuite->run() /usr/local/share/pear/PHPUnit/TextUI/TestRunner.php:325
    0.4457    5437952   6. PHPUnit_Framework_TestSuite->runTest() /usr/local/share/pear/PHPUnit/Framework/TestSuite.php:746
    0.4457    5437952   7. PHPUnit_Framework_TestCase->run() /usr/local/share/pear/PHPUnit/Framework/TestSuite.php:776
    0.4457    5437952   8. PHPUnit_Framework_TestResult->run() /usr/local/share/pear/PHPUnit/Framework/TestCase.php:770
    0.4458    5439488   9. PHPUnit_Framework_TestCase->runBare() /usr/local/share/pear/PHPUnit/Framework/TestResult.php:649
    0.4461    5495408  10. PHPUnit_Framework_TestCase->runTest() /usr/local/share/pear/PHPUnit/Framework/TestCase.php:825
    0.4461    5497552  11. ReflectionMethod->invokeArgs() /usr/local/share/pear/PHPUnit/Framework/TestCase.php:967
    0.4461    5497608  12. PhpNw12\Tests\Workshop\TutorialTest->testTutorialHasNoPlacesLeftWhenMoreThen3Attendees() /usr/local/share/pear/PHPUnit/Framework/TestCase.php:967
```

So let’s add the code then:

```php
// src/Workshop/Tutorial.php
<?php

// (...)

/**
 * Are there any more places left for the tutorial
 *
 * @return boolean
 */
public function arePlacesLeft()
{
	return (count($this->getAttendees()) <= 3);
}

// (...)
```

And run the tests again:

```
$ phpunit tests/Workshop/TutorialTest.php
PHPUnit 3.7.1 by Sebastian Bergmann.

...

Time: 0 seconds, Memory: 5.50Mb

OK (3 tests, 3 assertions)
```

Bear in mind it was only one possible scenario for this method, we still need to make sure that if we have less then 3 attendees there are places left in the tutorial:

```php
// tests/Workshop/TutorialTest.php
<?php
// (...)

/**
 * Checks that if there are less then 3 attendees there are still some places left
 */
public function testTutorialHasPlacesLeftWhenLessThen3Attendees()
{
	$attendees = array(
		"Sebastian Marek"
	);

	$tutorial = new Tutorial($attendees);
	$this->assertTrue($tutorial->arePlacesLeft());
}

// (...)
```

```
$ phpunit tests/Workshop/TutorialTest.php
PHPUnit 3.7.1 by Sebastian Bergmann.

....

Time: 1 second, Memory: 5.50Mb

OK (4 tests, 4 assertions)
```
