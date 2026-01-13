<h1 align="center" style="color:#2E86C1;">R'SOFT</h1>
<p align="center" style="color:#2E86C1; font-size:20px;">Web Development Company</p>

---

## 📘 Lesson 9 - Mouse Actions Selenium-ում (Actions API)

### 🌐 Պրակտիկ կայք․ https://www.qa-practice.com/

### ⚠️ Նշում

📌 Շատ կարևոր է՝ որոշ mouse գործողություններ error կտան, եթե element-ը viewport-ում չէ։

---

### ✳️ Ինչ է Mouse Action-ը

Mouse-ի իրական «հիմնական» գործողությունները ընդամենը 3-ն են՝

- `pointer_down` → կոճակը սեղմել
- `pointer_up` → կոճակը բաց թողնել
- `move` → մկնիկը շարժել

📌 Selenium-ը տալիս է մեթոդներ, որոնք այս 3 գործողությունները միացնում են ամենատարածված սցենարների համար։

---

### ❗ Կարևոր տարբերություն

- `element.click()` → “սովորական” click (հաճախ բավական է)
- `ActionChains` → երբ պետք է hover, drag&drop, right click, offset click, click-and-hold և այլն

📌 Եթե UI-ը կախված է hover-ից կամ dragging-ից, սովորական `click()`-ը հաճախ չի բավարարում։

---

### 🔁 Mouse actions-ի հիմնական սցենարները

---

#### 1️⃣ Click and hold

##### ✳️ Երբ պետք է

- element-ը “focus” տալու համար
- drag սկսելու (click-and-hold) համար

```python
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.maximize_window()
driver.get('https://www.selenium.dev/selenium/web/mouse_interaction.html')

clickable = driver.find_element(By.ID, 'clickable')

ActionChains(driver)\
    .click_and_hold(clickable)\
    .perform()
```

📌 Սա անում է՝

- գնում է element-ի կենտրոն
- սեղմում է Left button-ը ու պահում

---

#### 2️⃣ Click (Click and release)

##### ✳️ Սովորական “click”

```python
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.get('https://www.selenium.dev/selenium/web/mouse_interaction.html')

clickable = driver.find_element(By.ID, 'clickable')

ActionChains(driver)\
    .click(clickable)\
    .perform()
```

📌 Սա նույնն է ինչ “press + release”։

---

#### 3️⃣ Context click (Right click)

##### ✳️ Երբ պետք է

- context menu բացելու
- right-click event-ներ ունեցող UI-ների համար

```python
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.get('https://www.selenium.dev/selenium/web/mouse_interaction.html')

clickable = driver.find_element(By.ID, 'clickable')

ActionChains(driver)\
    .context_click(clickable)\
    .perform()
```

---

#### 4️⃣ Double click

##### ✳️ Երբ պետք է

- double-click event-ով աշխատող element-ների համար (օր․ ֆայլ բացել, edit mode)

```python
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.get('https://www.selenium.dev/selenium/web/mouse_interaction.html')

clickable = driver.find_element(By.ID, 'clickable')

ActionChains(driver)\
    .double_click(clickable)\
    .perform()
```

---

#### 5️⃣ Move to element (Hover)

⚠️ Նշում

Element-ը պետք է **viewport-ում լինի**, հակառակ դեպքում կարող է **error** տալ։

```python
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.get('https://www.selenium.dev/selenium/web/mouse_interaction.html')

hoverable = driver.find_element(By.ID, 'hover')

ActionChains(driver)\
    .move_to_element(hoverable)\
    .perform()
```

📌 Սա սովորաբար օգտագործվում է dropdown/menu-ների hover բացելու համար։

---

#### 6️⃣ Move by offset (տեղաշարժ պիքսելներով)

##### 6.1️⃣ Offset from element

Նախ գնում է element-ի կենտրոն, հետո շարժվում է `x, y` offset-ով։

```python
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.get('https://www.selenium.dev/selenium/web/mouse_interaction.html')

tracker = driver.find_element(By.ID, 'mouse-tracker')

ActionChains(driver)\
    .move_to_element_with_offset(tracker, 8, 0)\
    .perform()
```

📌 `x` → աջ (դրական), ձախ (բացասական)
📌 `y` → ներքև (դրական), վերև (բացասական)

##### 6.2️⃣ Offset from current pointer location

Սա շարժում է մկնիկը իր ընթացիկ դիրքից։

```python
from selenium import webdriver
from selenium.webdriver import ActionChains

driver = webdriver.Chrome()
driver.get('https://www.selenium.dev/selenium/web/mouse_interaction.html')

ActionChains(driver)\
    .move_by_offset(30, -10)\
    .perform()
```

⚠️ Եթե մինչ այդ մկնիկը “չի շարժվել”, սկզբնական դիրքը կարող է համարվել viewport-ի ձախ վերևը։

---

#### 7️⃣ Drag and drop

##### 7.1️⃣ Drag and drop to element

Անում է՝ hold → move → release։

```python
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.get('https://www.selenium.dev/selenium/web/mouse_interaction.html')

draggable = driver.find_element(By.ID, 'draggable')
droppable = driver.find_element(By.ID, 'droppable')

ActionChains(driver)\
    .drag_and_drop(draggable, droppable)\
    .perform()
```

📌 Եթե drag&drop-ը չի աշխատում որոշ կայքերում՝ դա հաճախ լինում է JS/HTML5 հատուկ implementation-ի պատճառով (այն ժամանակ
պետք է այլ մոտեցում՝ click_and_hold + move + release / կամ JS events)։

---

##### 7.2️⃣ Drag and drop by offset

Երբ ուզում ենք տեղափոխել կոնկրետ պիքսելներով։

```python
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.get('https://www.selenium.dev/selenium/web/mouse_interaction.html')

draggable = driver.find_element(By.ID, 'draggable')

ActionChains(driver)\
    .drag_and_drop_by_offset(draggable, 120, 40)\
    .perform()
```

---

#### 8️⃣ Alternate button clicks (Back / Forward)

Mouse-ի button-ները ընդհանուր՝

- `0` — Left (default)
- `1` — Middle (հաճախ unsupported)
- `2` — Right
- `3` — Back
- `4` — Forward

Back/Forward-ի համար սովորաբար օգտագործվում է ActionBuilder։

```python
from selenium import webdriver
from selenium.webdriver.common.actions.action_builder import ActionBuilder
from selenium.webdriver.common.actions.mouse_button import MouseButton

driver = webdriver.Chrome()
driver.get('https://www.selenium.dev/')

action = ActionBuilder(driver)
action.pointer_action.pointer_down(MouseButton.BACK)
action.pointer_action.pointer_up(MouseButton.BACK)
action.perform()
```

---

### ✅ Լավ պրակտիկա (Important)

- Եթե element-ը viewport-ում չէ՝ նախ scroll արա (կամ օգտագործիր scroll actions, ինչպես նախորդ դասում)
- Hover/drag-երում միշտ մտածիր wait-երի մասին (element visible/clickable)
- ActionChains-ը միշտ վերջացնում ենք .perform()-ով





