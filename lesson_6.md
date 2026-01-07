<h1 align="center" style="color:#2E86C1;">R'SOFT</h1>
<p align="center" style="color:#2E86C1; font-size:20px;">Web Development Company</p>

---

## 📘 Lesson 6 - Pytest Basics for Selenium (Fixtures, Markers, Conftest)

---

### ✳️ Ի՞նչ է Pytest-ը և ինչու ենք օգտագործում Selenium-ում

**Pytest**-ը Python-ի թեստավորման framework է, որը թույլ է տալիս գրել պարզ ու ընթեռնելի թեստեր և հեշտորեն ընդլայնել
դրանք՝  
**fixtures, markers, plugins, configuration** և այլն։

Սովորաբար Selenium ավտոմատացման մեջ Pytest-ը մեզ օգնում է՝

- **✅** թեստերը կազմակերպել (ֆոլդերներ, naming, test discovery)
- **✅** ստեղծել ընդհանուր **setup/teardown** (օր․ browser driver-ի բացում/փակում)
- **✅** կիրառել թեստերի խմբավորում՝ **smoke**, **regression** և այլն
- **✅** մի տեղում պահել ընդհանուր կոնֆիգը (`pytest.ini`, `conftest.py`)

---

### ✳️ Նախագծի առաջարկվող կառուցվածք

📌 Minimal, բայց ճիշտ սկիզբ․

```markdown
project/
requirements.txt
pytest.ini
conftest.py
tests/
test_smoke.py
test_open_site.py
```

📌 Երբ նախագիծը մեծանում է՝ կարող ենք ավելացնել `pages/`, `utils/`, `data/` և այլն։

---

### ✳️ Pytest-ի տեղադրում (initial setup)

1. Տեղադրում ենք անհրաժեշտ փաթեթները՝

```shell
pip install pytest selenium
```

---

### ✳️ Առաջին թեստը (ամենապարզ տարբերակ)

`tests/test_smoke.py`

```python
def test_smoke():
    assert 1 + 1 == 2
```

Run:

```shell
pytest -v
```

📌 `-v` → ավելի մանր output։

---

### ✅ Preconditions (Setup) և Postconditions (Teardown/Cleanup)

#### ✳️ Setup/Teardown-ը Pytest-ում ինչպես է արվում

Pytest-ում setup/teardown-ի ամենահարմար ձևը fixture-ներն են։

Fixture-ը կարող է՝

- տպել ինչ-որ բան (debug)
- վերադարձնել արժեք
- բացել browser / ստեղծել driver
- ավարտից հետո անել cleanup (close/quit, delete temp data, logout և այլն)

Fixture օգտագործելու համար պետք է import անել `pytest`-ը՝

```python
import pytest
```

---

#### ✳️ Preconditions (Setup) — fixture-ի ամենապարզ օրինակ

սրա համար մեզ անհրաժեշտ է ստեղծել `fixture`

```python
import pytest


@pytest.fixture()
def separator():
    print("=" * 10)
```

Այն թեստում, որտեղ ուզում ենք օգտագործել `separator`-ը, պարզապես ավելացնում ենք որպես argument՝

```python
def test_one_is_one(separator):
    assert 1 == 1
```

📌 Նշում՝ fixture-ը կկատարվի հենց այն թեստի առաջ, որը կանչում է իրեն։

---

#### ✳️ Fixture-ը կարող է վերադարձնել արժեք

```python
import pytest


@pytest.fixture()
def separator():
    print("=" * 10)
    return 'value'
```

Օգտագործում թեստում՝

```python
def test_one_is_one(separator):
    print(f"This is separator: {separator}")
    assert 1 == 1
```

---

#### ✳️ Postconditions (Teardown) — `yield`-ով

Եթե ուզում ենք, որ fixture-ը նաև cleanup անի, ապա return-ը փոխարինում ենք yield-ով։

```python
import pytest


@pytest.fixture()
def separator():
    print("=" * 10)  # Setup
    yield 'value'  # Ահա այստեղից հետո կատարվում է թեստը
    print("Test Finished")  # Teardown (թեստից հետո)
```

📌 `yield`-ից հետո գտնվող հատվածը միշտ կաշխատի՝ նույնիսկ եթե թեստը fail լինի։

---

### 🌍 Scope: երբ fixture-ը պետք է աշխատի մեկ անգամ, ոչ թե ամեն թեստից առաջ

#### ✳️ `scope='session'` — “Before All / After All” ճիշտ տարբերակով

Սա աշխատում է մի անգամ ամբողջ test run-ի ընթացքում՝

```python
import pytest


@pytest.fixture(scope='session')
def all_tests():
    print("Before All Tests (session start)")
    yield
    print("After All Tests (session end)")
```

📌 Կարևոր․

- `Before All...` → մեկ անգամ, թեստերի սկզբում
- `After All...` → մեկ անգամ, թեստերի վերջում

(ոչ թե յուրաքանչյուր թեստից հետո)

---

### 🏷️ Marks (skip, smoke, regression)

#### ✳️ Mark `skip`

```python
import pytest


@pytest.mark.skip(reason="Not Implemented")
def test_three_is_three():
    assert 3 == 3
```

---

#### ✳️ Mark `smoke`

```python
import pytest


@pytest.mark.smoke
def test_two_is_two():
    assert 2 == 2
```

---

#### ✳️ Mark `regression`

```python
import pytest


@pytest.mark.regression
def test_one_is_one():
    assert 1 == 1
```

---

#### ✳️ `smoke` և `regression` warning-ներից խուսափելու համար

Ստեղծում ենք `pytest.ini` և ավելացնում markers-ը՝

```ini
[pytest]
markers =
    smoke: marks tests as smoke
    regression: marks tests as regression
```

📌 Սա Pytest-ին ասում է՝ “այս markers-ը գոյություն ունի”։

---

#### ✳️ Ինչպես run անել միայն smoke կամ regression թեստերը

```shell
pytest -m smoke -v
pytest -m regression -v
```

### 🧩 conftest.py — ընդհանուր fixtures / configuration

#### ✳️ Ինչու է պետք conftest.py-ը

Որպեսզի նույն fixtures-ը չգրենք ամեն `test_*` ֆայլում, դրանք տեղափոխում ենք `conftest.py`։

📌 Pytest-ը ավտոմատ գտնում է conftest.py-ն ու fixture-ները հասանելի է դարձնում թեստերին՝ առանց import-ի։

---

### ✳️ Selenium WebDriver fixture (setup/teardown ճիշտ ձևով)

`conftest.py`

```python
import pytest
from selenium import webdriver


@pytest.fixture
def driver():
    driver = webdriver.Chrome()
    driver.maximize_window()
    yield driver
    driver.quit()
```

Օրինակ օգտագործում՝ `tests/test_open_site.py`

```python
def test_open_site(driver):
    driver.get('https://www.qa-practice.com/')
    assert 'QA Practice' in driver.title
```

---

### ✳️ Օգտակար run հրամաններ (պրակտիկ)

- Մի կոնկրետ ֆայլի թեստերը՝

```shell
pytest tests/test_open_site.py -v
```

- Ըստ անունի ֆիլտրել (-k)՝

```shell
pytest -k open_site -v
```

- Output-ը չթաքցնել (debug-ի ժամանակ)՝

```shell
pytest -s -v
```

- Միայն smoke թեստերը՝

```shell
pytest -m smoke -v
```
