<h1 align="center" style="color:#2E86C1;">R'SOFT</h1>
<p align="center" style="color:#2E86C1; font-size:20px;">Web Development Company</p>

---

## 📘 Lesson 12 — iframe Selenium-ում (Frame Switching)

### 🌐 Պրակտիկ կայք․ https://www.qa-practice.com/elements/iframe/iframe_page

---

### ✳️ Ինչ է iframe-ը

`iframe`-ը էջի ներսում այլ էջ է՝ իր առանձին HTML DOM-ով։

📌 Կարևոր միտք՝ iframe-ի ներսի element-ները “տեսանելի չեն” հիմնական էջի DOM-ից, մինչև դու չանցնես այդ iframe-ի context-ը։

---

### ❗ Ինչու է Selenium-ում խնդիր առաջանում

Եթե iframe-ի ներսում կա #button կամ #textInput, բայց դու փորձում ես սա անել՝

```python
driver.find_element(...)
```

հաճախ ստանում ես `NoSuchElementException`, որովհետև Selenium-ը դեռ գտնվում է main page context-ում, իսկ element-ը
frame-ի ներսում է։

**✅** Լուծումը՝ նախ անցնել iframe-ի մեջ.

---

### ✅ Ամենակարևոր սինթաքսը

#### 1) Մուտք iframe (switch)

Կարող ես անցնել iframe-ի մեջ 3 տարբերակով՝

**✅ a) Frame-ը որպես WebElement (ամենակայունը)**

```python
frame = driver.find_element(By.TAG_NAME, "iframe")
driver.switch_to.frame(frame)
```

**b) Frame-ը ըստ name կամ id**

```python
driver.switch_to.frame('frameNameOrId')
```

**c) Frame-ը ըստ index (ոչ ցանկալի՝ fragile է)**

```python
driver.switch_to.frame(0)
```

---

#### 2) Դուրս գալ iframe-ից

**✅ Վերադառնալ հիմնական էջ**

```python
driver.switch_to.default_content()
```

Վերադառնալ parent frame (երբ nested frames կան)

```python
driver.switch_to.parent_frame()
```

---

### 🔁 Պրակտիկ օրինակ — գտնել էլեմենտը iframe-ի ներսում

եթե փորձենք վերցնել `btn-secondary` էլեմենտը, որը iframe֊ի ներսում է այս եղանակով կստանանք ERROR:

```python
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.implicitly_wait(10)
driver.maximize_window()

driver.get("https://www.qa-practice.com/elements/iframe/iframe_page")
button = driver.find_element(By.CLASS_NAME, "btn-secondary")

print(button.text)
```

`btn-secondary` էլեմենտը ճիշտ վերցնելու համար անհրաժեշտ է գրել այսպես՝

```python
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.implicitly_wait(10)
driver.maximize_window()

driver.get("https://www.qa-practice.com/elements/iframe/iframe_page")

iframe = driver.find_element(By.TAG_NAME, "iframe")
driver.switch_to.frame(iframe)
button = driver.find_element(By.CLASS_NAME, "btn-secondary")

print(button.text)
```

---

### ⚠️ Ամենատարածված սխալները

#### ✅ Սխալ 1 — Փորձել element գտնել frame-ի ներսից՝ առանց switch-ի

Արդյունք → `NoSuchElementException`

**✅** Միշտ հիշիր՝ iframe-ի ներսում element գտնելուց առաջ՝ `switch_to.frame(...)`

---

#### ✅ Սխալ 2 — Մոռանալ դուրս գալ iframe-ից

Եթե մնաս iframe-ում ու հետո փորձես main page-ի element գտնել՝ նորից `NoSuchElementException` կստանաս։

**✅** Լուծում՝ գործողությունից հետո միշտ՝

```python
driver.switch_to.default_content()
```

---

#### ✅ Սխալ 3 — Nested iframe-ներ (iframe-ի ներսում iframe)

Այս դեպքում պետք է հերթով անցնես ներս՝ 1-ին frame → հետո 2-րդ frame։

```python
driver.switch_to.frame(frame1)
driver.switch_to.frame(frame2)
# ... աշխատում ես
driver.switch_to.parent_frame()  # դուրս 2-րդից
driver.switch_to.default_content()  # դուրս main
```

---

### ✅ Լավ պրակտիկա (Important)

**✅** Frame-ը վերցրու որպես WebElement (CSS/XPath) → ավելի կայուն է, քան index-ը  
**✅** Օգտագործիր `WebDriverWait` → iframe-ը կարող է ուշ բեռնվել  
**✅** Ամեն frame action-ից հետո սովորություն դարձրու `default_content()`



