---
tags:
  - FORMAT
  - Testing
---
# What is Unit Testing?

**Unit testing** is a software testing technique where individual units of code (usually functions or methods) are tested in isolation to ensure they behave as expected.

---

# Goals:

- Verify correctness of logic
- Catch regressions early
- Enable safe refactoring
- Document expected behavior

---

# Key Characteristics:

---

# Unit vs Other Test Types:

---

# Structure of a Unit Test:

Most follow the **AAA pattern**:

1. **Arrange**: Set up inputs and context
2. **Act**: Call the function or method
3. **Assert**: Check the result is what you expect

Example in Python (with `unittest`):

```python
def add(a, b):
    return a + b

import unittest
class TestMath(unittest.TestCase):
    def test_add(self):
        self.assertEqual(add(2, 3), 5)
```

---

# Best Practices:

- Test one thing per test
- Keep tests short and focused
- Use descriptive test names (`test_add_two_positive_numbers`)
- Avoid external dependencies (mock where needed)
- Aim for high coverage, but prioritize meaningful coverage
- Make tests part of CI/CD

---

# Mocking in Unit Tests:

Use mocks/stubs to isolate the unit from:

- Databases
- Network calls
- File systems
- Time-based functions

Example in Python (with `unittest.mock`):

```python
from unittest.mock import patch
@patch('module.api_call')
def test_function(mock_api):
    mock_api.return_value = {'status': 'ok'}
    assert my_function() == expected_result
```

---

# Common Frameworks:

---

# What Not to Do:

- Don’t test external systems (use integration tests for that)
- Don’t write overly complex mocks
- Don’t ignore failing tests
- Don’t rely on test order (they should be independent)

---

Want a tailored example in a specific language or framework?