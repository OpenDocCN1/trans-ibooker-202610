
```
```
--snip--
test_name_function.py .
[100%]
============================= 1 passed in 0.00s ==============================
------------------------------
```
```

The test passes now. This is ideal; it means the function works for names like Janis Joplin again, without us having to test the function manually. Fixing our function was easier because the failed test helped us identify how the new code broke existing behavior.

### Adding New Tests

Now that we know get_formatted_name() works for simple names again, let’s write a second test for people who include a middle name. We do this by adding another test function to the file test_name_function.py:

**test_name_function.py**

```
```
from name_function import get_formatted_name

def test_first_last_name():
    --snip--

def test_first_last_middle_name():
    """Do names like 'Wolfgang Amadeus Mozart' work?"""
    formatted_name = get_formatted_name(
        'wolfgang', 'mozart', 'amadeus')
    assert formatted_name == 'Wolfgang Amadeus Mozart'
```
```

We name this new function test_first_last_middle_name(). The function name must start with test_ so the function runs automatically when we run **pytest**. We name the function to make it clear which behavior of get_formatted_name() we’re testing. As a result, if the test fails, we’ll know right away what kinds of names are affected.

# TRY IT YOURSELF

**11-1. City, Country:** Write a function that accepts two parameters: a city name and a country name. The function should return a single string of the form *City, Country*, such as `Santiago, Chile`. Store the function in a module called `city_functions.py`, and save this file in a new folder so `pytest` won’t try to run the tests we’ve already written.

Create a file called `test_cities.py` that tests the function you just wrote. Write a function called `test_city_country()` to verify that calling your function with values such as 'santiago' and 'chile' results in the correct string. Run the test, and make sure `test_city_country()` passes.

**11-2. Population:** Modify your function so it requires a third parameter, `population`. It should now return a single string of the form *City, Country – population xxx*, such as `Santiago, Chile – population 5000000`. Run the test again, and make sure `test_city_country()` fails this time.

Modify the function so the `population` parameter is optional. Run the test, and make sure `test_city_country()` passes again.

Write a second test called `test_city_country_population()` that verifies you can call your function with the values 'santiago', 'chile', and 'population=5000000'. Run the tests one more time, and make sure this new test passes.

# Testing a Class

In the first part of this chapter, you wrote tests for a single function. Now you’ll write tests for a class. You’ll use classes in many of your own programs, so it’s helpful to be able to prove that your classes work correctly. If you have passing tests for a class you’re working on, you can be confident that improvements you make to the class won’t accidentally break its current behavior.

## A Variety of Assertions

So far, you’ve seen just one kind of assertion: a claim that a string has a specific value. When writing a test, you can make any claim that can be expressed as a conditional statement. If the condition is True as expected, your assumption about how that part of your program behaves will be confirmed; you can be confident that no errors exist. If the condition you assume is True is actually False, the test will fail and you’ll know there’s an issue to resolve. Table 11-1 shows some of the most useful kinds of assertions you can include in your initial tests.

**Table 11-1: _Commonly Used Assertion Statements in Tests_**

| Assertion | Claim |
|---|---|
| `assert a == b` | Assert that two values are equal. |
| `assert a != b` | Assert that two values are not equal. |
| `assert a` | Assert that a evaluates to True. |
| `assert not a` | Assert that a evaluates to False. |
| `assert element in list` | Assert that an element is in a list. |
| `assert element not in list` | Assert that an element is not in a list. |

These are just a few examples; anything that can be expressed as a conditional statement can be included in a test.

## A Class to Test

Testing a class is similar to testing a function, because much of the work involves testing the behavior of the methods in the class. However, there are a few differences, so let’s write a class to test. Consider a class that helps administer anonymous surveys:

**survey.py**

```python
class AnonymousSurvey:
    """Collect anonymous answers to a survey question."""

    def __init__(self, question):
        """Store a question, and prepare to store responses."""
        self.question = question
```

```python
self.responses = []

def show_question(self):
    """Show the survey question."""
    print(self.question)

def store_response(self, new_response):
    """Store a single response to the survey."""
    self.responses.append(new_response)

def show_results(self):
    """Show all the responses that have been given."""
    print("Survey results:")
    for response in self.responses:
        print(f"- {response}")
```
```

This class starts with a survey question that you provide ❶ and includes an empty list to store responses. The class has methods to print the survey question ❷, add a new response to the response list ❸, and print all the responses stored in the list ❹. To create an instance from this class, all you have to provide is a question. Once you have an instance representing a particular survey, you display the survey question with show_question(), store a response using store_response(), and show results with show_results().

To show that the AnonymousSurvey class works, let’s write a program that uses the class:

# **language_survey.py**

```python
from survey import AnonymousSurvey

# Define a question, and make a survey.
question = "What language did you first learn to speak?"
language_survey = AnonymousSurvey(question)

# Show the question, and store responses to the question.
language_survey.show_question()
print("Enter 'q' at any time to quit.\n")
while True:
    response = input("Language: ")
```

```
if response == 'q':
    break
language_survey.store_response(response)

# Show the survey results.
print("\nThank you to everyone who participated in the survey!")
language_survey.show_results()
```

This program defines a question ("What language did you first learn to speak?") and creates an AnonymousSurvey object with that question. The program calls show_question() to display the question and then prompts for responses. Each response is stored as it is received. When all responses have been entered (the user inputs q to quit), show_results() prints the survey results:

```
What language did you first learn to speak?
Enter 'q' at any time to quit.

Language: English
Language: Spanish
Language: English
Language: Mandarin
Language: q

Thank you to everyone who participated in the survey!
Survey results:
- English
- Spanish
- English
- Mandarin
```

This class works for a simple anonymous survey, but say we want to improve AnonymousSurvey and the module it’s in, survey. We could allow each user to enter more than one response, we could write a method to list only unique responses and to report how many times each response was given, or we could even write another class to manage non-anonymous surveys.

Implementing such changes would risk affecting the current behavior of the class AnonymousSurvey. For example, it’s possible that while trying to allow each user to enter multiple responses, we could accidentally change how single responses are handled. To ensure we don’t break existing behavior as we develop this module, we can write tests for the class.

# Testing the AnonymousSurvey Class

Let’s write a test that verifies one aspect of the way AnonymousSurvey behaves. We’ll write a test to verify that a single response to the survey question is stored properly:

test_survey.py

```
from survey import AnonymousSurvey

def test_store_single_response():
    """Test that a single response is stored properly."""
    question = "What language did you first learn to speak?"
    language_survey = AnonymousSurvey(question)
    language_survey.store_response('English')
    assert 'English' in language_survey.responses
```

We start by importing the class we want to test, AnonymousSurvey. The first test function will verify that when we store a response to the survey question, the response will end up in the survey’s list of responses. A good descriptive name for this function is test_store_single_response() ①. If this test fails, we’ll know from the function name in the test summary that there was a problem storing a single response to the survey.

To test the behavior of a class, we need to make an instance of the class. We create an instance called language_survey ② with the question "What language did you first learn to speak?" We store a single response, English, using the store_response() method. Then we verify that the
