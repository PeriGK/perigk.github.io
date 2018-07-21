+++
author = "Periklis Gkolias"
categories = ["python", "mock", "tutorial", "programming", "python3", "unittest"]
date = 2018-01-30
description = ""
draft = false
slug = "a-comprehensive-introduction-to-unit-testing-and-mocking-with-python3"
tags = ["python", "mock", "tutorial", "programming", "python3", "unittest"]
title = "A1 comprehensive introduction to unit-testing and mocking with Python3"
+++

Unit testing is quickly becoming a must for all job adverts. There are a few people who don't like Test Driven Development(TDD), but at least they agree on the value, automation testing(not only unit tests) add to the overall code quality and to the debugging process.

By the time of writing, according to [dice.com](https://www.dice.com/skills/Test-driven%20development.html), TDD is enjoying increased popularity year after year.

Without further ado, let's start our journey. You can download the source code from [here](https://github.com/PeriGK/filesystem_handling/)


## A small background on unit tests

Suppose we have a new functionality, which implements a shopping cart, which we want to test. There are two big families of test methodologies, that we can use. The automated and the non-automated ones.

If we choose a non-automated way, we can use for example our mouse or keyboard to add some products to the cart and verify the functionality 'as a user'. And that was the most popular way to test software for a very long time. But as new and exciting test frameworks were being created, people have started shifting their attention towards automated test methodologies.

The automated way requires the developer to write some bytes of code, which test the (business)logic. Given the shopping cart, in the previous paragraph, instead of clicking here and there, we would use code to make this happen automatically. It is like test scenarios executed by the machine, instead of a human.

##Get dirty(with code) ##

In the example code above, I have written a small utility which does few basic operations on the filesystem. Let's dive into the source code and the relevant test cases, to understand the unit testing better.

```python
import os
def create_file(target_directory, new_file_name):
    if check_dir_exists(target_directory) and not check_file_exists(new_file_name): 
        new_file_name = os.path.join(target_directory, new_file_name)
        with open(new_file_name, 'w') as f:
            f.write('')
        return True
    return False
```

In the function above, we want to create a new file. In this function, we provide the full path of the target file in two pieces, the directory it will live in and its filename. We check if the directory is there but we don't want the file to exist already. If we satisfy both, we create the file.

Before moving to the unit test code, for this function, let's explain a bit what is mocking.

## Mocking ##

How do `check_dir_exists` and `check_file_exists` work? That's the nice thing with unit testing. You don't need to know. All you need to know is, how it is called and what is the return type, in order to force the desired return value. This process is called mocking. So, instead of an actual invocation of a method/object, you use a dummy method/object, ready to be customized according to your needs.

I like to think mocking as the art of not giving a f*\*k  about 'calls to outsider code'.

> My code depends on an external service, thus I cannot unit test it.

Mock the external call and return the expected value, according to your test scenario. If the call is supposed to put some data in the database, based on an API response, return a JSON/XML/whatever response that indicates success. The respective API docs are your best friend here.

> The function you asked me to unit test is deleting an entire partition in the disk.

That's fine(almost). You can mock the system call(s) and verify the correct sequence of them is invoked.

>  I cannot test this code, as it uses builtin function and third-party function.

Yeap, you guessed it. Mock the aforementioned involved functions.

As you can see, the fewer dependencies you have on your code, the easier the unit test is written. Unfortunately, this is easier said than done.

In our case, we will be using the mock module of the unittest library to achieve such effects.

## How to write unit tests in python - the boilerplate part. ##

A unit test in python, is a function, inside a class which inherits from `unittest.TestCase`. You may hear those called *Test Suites* but for the scope of the article we will call them `TestCases`. Each TestCase has some standard helper methods that move through the unit testing process.

Below you can see an example of the two most common ones. `setUp` and `tearDown`. As explained in the comments(and maybe it is self-explanatory from their names) `setUp ` is initializing values required for a unit test and is running before the execution of the test(hello captain obvious). `tearDown` does the opposite, it cleans up stuff and runs after the execution of the unit test.

```python
import unittest
from unittest import mock
from fs_handler_main import *
class FileSystemHandlerTest(unittest.TestCase):
    def setUp(self):
        """setUp is run before each test case. 
        It is usually used to  setup(wow) some common
        values for all test cases"""
        print('setUp: Initialize instance variables')
        self.directory = '/home/periklis/'
        self.new_filename = 'dummy_new_filename.txt'

        # Fun fact: The os module we are using is 
        # indirectly imported from fs_handler_main and not
        # from the python library
        self.full_path = os.path.join(self.directory, 
        self.new_filename)
    def tearDown(self):
        # This is something not very useful, it is only written
        # this way to demonstrate how tearDown works
        print('tearDown: Reset instance variables')
        self.directory = ''
        self.new_filename = ''
```


## How to write unit tests in python - unit test the business logic. ##

For the purpose of this article, I have written three unit tests for the `create_file` function seen above. Remember you can write as many test scenarios as you want and find useful. Sky in the limit or as Buzz would say, [to infinite unit tests and beyond](https://www.youtube.com/watch?v=ejwrxGs_Y_I)

```python
    @mock.patch('builtins.open', mock=mock.mock_open)
    @mock.patch('fs_handler_main.check_file_exists')
    @mock.patch('fs_handler_main.check_dir_exists')
    def test_create_file_success(self, mock_check_dir_exists, 
    mock_check_file_exists, mock_open_func):
        print('test_create_file_success')
        mock_check_dir_exists.return_value = True
        mock_check_file_exists.return_value = False
        self.assertTrue(create_file(self.directory,
        self.new_filename))

    @mock.patch('builtins.open', mock=mock.mock_open)
    @mock.patch('fs_handler_main.check_file_exists')
    @mock.patch('fs_handler_main.check_dir_exists')
    def test_create_file_failure_dir_no_exists(self, mock_check_dir_exists, mock_check_file_exists, mock_open_func):
        print('test_create_file_failure_dir_no_exists')
        mock_check_dir_exists.return_value = False
        mock_check_file_exists.return_value = False
        self.assertFalse(create_file(self.directory, self.new_filename))

    @mock.patch('builtins.open', mock=mock.mock_open)
    @mock.patch('fs_handler_main.check_file_exists')
    @mock.patch('fs_handler_main.check_dir_exists')
    def test_create_file_failure_file_already_exists(self, mock_check_dir_exists, mock_check_file_exists, mock_open_func):
        print('test_create_file_failure_file_already_exists')
        mock_check_dir_exists.return_value = False
        mock_check_file_exists.return_value = False
        self.assertFalse(create_file(self.directory, self.new_filename))
```

Have you noticed the test function starts with `test_`? That's because python applies a regular expression, in order to separate the actual test cases from helper function in a unit test file.

Regarding the tests above:
* The first unit test is checking the normal case, where the file is created smoothly. Thus the business logic function is returning `True`, value which we assert.
* The second one is trying to create the new file, but the directory is not there. So it is asserting the file will not be created, by checking the return value of `create_file` is `False`.
* Similarly, the third unit test, is checking if the file already exists and in such case, it is not creating it.

Are you wondering why `mock_open_func`, which is the mock object created from ` @mock.patch('builtins.open', mock=mock.mock_open)` is not used? We only mocked this part, so that an actual file is NOT created in every test run. We will discuss more later.

## Decorators ##
But what is this '@' thingy on top of the declaration of each function? I am about to write an article for decorators, as they cannot be explained briefly in a few lines, but for now, you only need to know, that they are functions, which take other functions as arguments and return those arguments extended.

So to be specific, `@mock.patch(fs_handler_main.check_file_exists)` on top of `test_create_file_success` means:
> Locate the function `check_file_exists` from `fs_handler_main` and as long we are in the scope of the function below(namely `fs_handler_main`) whenever `check_file_exists` is invoked, use a mock object called `mock_check_file_exists` instead.

This is the same for python modules too. For example, if you want to mock `os` or `math` you need to provide a full path, starting from the code under test module(in our case `fs_handler_main`)

## Moar mocking and decorators ##
Let's dive more into decorators and mocking for unit testing and examine the following pair of business logic-unit test code.

```python
def delete_file(full_path_to_file):
    if os.path.isfile(full_path_to_file):
        os.remove(full_path_to_file)

    # We mock the fs_handler_main.os.* modules, if we mock the os provided from python directly, it will not work
    @mock.patch('fs_handler_main.os.path.isfile')
    @mock.patch('fs_handler_main.os.remove')
    def test_delete_file_success(self, mock_os_remove, mock_os_is_file):
        print('test_delete_file_success')
        mock_os_is_file.return_value = True
        delete_file(self.full_path)
        mock_os_remove.assert_called_with(self.full_path)
```

 In the core logic function, we check if the given full path is actually a file and in such case we delete it.

As we use the os module of python, we need to mock it as well and this is considered a best industry practice. **Not that it won't work otherwise.** But there are too many unnecessary things to take care of, in such case, namely:
* Make sure you have permissions to read/write in the directory provided as an argument.
* Make sure you have the target file created, in a previous piece of code(maybe a helper function).
* Make sure the disk has sufficient space to write, even if the all the rest prerequisites are satisfied.

As explained above, we need to mock the os modules provided from fs_handler_main file and not the original ones(the functionality is the same, **it is just a matter of how python handles namespacing**).

The important thing here is line:
```mock_os_is_file.return_value = False```

Our business logic is relying on the return value of `os.is_file`. So, first, we mock it and then we customize it, by altering the return value, according to the needs of our scenario.

## Things worth remembering  ##

Thank you for reading this article. Below you can find a few points I find worth reiterating.

* The methods that are test scenarios start with `test_` to help python discover them easily.
* The fewer dependencies you have, the easier the unit test is written.
* `mock` lives in unittest after python3.4.6. But this was not always the case. So check with the official documentation if you need to download the `mock ` library from [PyPi](https://pypi.python.org/pypi) or any other workaround that might be in effect back then.
* Mock things using the module-under-test(the business logic) namespace, not the python ones.


## Further reading ##
* [Python docs](https://docs.python.org/3/library/unittest.html?highlight=unittest#module-unittest)
* [Testing Python: Applying Unit Testing, TDD, BDD and Acceptance Testing](https://www.amazon.co.uk/gp/product/1118901223/ref=as_li_tl?ie=UTF8&camp=1634&creative=6738&creativeASIN=1118901223&linkCode=as2&tag=bitperi-21&linkId=e6220e748c81612f7f772a32d236e5b9)
* [Nose framework](http://nose.readthedocs.io/en/latest/)