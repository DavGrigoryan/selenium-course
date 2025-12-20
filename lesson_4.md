<h1 align="center" style="color:#2E86C1;">R'SOFT</h1>
<p align="center" style="color:#2E86C1; font-size:20px;">Web Development Company</p>

---

## 📘 Lesson 4 - Interacting with Web Elements (Web էլեմենտների հետ փոխազդեցություն)

### 🌐 Պրակտիկ կայք․ https://www.qa-practice.com/

---

### ✳️ Ինչ է WebElement-ը

WebElement-ը Selenium-ի օբյեկտ է, որը ներկայացնում է էջի որևէ HTML էլեմենտ՝
օրինակ՝

- input
- button
- checkbox
- form
- div և այլն

```python
element = driver.find_element(By.ID, 'some_id')
```

---

### ✳️ WebElement-ի հիմնական 5 գործողությունները

Selenium-ում գոյություն ունի **միայն 5 հիմնական գործողություն**, որոնք կարող ենք անել էլեմենտի վրա։

| Գործողություն | Նկարագրություն                                         |
|:--------------|:-------------------------------------------------------|
| `click()`     | Սեղմում է էլեմենտը                                     |
| `send_keys()` | Տեքստ է գրում կամ ստեղնաշարի հրաման է ուղարկում        |
| `clear()`     | Մաքրում է input-ի պարունակությունը                     |
| `submit()`    | Ուղարկում է form-ը (չի խորհուրդ տրվում Selenium 4-ում) |
| `select`      | Dropdown-ի համար (առանձին թեմա)                        |

---

### ✳️ Click գործողություն

Ինչպես է աշխատում

- click-ը կատարվում է **էլեմենտի կենտրոնի վրա**
- եթե կենտրոնը ծածկված է (overlay, modal, loader) → error

```python
# Navigate to URL
driver.get("https://www.selenium.dev/selenium/web/inputs.html")

# Click on the checkbox
check_input = driver.find_element(By.NAME, "checkbox_input")
check_input.click()
```

**Հնարավոր error-ներ`**

- ElementClickInterceptedException
- ElementNotInteractableException

---

### ✳️ Send Keys (տեքստ մուտքագրում)

Ինչ է անում

- գրում է տեքստ input կամ content-editable էլեմենտի մեջ
- կարող է ուղարկել նաև ստեղնաշարի հրամաններ (ENTER, TAB և այլն)

```python
# Handle the email input field
email_input = driver.find_element(By.NAME, "email_input")
email_input.clear()  # Clear field

email = "admin@localhost.dev"
email_input.send_keys(email)  # Enter text
```

**Հնարավոր error-ներ`**

- Եթե էլեմենտը editable չէ → InvalidElementStateException

---

### ✳️ Clear (մաքրում)

Ինչ է անում

- մաքրում է input-ի կամ content-editable դաշտի պարունակությունը

```python
email_input.clear()
```

Պահանջներ

- պետք է լինի editable
- պետք է լինի resettable
- Հակառակ դեպքում → error

### Submit (Form ուղարկում) ⚠️

Selenium 4-ում `submit()`-ը այլևս չի աշխատում որպես առանձին WebDriver հրաման։

**📌 Խորհուրդ է տրվում չօգտագործել։**

**Ճիշտ մոտեցում**

```python
submit_button = driver.find_element(By.ID, 'submit')
submit_button.click()
```

---

### ✳️ Ինչու երբեմն գործողությունները ձախողվում են

Ամենատարածված պատճառները․

- էլեմենտը դեռ չի երևում
- overlay կամ loader կա
- էջը դեռ render է անում
- DOM-ը փոխվել է (stale element)

**👉** Լուծում՝ **Explicit Wait + ճիշտ Expected Condition**

---

### ✳️ Լավ պրակտիկա (Best Practices)

- **✅** Միշտ սպասիր՝ մինչև էլեմենտը պատրաստ լինի
- **✅** Օգտագործիր Explicit Wait
- **❌** Մի օգտագործիր `time.sleep()`
- **❌** Մի վստահիր միայն page load-ին

Օրինակ՝

```python
wait.until(EC.element_to_be_clickable((By.ID, 'submit')))
```
