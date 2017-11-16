---
layout: post
title:  "Mocking with Python - Part II"
date:   2017-11-02 00:34:01
categories: python
description : Learning to mock in python
author: Kiran Koduru
image: /img/hare2.png
---
This is part 2 of my mocking with python blog series. If you haven't read the first part or have, and would like to skim over the basics, then please go ahead and [read it]({% post_url 2017-04-05-mocking-with-python-part-1 %}) (again). Both articles aren't in any way an expert guide but my personal quest on learning python mocking. I'll try my best to convey my thoughts and try to cover all topics related to Python [mock](https://docs.python.org/3.6/library/unittest.mock.html).

Mocking helps me runs local tests quickly. Another place where mocking helps is in continuous integration environments. When a whole team of developers that work on a same project, you want to quickly verify the new changes look okay. Mocking allows for faster test run times on CI servers as well. I wouldn't consider this as a replacement for running integration test but a first pass on testing with mocks is ok.

### Are my mocks called?

When testing with mocks, how do I know if my mocked object is being called? The python mocking library comes with `assert` methods that will help you run test mocked objects. I'll highlight the `called` and `assert_called_with` attributes with an example below. But you can also use similar logic for `assert_called`, `assert_called_once`, `assert_called_once_with`, `assert_any_call` and `assert_has_calls` methods.

Let's test with a function called `sum_numbers_add_random_int`, where I add a `random` integer to the sum of `list` of numbers. This function randomly selects a number between 1 and 100 to add to the sum of numbers. For example, I have a `list` of numbers `[1, 2, 3]` whose sum is `6`. I will randomly add an integer between __1-100__ to `6`, which could result into any number from __7-106__ being returned by `sum_numbers_add_random_int`.

{% highlight python %}
# random_sum.py
import random

def sum_numbers_add_random_int(nums=None):
    total = sum(nums)

    # adding randomness
    total += random.randint(1, 100)
    return total
{% endhighlight %}

To test the __random_sum.py__ file I will mock the `random` library's `randint` function. I do this cause I can't predict the return value for `randint`. I replace the `return_value` for `randint` with __1__, __2__ and __3__. This sould return a sum of __34__, __35__ and __36__ when the `sum_numbers_add_random_int` function is called.

{% highlight python %}
# test_random_sum.py
import random
from unittest import TestCase, main
from unittest.mock import patch
from random_sum import sum_numbers_add_random_int


class TestApp(TestCase):

    @patch('random.randint')
    def test_random_sum(self, mocked_randint):
        nums = [10, 11, 12]
        mocked_randint.return_value = 1 # making randint builtin return integer 1
        self.assertEqual(sum_numbers_add_random_int(nums), 34)
        self.assertTrue(mocked_randint.called)
        mocked_randint.assert_called_with(1, 100)

        mocked_randint.return_value = 2 # making randint builtin return integer 2
        self.assertEqual(sum_numbers_add_random_int(nums), 35)
        self.assertTrue(mocked_randint.called)
        mocked_randint.assert_called_with(1, 100)

        mocked_randint.return_value = 3 # making randint builtin return integer 3
        self.assertEqual(sum_numbers_add_random_int(nums), 36)
        self.assertTrue(mocked_randint.called)
        mocked_randint.assert_called_with(1, 100)

if __name__ == '__main__':
    main()
{% endhighlight %}

### Mocking Functions

In the same way I can patch functions in python as well. I will demonstrate with two functions `add_two_numbers` and `print_text` in a file __adding_two_nums__. The `add_two_numbers` returns the sum of it's parameters `number1` and `number2` but prints the sum before returning it.

{% highlight python %}
# adding_two_nums.py

def print_text(text):
    print(text)

def add_two_numbers(number1, number2):
    total = number1 + number2
    print_text(total)
    return total
{% endhighlight %}

In my test case I patch the `print_text` method, whose only purpose in life is to print any given text. You could use this idea to patch any function's return value with mock. Just make sure to test if they are called correctly.

{% highlight python %}
# test_adding_two_nums.py

from unittest import TestCase, main
from unittest.mock import patch
from adding_two_nums import add_two_numbers

class TestFunctions(TestCase):

    @patch('random_sum.print_text')
    def test_add_two_numbers(self, mocked_print_text):
        self.assertEqual(add_two_numbers(1, 2), 3)

        # making sure mocked print_text function is called properly
        self.assertTrue(mocked_print_text.called)
        mocked_print_text.assert_called_with(3)

if __name__ == '__main__':
    main()
{% endhighlight %}

Hope this post gave you an idea on how and where to mock your functions. In my next post on mocking with python series I will discuss how to mock/test exceptions, iterables and other side effects. If you'd like to stay updated, do signup to my newsletter.
