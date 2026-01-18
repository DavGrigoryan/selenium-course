<h1 align="center" style="color:#2E86C1;">R'SOFT</h1>
<p align="center" style="color:#2E86C1; font-size:20px;">Web Development Company</p>

---

## 📘 Lesson 10 - Keyboard Actions Selenium-ում (Actions API)

### 🌐 Պրակտիկ կայք․ https://www.qa-practice.com/

### ⚠️ Նշում

📌 Keyboard actions-ը աշխատում է focus-ով.

- Եթե input-ը focus չունի → `ActionChains(...).send_keys(...)`-ը կարող է գնալ «ոչ ճիշտ տեղ» կամ ոչինչ չանի։
- Լավ տարբերակներն են՝
  - նախ `click()` անել input-ի վրա
  - կամ օգտագործել `send_keys_to_element(element, ...)`

---

### ✳️ Ինչ է Keyboard Action-ը

Keyboard Action-ը Selenium-ում ներկայացնում է ստեղնաշարի input սարքը, որի միջոցով կարող ենք՝

- typing անել (տառեր, թվեր)
- սեղմել հատուկ ստեղներ (`ENTER`, `TAB`, `ESC`, սլաքներ)
- օգտագործել combo-ներ (`CTRL + A`, `CTRL + C`, `CTRL + V`, `SHIFT + ARROW`)

📌 Հատուկ ստեղների համար օգտագործվում է `Keys` դասը։

```python
from selenium.webdriver.common.keys import Keys
```

---

### ❗ Կարևոր տարբերություն

- `element.send_keys("abc")`  
  **✅** ուղիղ գրում է տվյալ element-ի մեջ (ամենապարզը)

- `ActionChains(driver).send_keys("abc")`  
  **✅** ուղարկում է keyboard input-ը active element-ին  
  **✅** հիմնականում պետք է, երբ typing-ը ուզում ենք խառնել այլ action-ների հետ (mouse + keyboard)

---

### 🔁 Keyboard actions-ի հիմնական սցենարները

---

### ✳️ Keys (հատուկ ստեղներ)

Ամենաշատ օգտագործվողները՝

```python
from selenium.webdriver.common.keys import Keys

Keys.ENTER
Keys.TAB
Keys.ESCAPE
Keys.BACKSPACE
Keys.DELETE
Keys.ARROW_LEFT
Keys.ARROW_RIGHT
Keys.ARROW_UP
Keys.ARROW_DOWN
Keys.SHIFT
Keys.CONTROL
Keys.ALT
Keys.COMMAND
```

---

### ✳️ Key down (օր․ SHIFT պահել ու գրել)

Եթե ուզում ենք “ABC” ստանալ՝ SHIFT + abc.

```python
from selenium.webdriver import Keys, ActionChains
from selenium.webdriver.common.by import By


def test_key_down(driver):
    driver.get('https://selenium.dev/selenium/web/single_text_input.html')

    ActionChains(driver) \
        .key_down(Keys.SHIFT) \
        .send_keys("abc") \
        .key_up(Keys.SHIFT) \
        .perform()

    assert driver.find_element(By.ID, "textInput").get_attribute('value') == "ABC"
```

📌 Եթե `key_up` չանես, SHIFT-ը կարող է «սեղմված» մնալ հաջորդ գործողությունների համար։

---

### ✳️ Key up (մի մասը մեծատառ, մի մասը՝ փոքրատառ)

Օր․ առաջինը մեծատառ, հետո փոքրատառ։

```python
from selenium.webdriver import Keys, ActionChains
from selenium.webdriver.common.by import By

def test_key_up(driver):
    driver.get('https://selenium.dev/selenium/web/single_text_input.html')

    ActionChains(driver) \
        .key_down(Keys.SHIFT) \
        .send_keys("a") \
        .key_up(Keys.SHIFT) \
        .send_keys("b") \
        .perform()

    assert driver.find_element(By.ID, "textInput").get_attribute('value') == "Ab"
```

Արդյունքը՝ `Ab`

---

### ✳️ Send keys → Active element (focus պետք է լինի)

Այս տարբերակում typing-ը գնում է այն element-ի մեջ, որը տվյալ պահին active է։

```python
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By

def test_send_keys_to_active_element(driver):
    driver.get('https://selenium.dev/selenium/web/single_text_input.html')

    ActionChains(driver) \
        .send_keys("abc") \
        .perform()

    assert driver.find_element(By.ID, "textInput").get_attribute('value') == "abc"
```

📌 Եթե input-ը չես focus արել, հնարավոր է text-ը գնա body-ի վրա կամ «կորչի»։

---

### ✳️ Send keys to designated element (ամենակայուն տարբերակներից)

Ուղիղ գրում ենք կոնկրետ element-ի մեջ՝ առանց focus-ի վրա շատ հույս դնելու։

```python
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By

def test_send_keys_to_designated_element(driver):
    driver.get('https://selenium.dev/selenium/web/single_text_input.html')
    driver.find_element(By.TAG_NAME, "body").click()

    text_input = driver.find_element(By.ID, "textInput")
    ActionChains(driver) \
        .send_keys_to_element(text_input, "abc") \
        .perform()

    assert driver.find_element(By.ID, "textInput").get_attribute('value') == "abc"
```

---

### ✳️ TAB-ով navigation (focus տեղափոխել հաջորդ դաշտ)

Օգտակար է form-երի ավտոմատացման ժամանակ։

```python
from selenium.webdriver import ActionChains
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By

def test_send_key_with_tab_navigation_to_another_element(driver):
    driver.get('https://www.selenium.dev/selenium/web/inputs.html')
    driver.find_element(By.NAME, "search_input").click()

    ActionChains(driver) \
        .send_keys(Keys.TAB) \
        .send_keys('typing in next field') \
        .perform()

    assert driver.find_element(By.NAME, "tel_input").get_attribute('value') == "typing in next field"
```

---

### Copy / Paste (Ctrl/Cmd)

📌 Windows/Linux → `CTRL`
📌 macOS → `COMMAND`

```python
import sys
from selenium.webdriver import ActionChains
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By

def test_copy_and_paste(driver):
    driver.get('https://selenium.dev/selenium/web/single_text_input.html')
    cmd_ctrl = Keys.COMMAND if sys.platform == 'darwin' else Keys.CONTROL

    ActionChains(driver) \
        .send_keys("Selenium!") \
        .send_keys(Keys.ARROW_LEFT) \
        .key_down(Keys.SHIFT) \
        .send_keys(Keys.ARROW_UP) \
        .key_up(Keys.SHIFT) \
        .key_down(cmd_ctrl) \
        .send_keys("xvv") \
        .key_up(cmd_ctrl) \
        .perform()

    assert driver.find_element(By.ID, "textInput").get_attribute('value') == "SeleniumSelenium!"
```

---

### ✅ Լավ պրակտիկա (Important)

- Միշտ համոզվիր, որ input-ը focus ունի (click/TAB)
- sleep()-ի փոխարեն ցանկալի է օգտագործել wait, եթե էջը դանդաղ է
- Եթե typing-ը «չի մտնում» `input`-ի մեջ → օգտագործիր `send_keys_to_element`
