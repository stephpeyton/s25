---
num: "Lecture 5"
desc: ""
ready: false
lecture_date: 2025-04-17 15:30:00.00-7:00
---

* [Slides folder]({{ site.slides_url }}){:target="_blank"}

---
# Questions
* Does overloading and overwriting mean the same thing?
    - In this class, yes.
    - In Python, you can simulate overloading a function to work with different parameters, but redefining a function with the same name will overwrite the original function.

* Why should we perform tests?
    - In order to make sure our code runs properly and that it produces the output we want it to.
    - Writing tests concurrently allows us to define the expected behavior of our code and better understand what the behavior is supposed to be.
    - We can catch our bugs earlier if we write tests along with our code.
    - Tests allow us to run those changes automatically to verify we did not disrupt or break any other parts of our code.

* What about py-test?
    - Py-test tells us all of the different tests that failed instead of just stopping at the first failed assert statement.

### Square and Rectangle Example

# Creating Square, a subclass of Rectangle
class Square(Rectangle):
    def __init__(self, side):
        # equivalent to calling Rectangle.__init__(self, side, side)
        super().__init__(side, side)

# Rectangle parent class for context
class Rectangle:
    """A class that describes the properties of a rectangle"""
    def __init__(self, width, height):
        self.width = width
        self.height = height

# Explanations
* Side is not an attribute of the Rectangle class.
* It is a parameter that we are using to create the Rectangle, by passing the same value for both width and height.
* As a constructor, when creating a Square, we are calling the parent Rectangle's constructor and setting:

        self.width = side
        self.height = side

through super().__init__(side, side)

Have we defined what side is set to?
* Yes, when the Square object is instantiated, the value for side is passed in by the user.

Could a child have more than one parent class?
* It is possible for a child class to have more than one parent class through multiple inheritance.
* For example, a square could inherit from both a Rectangle class (for right angles) and a Rhombus class (for equal side lengths).
* This way, the Square class combines properties from both parent shapes, accurately modeling what a square is.

# Notes:
* Since we have already defined the value for the width and height, self.width and self.height are both set during object construction using the side parameter.
* When we extend the parent class (Rectangle), we inherit everything that the parent class has, including its attributes (width, height) and methods.
* We can use them as is, or override them as needed.
* Even though the Square class calls the Rectangle class constructor using super().__init__(), the Square class is not stored inside the Rectangle class.
* Square and Rectangle are two separate classes, stored separately in memory.
* However, because Square inherits from Rectangle, there is a link between them where the child class (Square) has access to everything that the parent class (Rectangle) has.
* The Square class and the Rectangle class are defined independently at the same indentation level and they are stored separately as individual classes.
* Inheritance connection that allows the child class to use and extend the properties and behavior of the parent class.
  

# In-class Instructions

Let's create a `Drink` base class as well as defining specific classes for a couple types of Drinks (`Tea` and `Juice`). The `DrinkOrder` class will organize Drinks and will provide a summary of a specific drink order.

In addition to defining classes for various Drinks and a Drink Order, you will test your code for correctness by unit testing various scenarios using `pytest`.

You will need to create five files:
* `Drink.py` - file containing a class definition with attributes all Drinks have.
* `Tea.py` - file containing a class definition of a tea beverage that inherits from the `Drink` class.
* `Juice.py` - file containing a class definition of a juice beverage that inherits from the `Drink` class.
* `DrinkOrder.py` - file containing a class definition of a customer's drink order containing various beverages.
* `testFile.py` - file containing `pytest` functions testing the `Drink`, `Tea`, `Juice`, and `DrinkOrder` classes.

For testing, you will create the `TestDrink` class in the `testFile.py`, so that you can write the tests as you are implementing the class and its methods.


<a href="#" id="drinkclass"></a>
## `Drink` class

The `Drink.py` file will contain the class definition of a general beverage.

We will define this class' attributes as follows:

