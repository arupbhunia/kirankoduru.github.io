---
layout: post
title:  "Writing unit tests in python"
date:   2015-07-29 22:02:48
categories: python
description : Learn to write a unit test in python
---
A good developer always tests his code. And python being a robust language is very easy to test. Heres a function that adds two numbers and returns their result.


{% highlight python %}
# filename: sum_num.py
def sum(x, y):
    return x + y
{% endhighlight %}

Now here is a test case that will test if your function returns the excepted result:

{% highlight python %}
from unittest import TestCase

class TestSum(TestCase):
    
    def test_sum(self):
        from sum_num import sum

        # first store the expected result in a variable
        result = sum(3, 4)

        # check if the result is equal to expected result
        # here the result should be equal to 7.
        self.assertEquals(7, result)
{% endhighlight %}

Thats it ! 

#### Running your tests
Install the `nose2` python package using __pip__ 

```
pip install nose2
```

This is how my directory is structured:

<pre>
.
├── sum_num.py
└── tests
   ├── __init__.py
   └── test_sum_num.py
</pre>

Now when you are in the root directory and run __nose2__ using the command line (command is `nose2`), it will check for all the files inside the __tests/__ directory and look for test cases that have file names that contain in it __test__ and have class names that have the name __Test__ and method names __test__. So this is how it will look like:

```
tests/ ---> test_sum_num.py ---> class TestSum ---> method test_sum()
```

So this is a short run down of how to start running test cases in your projects. You can checkout the official list of assert methods that the `unittest.TestsCase` class provides [here](https://docs.python.org/2/library/unittest.html#assert-methods).
