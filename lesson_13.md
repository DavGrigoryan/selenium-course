<h1 align="center" style="color:#2E86C1;">R'SOFT</h1>
<p align="center" style="color:#2E86C1; font-size:20px;">Web Development Company</p>

---

## 📘 Lesson 13 — Window Handles (Tab/Window) և switch_to.window()

### 🌐 Պրակտիկ կայք․ https://www.qa-practice.com/elements/new_tab/link

---

### ✳️ Ի՞նչ է Window handle-ը

Window handle-ը browser-ի յուրաքանչյուր window/tab-ի համար յուրահատուկ id (string) է։

Selenium-ում՝

- `driver.current_window_handle` → ընթացիկ tab/window-ի handle-ը
- `driver.window_handles` → բացված բոլոր tab/window-ների handle-ների ցուցակը (list)

📌 Սա կապ չունի iframe-ի հետ.

- iframe → նույն էջի ներսում “ներս” ես մտնում (`switch_to.frame`)
- new tab/window → browser-ում բացվել է ուրիշ պատուհան/տաբ (`switch_to.window`)

---

### ✅ Ամենակարևոր սինթաքսը

1) Պահել current window-ը

```python
main = driver.current_window_handle
```

2) Ստանալ բոլոր window-ները

```python
handles = driver.window_handles
```

3) Switch անել նոր window/tab-ի վրա

```python
driver.switch_to.window(new_handle)
```

4) Փակել նոր window/tab-ը և վերադառնալ հիմնականին

```python
driver.close()
driver.switch_to.window(main)
```