* `size` - a `str` that represents the size of the beverage (`'small'`, `'medium'`, `'large'`).
* `price` - positive `float` that represents the price of the beverage.

You should write a constructor that passes in values for all the fields. You may assume calls to the constructor will always contain a non-empty `str` representing the beverage's size and a positive `float` representing the beverage's price.

* `__init__(self, size, price)`

In addition to your constructor, your class definition should also support "setter" and "getter" methods that can update and retrieve the state of the Drink objects:

* `get_size(self)` - returns the size of the beverage
* `get_price(self)` - returns the price of the beverage
* `update_size(self, new_size)` - updates the size of the beverage
* `update_price(self, new_price)` - updates the price of the beverage

Each Drink object should be able to call a method `info(self)` that you will implement, which **returns** a `str` with the current beverage's size and price. Since there are many beverages, the following output represents what will be returned if we call the `info` method after constructing a `Drink` object:

```python
bev1 = Drink('medium', 20.5)
print(bev1.info())
```

<b>Output:</b>

```
medium: $20.50
```

<b>Note:</b> The `bev1.info()` return value in the example above does not contain a newline character (`\n`) at the end.

**Note:** The quotation marks around the **returned string** in IDLE shell tell us that the value was returned, _not_ printed, hence the string representation is shown.

<b>Hint:</b> Note that the return string should contain a price with two decimal places (as traditionally used when displaying prices). Use the f-string to show the floating point values with 2 decimal places. For example:

```
>>> price = 5
>>> f"${price:.2f}"
'$5.00'
```

### Template for the `Drink` and `TestDrink` classes

Below is the skeleton template for the `Drink` class that you can use as a starting point for your `Drink.py` file:

```python
class Drink:
    def __init__(self):
        pass

    def get_size(self):
        pass

    def get_price(self):
        pass

    def update_size(self):
        pass

    def update_price(self):
        pass

    def info(self):
        pass
```

Immediately, we can add the corresponding test class and its testing methods to the `testFile.py` like so:

```python
from Drink import Drink

class TestDrink:
    def test_init(self):
        pass

    def test_get_size(self):
        pass

    def test_get_price(self):
        pass

    def test_update_size(self):
        pass

    def test_update_price(self):
        pass

    def test_info(self):
        pass
```

The way the `TestDrink` template was created:
- copy the stubs for the Drink class directly
- add the corresponding `import` statement at the top of the file
- change the name of the class from `Drink` to `TestDrink`
- change the `__init__` method to be `test_init`
- prepend `test_` to all the other methods

### Write tests for the `TestDrink` class

Now, inside each test function in `testFile.py`, we test each class’s methods using `assert` statements. 
- If the method has a return value, directly assert the return value to verify its correctness; 
- if the method doesn’t have a return value, combine it with another method that has a return value to do the testing.

For example, we can use the example that we saw above, to create a sample Drink object and test that it was correctly created:

```py

from Drink import Drink

class TestDrink:
    def test_init(self):
        drink = Drink('medium', 20.5)
        assert drink.size == 'medium'
        assert drink.price == 20.5
```

Continue in this way to test the rest of the methods of the class:

```py
    def test_get_size(self):
        drink = Drink('large', 20.95)
        assert drink.get_size() == 'large'
```

Before submitting your code to Gradescope, run your `testFile.py` using pytest to verify that all your tests are correct and are passing.

---

You are now ready to implement the rest of the classes and add their tests to `testFile.py`.

<a href="#" id="teaclass"></a>
## `Tea` class

The `Tea.py` file will contain the class definition of a tea drink. Since a tea **IS-A** drink, it should inherit the values we defined in the `Drink` class.

**HINT: Think of the `super` keyword discussed in the lecture**

Your `Tea` class definition should support the following constructor and method:

* `__init__(self, size, price, style)` - constructor that extends the parent class' (`Drink`) constructor and sets the style of tea (for example, Camomile, Mint, English Breakfast, etc.) as an attribute to the `Tea` class. 
  - Note, you may assume the `style` parameter is a `str`. 
  - In order to avoid code duplication, **you must explicitly utilize the base class' constructor to set the size and price attributes.**
