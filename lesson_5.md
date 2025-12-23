<h1 align="center" style="color:#2E86C1;">R'SOFT</h1>
<p align="center" style="color:#2E86C1; font-size:20px;">Web Development Company</p>

---

## 📘 Lesson 5 - Working with Select Lists (Dropdown-ներ Selenium-ում)

### 🌐 Պրակտիկ կայք․ https://www.qa-practice.com/

---

### ✳️ Ինչ է Select List (Dropdown)

Select list-ը (`<select>`) HTML էլեմենտ է, որը թույլ է տալիս օգտատիրոջը ընտրել մեկ կամ մի քանի տարբերակ dropdown ցանկից։

Օրինակ HTML․

```html
<select name="selectomatic">
    <option value="one">One</option>
    <option value="two">Two</option>
    <option value="four">Four</option>
</select>
```

**❗ Select list-ը ունի հատուկ վարքագիծ, և Selenium-ում դրա համար գոյություն ունի հատուկ class։**

---

### ✳️ Ինչու չենք աշխատում սովորական click()-ով

Dropdown-ը չի աշխատում ինչպես button կամ input։

- Սովորական `click()`-ը բավարար չէ
- Selenium-ը տրամադրում է հատուկ Select class
- Այդ class-ը աշխատում է միայն իրական HTML `<select>`-ների հետ

---

### ⚠️ Կարևոր սահմանափակում

🔴 `Select` class-ը աշխատում է միայն հետևյալ tag-երի հետ․

- `<select>`

- `<option>`

**❌** Չի աշխատում JavaScript-ով ստեղծված dropdown-ների հետ (`div`, `li`, custom UI)

---

### ✳️ Select class-ի ներմուծում

```python
from selenium.webdriver.support.ui import Select
```

---

### ✳️ Select object-ի ստեղծում

Նախ պետք է գտնենք `<select>` էլեմենտը, հետո ստեղծենք Select object։

```python
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import Select

select_element = driver.find_element(By.NAME, 'selectomatic')
select = Select(select_element)
```

**📌 Selenium 4.5+** տարբերակներում՝  
եթե `<select>`-ը disabled է → Select object չի ստեղծվի։

---

### 🔹 Select List-ների տեսակները

**1️⃣ Single Select (մեկ ընտրություն)**

Սա սովորական dropdown-ն է, որտեղ կարող ենք ընտրել միայն մեկ տարբերակ։

```html
<select name="selectomatic">
    <option value="one">One</option>
    <option value="two">Two</option>
</select>
```

📌 Երբ ընտրում ենք նոր option → հինը ավտոմատ փոխվում է։

---

**2️⃣ Multiple Select (բազմակի ընտրություն)**

Այս տեսակը թույլ է տալիս ընտրել մի քանի option միաժամանակ։

```html
<select name="multi" multiple="multiple">
    <option value="eggs">Eggs</option>
    <option value="ham">Ham</option>
    <option value="sausages">Sausages</option>
</select>
```

📌 Սա աշխատում է միայն եթե կա `multiple` attribute։

### ✳️ Ստանալ բոլոր option-ների ցանկը

```python
options = select.options
```

Սա վերադարձնում է `<option>` WebElement-ների list։

Օրինակ՝

```python
for option in options:
    print(option.text)
```

---

### ✳️ Ստանալ ընտրված option-ները

```python
selected_options = select.all_selected_options
```

📌

- Single select → list-ը կունենա 1 էլեմենտ
- Multiple select → կարող է ունենալ 0, 1 կամ մի քանի

---

### ✳️ Option ընտրելու եղանակները

Select class-ը տրամադրում է **3 հիմնական մեթոդ**։

---

### 🔹 1․ Ըստ տեսանելի տեքստի (visible text)

```python
select.select_by_visible_text('Python')
```

📌 Սա ամենահաճախ օգտագործվող և ամենաընթեռնելի տարբերակն է։

---

### 🔹 2․ Ըստ value attribute-ի

```python
select.select_by_value('1')
```

📌 Օգտագործվում է, երբ գիտենք option-ի value-ն։

### 🔹 3․ Ըստ index-ի

```python
select.select_by_index(0)
```

📌 Index-ը սկսվում է `0-ից`։

**⚠️** Խորհուրդ չի տրվում, քանի որ `option`-ների հերթականությունը կարող է փոխվել։

---

### ⚠️ Disabled option-ներ

Եթե `option`-ը ունի `disabled` attribute, Selenium-ը **չի թույլատրում ընտրել այն**։

```html
<option value="disabled" disabled="disabled">Disabled</option>
```

```python
select.select_by_value('disabled')  # Error
```

📌 Սա Selenium-ի պաշտպանիչ վարքագիծ է։

---

### ✳️ Option-ների deselect (միայն Multiple Select)

Միայն **multiple select** dropdown-ների դեպքում կարող ենք հանել ընտրությունը։

```python
select.deselect_by_value('eggs')
```

Կամ բոլորն ընտրվածները միանգամից հանել․

```python
select.deselect_all()
```

**❌** Single select-ի դեպքում սա կգեներացնի error։

---

### ✳️ Օգտակար հատկություններ

```python
select.is_multiple
```

Վերադարձնում է `True` կամ `False`՝
արդյոք dropdown-ը multi է։
