<h1 align="center" style="color:#2E86C1;">R'SOFT</h1>
<p align="center" style="color:#2E86C1; font-size:20px;">Web Development Company</p>

---

## 📘 Lesson 1 - Web Automation with Python & Selenium

### 📌 Թեմա․ Selenium WebDriver-ի հիմունքներ

### 🌐 Պրակտիկ կայք․ https://www.qa-practice.com/

---

### 🛠️ 1. Աշխատանքային միջավայրի նախապատրաստում

#### 1.1 Python միջավայր

- Տեղադրել Python 3.x
- Ստուգել տեղադրումը․

```shell
python --version
pip --version
```

---

#### 1.2 IDE (Ծրագրավորման միջավայր)

Ընտրել որևէ IDE․

- PyCharm (խորհուրդ է տրվում սկսնակներին)
- VS Code

---

#### 1.3 Վիրտուալ միջավայր (venv)

Վիրտուալ միջավայրը թույլ է տալիս յուրաքանչյուր նախագծի համար ունենալ առանձին փաթեթներ։  
📎 Օգտակար ուղեցույց․

- https://github.com/DavGrigoryan/python-course/blob/main/lesson_37.md

#### 1.4 Selenium-ի տեղադրում

```shell
pip install selenium
```

Տեղադրումից հետո մենք պատրաստ ենք աշխատել Selenium-ի հետ։

---

### 🌐 2․ Selenium WebDriver-ի հիմունքներ

#### 2.1 Selenium-ում բրաուզերի ստեղծում

Սովորաբար Selenium-ում `webdriver`֊ից բրաուզեր ստեղծած փոփոխականի անունը դնում ենք՝ `browser` կամ `driver`։

```python
from selenium import webdriver

browser = webdriver.Chrome()
```

📌 Այս կոդը բացում է Google Chrome բրաուզերը։

---

#### 2.2 Էջ բացել բրաուզերում

```python
browser.get('https://www.qa-practice.com/')
```

---

### 🔍 3․ Web Element-ների որոնում

Վեբ էջում գործողություն կատարելու համար նախ պետք է գտնել էլեմենտը։

#### 3.1 Էլեմենտ գտնել ID-ով

```python
from selenium import webdriver
from selenium.webdriver.common.by import By

browser = webdriver.Chrome()
browser.get('https://www.qa-practice.com/elements/button/simple')

click_button = browser.find_element(By.ID, "submit-id-submit")
```

---

#### 3.2 Գտնված էլեմենտի վրա գործողություն (`click()`)

```python
click_button.click()
```

📌 Սա սեղմում է կոճակը, ինչպես իրական օգտատերը։

---

### 🧭 4․ Էլեմենտների որոնման եղանակներ

Selenium-ը աջակցում է հետևյալ locator-ներին․

- `ID`,
- `XPATH`,
- `LINK_TEXT`,
- `PARTIAL_LINK_TEXT`,
- `NAME`,
- `TAG_NAME`,
- `CLASS_NAME`,
- `CSS_SELECTOR`,

---

#### 4.1 `CLASS_NAME`-ով որոնում

```python
browser.find_element(By.CLASS_NAME, 'btn')
```

📌 Օգտակար է, երբ էլեմենտներն ունեն ընդհանուր CSS class։

---

#### 4.2 `LINK_TEXT`-ով որոնում

```python
browser.find_element(By.LINK_TEXT, 'Contact')
```

📌 Օգտագործվում է `<a>` tag հղումների համար։

---

#### `CSS_SELECTOR`-ով որոնում

**օրինակ 1՝**

```python
browser.find_element(By.CSS_SELECTOR, 'input[class="btn btn-primary"]')
```

**օրինակ 2՝**

```python
browser.find_element(By.CSS_SELECTOR, 'input#submit-id-submit.btn.btn-primary')
```

📌 CSS Selector-ը շատ հզոր և հաճախ օգտագործվող մեթոդ է։

---

### 4.4 `XPATH`-ով որոնում

```python
browser.find_element(By.XPATH, '//input[@class="btn btn-primary"]')
```

📌 XPath-ը թույլ է տալիս գտնել էլեմենտներ բարդ կառուցվածք ունեցող էջերում։

---

## 🧪 Տնային

### 🔹 Գրել 5 հատ ծրագիր, որը․

1. Բացում է https://www.qa-practice.com
2. Գտնում է որևէ կոճակ
3. Սեղմում է այն