* `info(self)` - method should override the inherited `info()` method in the `Drink` class, and returns a `str` with the properties of a `Tea` object. 
  - In order to avoid code duplication, **you must explicitly utilize the base class' `info()` method to construct the `Tea` object's information**. 
  - An example of what the return string format of the `info()` method is shown below:

```
>>> drink1 = Tea('small', 3.0, "Camomile")
>>> drink1.info()
'Camomile Tea, small: $3.00'
```

<b>Note:</b> The `drink1.info()` return value in the example above does not contain a newline character (`\n`) at the end.

**Note:** The quotation marks around the **returned string** in IDLE tell us that the value was returned, _not_ printed, hence the string representation is shown.

---

<a href="#" id="juiceclass"></a>
## `Juice` class

The `Juice.py` file will contain the class definition of what a juice drink will have. Since a juice **IS-A** drink, it should inherit the values we defined in the `Drink` class.

Your `Juice` class definition should support the following constructor and method:

* `__init__(self, size, price, ingredients)` - constructor that extends the parent class' (`Drink`) constructor and sets a list of ingredients used in this juice object. 
  - Note, you may assume the `ingredients` parameter is a list of strings representing the types of ingredients (for example, `"tomato"`, `"orange"`, `"blueberry"`, `"guava"`, etc.) used in the juice. 
  - In order to avoid code duplication, **you must explicitly utilize the base class' constructor to set the size and price attributes.**
* `info(self)` - method that overrides the inherited `info` method in the `Drink` class, and returns a `str` with the properties of a `Juice` object.  - In order to avoid code duplication, **you must explicitly utilize the bass class `info()` method to construct the `Juice` object's information.** 
  - An example of what the return string format of the `info` method is shown below:

```
>>> juice1 = Juice('large', 8.5, ["Apple", "Guava"])
>>> juice1.info()
'Apple/Guava Juice, large: $8.50'
```

<b>Note:</b> The `juice.info()` **return value** in the example above does not contain a newline character (`\n`) at the end. 

**Note:** The quotation marks around the **returned string** in IDLE tell us that the value was returned, _not_ printed, hence the string representation is shown.

---

<a href="#" id="drinkorderclass"></a>
## `DrinkOrder` class

The `DrinkOrder.py` file will contain the class definition of what a customer's drink order will contain, along with the total price of all beverages in the drink order.

Your `DrinkOrder` class definition should support the following constructor and methods:

* `__init__(self)` - constructor that initializes an empty list to the class. Name this list attribute `drinks`. This list `drinks` will eventually expand with drinks for the customer's drink order.
* `add(self, drink)` - method that will add the drink parameter to the `DrinkOrder`'s list. The most recently added drink will be at the end of the list. You may assume the drink parameter will either be a `Juice` or `Tea` object.
*  `total(self)` - method that will return a `str` containing each drink in the drink order, and the total price of all drinks in the drink order. An example of what the return string format of the `total` method is shown below:

```python
drink1 = Tea('small', 3.0, "Camomile")
juice1 = Juice('large', 8.5, ["Apple", "Guava"])
order = DrinkOrder()
order.add(drink1)
order.add(juice1)
print(order.total())
```

<b>Output:</b>

```
Order Items:
* Camomile Tea, small: $3.00
* Apple/Guava Juice, large: $8.50
Total Price: $11.50
```

**IMPORTANT**: be careful with the string formatting in the DrinkOrder class; especially the new line character and the space after the `*` for every new order.

An example of what the return string format of the `total()` method when there are no drinks in the Drink Order is shown below:

```
Order Items:
Total Price: $0.00
```

<b>Note:</b> There is NO space after the colon on the first line, just a newline (i.e., `"Order Items:\n"`). The `order.total()` return value in the examples above do not contain a newline character (`\n`) at the end. 

