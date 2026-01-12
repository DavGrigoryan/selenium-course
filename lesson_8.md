<h1 align="center" style="color:#2E86C1;">R'SOFT</h1>
<p align="center" style="color:#2E86C1; font-size:20px;">Web Development Company</p>

---

## 📘 Lesson 7 - Scroll Wheel Actions Selenium-ում (v4.2)

### 🌐 Պրակտիկ կայք․ https://www.qa-practice.com/

### ⚠️ Նշում

Scroll Wheel Actions-ը աշխատում է՝

- **Selenium ≥ 4.2**
- **Chromium-based browser-ներում** (Chrome, Edge)

---

### ✳️ Ինչ է Scroll Wheel Action-ը

Scroll Wheel Action-ը Selenium-ում ներկայացնում է մկնիկի scroll wheel-ի վարքը, որը թույլ է տալիս՝

- Scroll անել էջը
- Scroll անել կոնկրետ element-ի նկատմամբ
- Կառավարել scroll-ի ուղղությունն ու քանակը
- Աշխատել lazy-load / infinite-scroll էջերի հետ

📌 Սա իրականացվում է `ActionChains`-ի միջոցով։

---

### ❗ Կարևոր տարբերություն

- **❌** `click()` և `send_keys()` մեթոդները **միշտ ավտոմատ scroll չեն անում** element-ի մոտ
- **✅** Scroll actions-ը թույլ է տալիս ինքնուրույն վերահսկել scroll-ը

---

### 🔁 Scroll-ի հիմնական 5 սցենարները

---

### 1️⃣ Scroll to element

### ✳️ Ամենատարածված սցենարը

Այս մեթոդը օգտագործվում է, երբ element-ը viewport-ում չէ։

📌 Viewport-ը ավտոմատ scroll է արվում այնպես, որ
**element-ի ներքևի մասը հայտնվի էկրանի ներքևում**

---

### 🔹 Python օրինակ

```python
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.get('https://www.selenium.dev/selenium/web/scroll3.html')

button = driver.find_element(By.ID, "button1")

ActionChains(driver).scroll_to_element(button).perform()
```

📌

- Չի անում hover 
- Միայն ապահովում է element-ի տեսանելի լինելը

---

### 2️⃣ Scroll by given amount
### ✳️ Scroll կոնկրետ քանակով

Օգտագործվում է, երբ ուզում ենք scroll անել՝

- ներքև / վերև
- անկախ կոնկրետ element-ից

📌 delta արժեքները ներկայացնում են պիքսելներ

```python
from selenium import webdriver
from selenium.webdriver import ActionChains

driver = webdriver.Chrome()
driver.get('https://www.selenium.dev/selenium/web/scroll3.html')

ActionChains(driver)\
    .scroll_by_amount(500, 1000)\
    .perform()
```

📌
- `delta_x` X֊երի առանցք (Հորիզոնական)
- `delta_y` Y֊ների առանցք (Ուղղահայաց)

### 3️⃣ Scroll from element by given amount
### ✳️ Համակցված սցենար

Այս մեթոդը՝
- 1️⃣ Նախ scroll է անում element-ը viewport
- 2️⃣ Հետո scroll է անում տրված քանակով

📌 Օգտագործվում է, երբ element-ը reference point է

---

### 🔹 Python օրինակ

```python
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.actions.wheel_input import ScrollOrigin
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.get('https://www.selenium.dev/selenium/web/scroll3.html')

button = driver.find_element(By.ID, "button1")
scroll_origin = ScrollOrigin.from_element(button)

ActionChains(driver)\
    .scroll_from_origin(scroll_origin, 100, 500)\
    .perform()
```