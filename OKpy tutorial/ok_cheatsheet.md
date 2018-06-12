# OKpy Cheatsheet

A markdown version of the OKpy tutorial, without the interactivity.

### 1. Importing
```
from client.api.notebook import Notebook
ok = Notebook('ok_tutorial.ok')
_ = ok.auth(inline=True)
```
**Things to remember:**
* `.ok` and `.ipynb` filenames should match
* Keep `.ok` file in the same directory as the `.ipynb`
  * e.g if name of assignment is `hw1.ipynb` then `.ok` file should be `hw1.ok`

----

### 2. Creating and Modifying a `.ok` file

Example: `.ok` file for a lab

```
{
  "name": "lab 01",
  "endpoint": "cal/ugba96/fa18/lab01",
  "src": [
    "lab01.ipynb"
  ],
  "tests": {
      "tests/q*.py": "ok_test"
  },
  "protocols": [
      "file_contents"
      "grading",
      "backup"
  ]
}
```

**Things to remember:**
* Change `name`, `endpoint`, `src` values for relevant assignment/lab
* No need to change `protocols`
* Leave `tests` alone as well
* Edit in Jupyter by changing extension to `.py`!!! (And change the extension back `.ok`)

----

### 3. Tests

Example: `q1_1.py` for question 1.1 for an arbitrary assignment:

```
test = {
  'name': 'Question 1_1',
  'points': 1,
  'suites': [
    {
      'cases': [
        {
          'code': r"""
          >>> x == 35
          True
          """,
          'hidden': False,
          'locked': False
        },
        {
          'code': r"""
          >>> x - 35 > 0
          True
          >>> x + 35 == 70
          True
          """,
          'hidden': False,
          'locked': False
        }
      ],
      'scored': True,
      'setup': '',
      'teardown': '',
      'type': 'doctest'
    }                         
  ]
}
```

**Things to remember:**
* **DO NOT FORGET to make a `tests` folder with `__init__.py` and *at least* one test (even if lab only needs to be  submitted)**
* It is possible to write as many lines of code you want in a specific case
* Not necessary to add suites, since most of the tests will be pretty similar
  * Usually testing variables, not multiple lines of code
* No need to change `setup`, `teardown`, and `type` values

----

### 4. Grading and Submission

Code to run grader and backup (if needed):
```
_ = ok.grade('q1_1')
_ = ok.backup()
```

**Things to remember:**
* Grader will run tests in `q1_1.py`
* Change the name of the file with respect to the question being graded

Code to run all tests at the end of notebook (same format as data 8 notebooks):

```
import os
print("Running all tests...")
_ = [ok.grade(q[:-3]) for q in os.listdir("tests") if q.startswith('q')]
print("Finished running all tests.")
```

Code to submit:

```
_ = ok.submit()
```
