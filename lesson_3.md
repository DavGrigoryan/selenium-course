<h1 align="center" style="color:#2E86C1;">R'SOFT</h1>
<p align="center" style="color:#2E86C1; font-size:20px;">Web Development Company</p>

---

## 📘 Lesson 3 - Waiting Strategies (Սպասման ռազմավարություններ)

### 🌐 Պրակտիկ կայք․ https://www.qa-practice.com/

---

### ✳️ Ինչու է սպասումը կարևոր Selenium-ում

Բրաուզերի ավտոմատացման ամենատարածված խնդիրներից մեկը այն է, որ Selenium-ը փորձում է աշխատել էջի հետ նախքան այն լիովին
պատրաստ լինելը։

- երբեմն էջը հասցնում է բեռնվել → թեստը աշխատում է
- երբեմն Selenium-ը «շտապում է» → թեստը ձախողվում է

**❕ Արդյունքում ստանում ենք անկայուն (flaky) թեստեր։**

---

### Դինամիկ էջերի խնդիրը (SPA, JavaScript)

Ժամանակակից վեբ հավելվածներում (SPA, React, Vue և այլն) հաճախ՝

- էլեմենտները ստեղծվում են ուշ
- կամ սկզբում թաքնված են (`display: none`)
- կամ հայտնվում են գործողությունից հետո (click)

**❕ Selenium-ը կարող է տեսնել էջը «բեռնված», բայց անհրաժեշտ էլեմենտը դեռ չլինեն։**

---

### Օրինակ իրական խնդրի

Օրինակ՝ էջում կան երկու կոճակներ․

- **Add a box!** → ստեղծում է նոր `div`
- **Reveal a new input** → ցույց է տալիս թաքնված `input`

Այս գործողությունները տևում են մի քանի վայրկյան։  
Եթե Selenium-ը անմիջապես փորձի աշխատել այդ էլեմենտների հետ → կստանա սխալ։

---

### Վատ լուծում՝ `sleep()`

Շատ սկսնակներ օգտագործում են․

```python
time.sleep(2)
```

Խնդիրներ․

- չգիտենք՝ 2 վայրկյանը բավարար է ?
- եթե քիչ է → թեստը կձախողվի
- եթե շատ է → թեստերը կդառնան դանդաղ
- շատ sleep → շատ վատ պրակտիկ

---

### ✳️ Selenium-ի ճիշտ լուծումները

Selenium-ը տրամադրում է 2 հիմնական սպասման մեխանիզմ՝

- **Implicit Wait**
- **Explicit Wait**

---

### ✳️ Implicit Wait (Անուղղակի սպասում)

Implicit wait-ը Selenium-ին ասում է՝  
👉 «Եթե էլեմենտը անմիջապես չգտնես, սպասիր մինչև X վայրկյան»։

Սա գլոբալ կարգավորում է, որը վերաբերում է ամբողջ session-ին։

```python
driver.implicitly_wait(2)
```

**Ինչպես է աշխատում**

- Եթե էլեմենտը կա → անմիջապես կգտնվի
- Եթե չկա → Selenium-ը կփորձի մինչև 2 վայրկյան
- Եթե չգտնվի → error

**⚠️** Սա **չի նշանակում**, որ միշտ սպասում է 2 վայրկյան։

---

### ✳️ Explicit Wait (Ուղղակի սպասում) ⭐️ Լավագույն մոտեցում

Explicit wait-ը թույլ է տալիս սպասել **հստակ պայմանին**, օրինակ՝

- էլեմենտը տեսանելի լինի
- clickable լինի
- text հայտնվի
- attribute փոխվի

**❕** Նմանատիպ պատրաստի պայմանները կարող եք տեսնել
այստեղ՝ [Expected Conditions](https://www.selenium.dev/selenium/docs/api/py/selenium_webdriver_support/selenium.webdriver.support.expected_conditions.html)

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait

driver = webdriver.Chrome()  # seconds
driver.get("https://www.qa-practice.com/elements/input/simple")

wait = WebDriverWait(driver, 10)
button = wait.until(
    EC.visibility_of_element_located((By.ID, 'id_text_string'))
)
```

**✔️** Selenium-ը պարբերաբար կստուգի պայմանը  
**✔️** Երբ պայմանը true դառնա → կշարունակի  
**❌** Եթե ժամանակը անցնի → TimeoutException

---

### Advanced Customization (Մասնագիտական մակարդակ)

Explicit wait-ը կարող ենք հարմարեցնել՝

- 🔁 polling interval (որքան հաճախ ստուգել)
- 🚫 ignored exceptions
- ⏱ timeout
- 📝 custom error message

```python
wait = WebDriverWait(
    driver,
    timeout=2,
    poll_frequency=0.2,
    ignored_exceptions=errors  # օրինակ՝ TimeoutException
)
```
