<h1 align="center" style="color:#2E86C1;">R'SOFT</h1>
<p align="center" style="color:#2E86C1; font-size:20px;">Web Development Company</p>

---

## 📘 Lesson 14 — Allure Report (Pytest + Selenium)

---

### ✳️ Ինչ է Allure-ը

Allure-ը test report-ի գործիք է, որը օգնում է՝
**✅** տեսնել գեղեցիկ report (անցած/չանցած tests, steps, ժամանակներ)  
**✅** ունենալ attach-ներ (screenshot, logs, html, json)  
**✅** ունենալ labels (feature/story/severity), և հեշտ նավիգացիա

---

### ✅ Ինչ է պետք, որպեսզի Allure report աշխատի

Allure report-ի համար սովորաբար պետք է 2 մաս.

1. Pytest plugin → որ test-ի արդյունքները գրվեն `allure-results/`
2. Allure Commandline → որ այդ results-ից report գեներացվի ու բացվի

---

### 1) 📦 Տեղադրում — Python փաթեթներ (բոլոր OS-երի համար նույնն է)

Եթե ունես venv՝ ակտիվացրու, հետո՝

```shell
pip install pytest allure-pytest
```

Ստուգում՝

```shell
pytest --version
```

---

### 2) 🧰 Տեղադրում — Allure Commandline (Windows / macOS / Ubuntu)

#### 🪟 Windows

**Տարբերակ A — Chocolatey**

```shell
choco install allurecommandline -y
```

**Տարբերակ B — Scoop**

```shell
scoop install allure
```

Ստուգում՝

```shell
allure --version
```

---

#### 🍎 macOS

```shell
brew install allure
allure --version
```

---

#### 🐧 Ubuntu (24.04 և այլն)

Քանի որ Allure commandline-ը Java-ով է աշխատում, լավ է ունենալ JRE։

```shell
sudo apt update
sudo apt install -y default-jre
```

Հետո Allure commandline-ը ամենահարմար տարբերակներից մեկը՝ npm-ով։

```shell
sudo apt install -y nodejs npm
sudo npm i -g allure-commandline
allure --version
```

---

### 3) ▶️ Ինչպես ստեղծել Allure report

**Քայլ 1 — Run անել tests-ը և պահել results-ը**

```shell
pytest -q --alluredir=allure-results
```

📌 allure-results/ folder֊ում կստեղծվեն json ֆայլեր՝ report-ի համար։

---

**Քայլ 2 — Բացել report-ը**

**Allure “preview” հրամանն է**

```shell
allure serve allure-results
```

այս հրամանը ավտոմատ՝

- բացում է browser-ում

---

### 4) 🏷️ Allure annotations (labels)՝ որ report-ը «կարդացվող» լինի

**Օրինակ՝ title + feature + severity**

```python
import allure


@allure.title('Login: valid credentials')
@allure.feature('Authentication')
@allure.severity(allure.severity_level.CRITICAL)
def test_login_valid(driver):
    ...
```

### 5) 🪜 Steps (քայլերով report) — ամենակարևորներից մեկը

```python
import allure
from selenium.webdriver.common.by import By


def test_login_valid(driver):
    with allure.step('Open login page'):
        driver.get('https://practicetestautomation.com/practice-test-login/')

    with allure.step('Fill username/password'):
        driver.find_element(By.ID, 'username').send_keys('student')
        driver.find_element(By.ID, 'password').send_keys('Password123')

    with allure.step('Click submit'):
        driver.find_element(By.ID, 'submit').click()

    with allure.step('Verify success'):
        assert 'Logged In Successfully' in driver.page_source
```

📌 Report-ում test-ը կերևա «քայլերով», ինչը շատ օգնում է debug-ի ժամանակ։

---

### 6) 📎 Attachments — Screenshot / Text / HTML

**a) Attach text (օր. log)**

```python
import allure

allure.attach('Some debug info', name='debug', attachment_type=allure.attachment_type.TEXT)

```

**b) Attach screenshot (PNG)**

```python
import allure

png = driver.get_screenshot_as_png()
allure.attach(png, name='screenshot', attachment_type=allure.attachment_type.PNG)
```

---

### 7) ✅ Ավտոմատ screenshot միայն failure-ի դեպքում (Pytest hook)

conftest.py ֆայլում՝

```python
import allure
import pytest


@pytest.hookimpl(hookwrapper=True)
def pytest_runtest_makereport(item, call):
    outcome = yield
    rep = outcome.get_result()

    if rep.when != 'call' or not rep.failed:
        return

    driver = item.funcargs.get('driver')

    if driver:
        allure.attach(
            driver.get_screenshot_as_png(),
            name=f'failure_screenshot_{item.name}',
            attachment_type=allure.attachment_type.PNG
        )

    # Attach exception message
    if call.excinfo:
        allure.attach(
            str(call.excinfo.value),
            name=f'exception_{item.name}',
            attachment_type=allure.attachment_type.TEXT
        )
```

📌 Սա ամենաօգտակար “պրակտիկ” բաներից է՝ report-ում failure-ի կողքը ավտոմատ screenshot կկցվի։

---

### ⚠️ Ամենատարածված սխալները

1. `allure command not found`
   → Allure Commandline-ը չի տեղադրված կամ PATH-ում չէ։
   Ստուգիր՝ `allure --version`
2. `allure-results` չկա/դատարկ է
   → pytest-ը չես run արել `--alluredir=...`-ով
   Լուծում՝ `pytest --alluredir=allure-results`
3. Հին results-ները խառնում են նոր report-ը
   → Միշտ մաքրիր

---

### Allure 3 History (Allure 3֊ի պատմություն)

եթե ցանկանում ենք Allure֊ի նախկին run եղած թեստերի պատմությունը պահպանել՝

Նախ պետք է ստեղծել `allurerc.mjs` ֆաիլը գլխավոր project֊ի root֊ից

```js
export default {
    output: 'allure-report',

    // history-ը պահվում է այս ֆայլում (կարևոր՝ սա չջնջես)
    historyPath: './.allure/history.jsonl',

    appendHistory: true,
};
```

**Ինչպես run անել լոկալում, որ history-ը չկորի**  
**մաքրի՛ր report/results-ը, բայց չջնջես .allure/ պանակը։**

```shell
rm -rf allure-results allure-report
pytest -q --alluredir=allure-results
allure generate -o allure-report allure-results
allure open allure-report
```

Սրանից հետո ստուգիր՝ history ֆայլը ստեղծվե՞լ է ու աճո՞ւմ է.

```shell
ls -l .allure/history.jsonl
wc -l .allure/history.jsonl
```

---

### Տնային աշխատանք (Homework)

1. Ստեղծիր 2 test՝
    - `test_login_valid`
    - `test_login_invalid` <br>
      և run արա՝ `pytest --alluredir=allure-results`, հետո `allure serve allure-results`

2. Քո tests-ում օգտագործիր `with allure.step(...)` նվազագույնը 3 քայլով
3. Ավելացրու failure screenshot hook-ը `conftest.py`-ում և ստուգիր՝ սխալ test-ի դեպքում report-ում screenshot կա