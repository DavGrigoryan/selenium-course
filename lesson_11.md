<h1 align="center" style="color:#2E86C1;">R'SOFT</h1>
<p align="center" style="color:#2E86C1; font-size:20px;">Web Development Company</p>

---

## 📘 Lesson 11 - Pytest parametrization (Running a test with different test data)

### 🌐 Պրակտիկ կայք․ https://www.qa-practice.com/

---

### ✳️ Ինչ է Parametrization-ը Pytest-ում

Parametrization-ը թույլ է տալիս նույն test-ը մեկ անգամ գրել, բայց աշխատեցնել բազմաթիվ test data-ներով։

Օրինակ՝ նույն login test-ը աշխատեցնենք՝

- ճիշտ username/password
- սխալ username
- սխալ password
- դատարկ դաշտեր

Սրա օգուտը՝  
**✅** քիչ կոդ, քիչ կրկնություն  
**✅** ավելի շատ coverage (ծածկույթ)  
**✅** ավելի հեշտ է ավելացնել նոր case-եր

---

### ✅ Ամենակարևոր սինթաքսը՝ `@pytest.mark.parametrize`

```python
import pytest


@pytest.mark.parametrize('a, b, expected', [
    (1, 2, 3),
    (5, 5, 10),
    (-1, 1, 0),
])
def test_sum(a, b, expected):
    assert a + b == expected
```

📌 Pytest-ը այս test-ը կվազեցնի 3 անգամ, ամեն անգամ՝ տարբեր input-ներով։

---

### ❗ Կարևոր կանոն

Parametrize-ի մեջ arg names-ը պետք է համընկնի test ֆունկցիայի argument-ների հետ։

Սխալ օրինակ՝

```python
@pytest.mark.parametrize('x, y', [(1, 2)])
def test_demo(a, b):  # սխալ՝ անունները չեն համընկնում
    ...
```

---

### 🔁 Selenium օրինակ՝ Login test տարբեր տվյալներով

#### ✅ Նախ՝ minimal “page actions” (քայլերը նույնը, տվյալները՝ տարբեր)

```python
import pytest
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

URL = 'https://practicetestautomation.com/practice-test-login/'


@pytest.mark.parametrize('username, password, expected_text', [
    ('student', 'Password123', 'Logged In Successfully'),  # ✅ valid
    ('wrong', 'Password123', 'Your username is invalid!'),  # ❌ invalid username
    ('student', 'wrong', 'Your password is invalid!'),  # ❌ invalid password
])
def test_login_with_different_data(driver, username, password, expected_text):
    driver.get(URL)
    wait = WebDriverWait(driver, 10)

    wait.until(EC.element_to_be_clickable((By.ID, 'username'))).send_keys(username)
    wait.until(EC.element_to_be_clickable((By.ID, 'password'))).send_keys(password)
    wait.until(EC.element_to_be_clickable((By.ID, 'submit'))).click()

    if expected_text == 'Logged In Successfully':
        title = wait.until(EC.visibility_of_element_located((By.CLASS_NAME, 'post-title'))).text.strip()
        assert title == expected_text
    else:
        error = wait.until(EC.visibility_of_element_located((By.ID, 'error'))).text.strip()
        assert error == expected_text
```

📌 Սա արդեն 3 test case է՝ մեկ ֆունկցիայի մեջ։

---

### ✳️ Parametrize + `ids` (որպեսզի report-ում case-երը գեղեցիկ երևան)

Եթե ուզում ես test report-ում case-երի անունները readable լինեն՝

```python
import pytest


@pytest.mark.parametrize(
    'username, password, expected_text',
    [
        ('student', 'Password123', 'Logged In Successfully'),
        ('wrong', 'Password123', 'Your username is invalid!'),
        ('student', 'wrong', 'Your password is invalid!'),
    ],
    ids=[
        'valid_credentials',
        'invalid_username',
        'invalid_password',
    ]
)
def test_login_ids(driver, username, password, expected_text):
    ...
```

**✅** Test runner-ում կտեսնես՝

- `test_login_ids[valid_credentials]`
- `test_login_ids[invalid_username]`
- `test_login_ids[invalid_password]`

---

### ✳️ Parametrize + dictionary (երբ շատ field-եր կան)

Որոշ դեպքերում ավելի հարմար է մեկ case dict փոխանցել։

```python
import pytest

cases = [
    {'username': 'student', 'password': 'Password123', 'expected': 'Logged In Successfully'},
    {'username': 'wrong', 'password': 'Password123', 'expected': 'Your username is invalid!'},
]


@pytest.mark.parametrize('case', cases, ids=['ok', 'bad_user'])
def test_login_case_dict(driver, case):
    username = case['username']
    password = case['password']
    expected = case['expected']
    ...
```

**✅** Հարմար է, երբ case-երը մեծանում են (օր․ role, language, rememberMe, և այլն)

---

### ✅ Լավ պրակտիկա (Important)

- Մի՛ գրիր 5 նույնատիպ test ֆունկցիա՝ տարբեր input-ներով → գրիր 1 test + `parametrize`
- Քո case-երը անվանիր `ids`-ով, որպեսզի debug անելիս արագ հասկանաս ինչն է ընկել
- Selenium-ում data-driven test-երի դեպքում միշտ պահիր նույն “flow”-ը, միայն տվյալներն փոխի
- Եթե տարբեր արդյունքներ ունես (success vs error)՝ արա պարզ branching (ինչպես վերևում)

---

### 🧪 Տնային աշխատանք (Homework)

1. Գրիր `test_sum` parametrized test՝ 5 տարբեր case-ով (նորմալ, 0, negative, մեծ թվեր)
2. Selenium-ում ավելացրու ևս 2 login case՝
    - username դատարկ, password ճիշտ
    - username ճիշտ, password դատարկ
3. Ավելացրու `ids` բոլոր case-երի համար (պարտադիր)

<details> <summary>💡 Solution N_1</summary>

```python
import pytest


@pytest.mark.parametrize(
    'a, b, expected',
    [
        (2, 3, 5),  # normal
        (0, 7, 7),  # includes 0
        (0, 0, 0),  # both 0
        (-4, 10, 6),  # negative + positive
        (1_000_000_000, 2_000_000_000, 3_000_000_000),  # big numbers
    ],
    ids=[
        'normal',
        'with_zero',
        'both_zero',
        'negative',
        'big_numbers',
    ]
)
def test_sum(a, b, expected):
    assert a + b == expected
```

</details>