<a href="#" id="testingdrinkorder"></a>
## Testing `DrinkOrder`

How to write a correct assert statement when a method’s return value is a string that is very long and contains newlines?

We have two options: see the "miscellaneous" section in the document that's linked above in Step 4. In that example, the test creates two drink orders `order1` and `order2`, which are both tested in the `TestDrinkOrder` class's `test_total()` method.

---


<a href="testingcode"></a>
# Testing your code

## `testFile.py` pytests

This file will contain unit tests using `pytest` to test if your functionality is correct. You should create your own tests different than the examples given in this writeup. Think of various scenarios and method calls to be certain that the state of your objects and return values are correct (provide enough tests such that all method calls in `Drink`, `Tea`, `Juice` and `DrinkOrder` are covered). Even though Gradescope will not use this file when running the automated tests, it is important to provide this file with various test cases (testing is important!). 

We will manually grade your `testFile.py` to make sure your unit tests cover the defined methods in `Drink`, `Tea`, and `Juice` and `DrinkOrder`. 


<a href="submission"></a>
# Submission

Once you're done with writing your class definition and tests, Submit your `Drink.py`, `Tea.py`, `Juice.py`, `DrinkOrder.py`, and `testFile.py` files to the `Lab02` assignment on Gradescope. There will be various unit tests Gradescope will run to ensure your code is working correctly based on the specifications given in this lab.

**Note on grading for labs with testing component:** For this lab assignment (and all lab assignments requiring a `testFile.py`), we will manually grade the tests you write. In general, your lab score will be based on the autograder's score (out of 100 points).

* If you write your tests correctly according to the specifications, then you will receive 100/100 points.
* If your written tests are missing, incomplete, or incorrect, then there will be **up to** a 10 point deduction from the autograder score. For example, if you didn't write any tests, then your lab score will be 90/100 (-10 point deduction from the autograder's score).

Additionally, if the instructions are asking you to implement something in a certain way, e.g., to minimize code duplication, we might subtract points if your implementation does not follow these instructions.


<a href="troubleshooting"></a>
# Troubleshooting

If Gradescope's tests don't pass, you may get some error message that may or may not be obvious. Don't worry - if the tests didn't pass, take a minute to think about what may have caused the error. Try to think of your pytests and see if you can write a test to help you debug the error (if you haven't already). If you're still not sure why you're getting the error, feel free to post on the forum or ask your TAs or Learning Assistants.

Some of the common issues that students encountered in this lab:

* be careful with the string formatting in the DrinkOrder class; especially the new line character and the space after the `*` for every new order.
* DrinkOrder is **not** a child class of the Drink class.


<a href="#" id="grs"></a>
## Interpreting the autograder output on Gradescope

If you see the error 
`"The autograder failed to execute correctly. Please ensure that your submission is valid. Contact your course staff for help in debugging this issue. Make sure to include a link to this page so that they can help you most effectively."`
Make sure to remove any `print()` statements from your code or add them in the `if __name__ == "__main__"` section.

---

Below is an example output for a failed test on Gradescope:
```
- * Berry/Grape Juice, small: $ 5.00
?                              -
+ * Berry/Grape Juice, small: $5.00
```
```
- * Chamomile, large: $4.00
+ * Chamomile Tea, large: $4.00
?            ++++
```

* the line that starts with a `-` is the result of _your_ code
* the line that starts with a `?` shows you where the autograder detected a difference between your result and what is expected
  * if that line is showing a `-` it marks what needs to be _removed_ from your result 
  * if that line is showing a `+` (in the example above, `++++`), it marks what needs to be _added_ to your result 
* the line that starts with a `+` shows what was expected

In the first case, see the hint in the instructions about the price formatting using f-strings. 
In the second case, make sure that the `.info()` is correctly defined for the `DrinkOrder` class.


If you run into any other issues, make a post on the forum (remember to follow the posting guidelines) or ask during the lab / office hours.

---
