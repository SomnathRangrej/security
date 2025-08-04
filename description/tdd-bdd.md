# Explain with examples -  Test design with BDD/TDD


Sure! Let's break down **Test Design using BDD (Behavior Driven Development)** and **TDD (Test Driven Development)** with simple explanations and examples.

---

## 🔁 What is TDD (Test Driven Development)?

TDD is a **development-first testing approach** where:

1. Write the **test first**.
2. Run the test (it fails initially).
3. Write the **minimum code** to make the test pass.
4. Refactor the code and keep tests green.

### ✅ TDD Cycle:

**Red → Green → Refactor**

---

### 🧪 TDD Example (Python)

Let’s say we want to implement a function to check if a number is even.

#### Step 1: Write test (it will fail)

```python
import unittest

from math_utils import is_even  # Assume we will implement this

class TestMathUtils(unittest.TestCase):
    def test_is_even(self):
        self.assertTrue(is_even(2))
        self.assertFalse(is_even(3))

if __name__ == '__main__':
    unittest.main()
```

#### Step 2: Write code to make test pass

```python
# math_utils.py

def is_even(n):
    return n % 2 == 0
```

#### Step 3: Refactor (if needed)

Since the function is already simple and readable, no changes needed.

---

## 💬 What is BDD (Behavior Driven Development)?

BDD is a **behavior-focused testing approach** where:

1. Write tests in **natural language** (Given/When/Then).
2. Automate these behaviors using a test runner.
3. Focus is more on **user scenarios** and **business outcomes**.

### ✅ BDD Format:

**Given** some context
**When** some action is taken
**Then** there is an expected outcome

---

### 🧪 BDD Example (Using `behave` in Python)

#### Step 1: Feature file (`features/is_even.feature`)

```gherkin
Feature: Check if number is even

  Scenario: Number is even
    Given the number is 4
    When I check if it is even
    Then the result should be True

  Scenario: Number is odd
    Given the number is 5
    When I check if it is even
    Then the result should be False
```

#### Step 2: Step definitions (`features/steps/is_even_steps.py`)

```python
from behave import given, when, then
from math_utils import is_even

@given('the number is {n:d}')
def step_given_number(context, n):
    context.number = n

@when('I check if it is even')
def step_when_check_even(context):
    context.result = is_even(context.number)

@then('the result should be {expected_result}')
def step_then_result(context, expected_result):
    expected = True if expected_result == "True" else False
    assert context.result == expected
```

#### Step 3: Function code

```python
# math_utils.py

def is_even(n):
    return n % 2 == 0
```

---

## 🔍 Key Differences

| Aspect       | TDD                          | BDD                         |
| ------------ | ---------------------------- | --------------------------- |
| Focus        | Unit tests, code correctness | Behavior, user expectations |
| Language     | Code                         | Natural language (Gherkin)  |
| Stakeholders | Developers                   | Developers + QA + Business  |
| Tools        | unittest, pytest (Python)    | behave, cucumber, specflow  |

---

## 🛠️ When to Use

* Use **TDD** when you're doing **low-level module development**.
* Use **BDD** when you need to **collaborate with non-dev stakeholders** (e.g., QA, Product Owners).

---

Would you like an example using JavaScript or Java as well? Or examples from API testing/UI automation?
