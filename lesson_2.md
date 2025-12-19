<h1 align="center" style="color:#2E86C1;">R'SOFT</h1>
<p align="center" style="color:#2E86C1; font-size:20px;">Web Development Company</p>

---

## 📘 Lesson 2 - Css Selectors

### 🌐 Պրակտիկ կայք․ https://www.qa-practice.com/

---

### HTML֊ի ստեղծում՝

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<p style="font-size: large">My name is Dawud</p>
<p style="font-size: x-small; color: blue">I am a developer</p>
<p style="font-size: x-small; color: blue">I Love Python</p>
</body>
</html>
```

---

### Css - cascading style sheets

1. Ստեղծել `styles.css` ֆաիլը՝

```css
.tiny-blue {
    font-size: x-small;
    color: blue;
}
```

2. փոփոխել `index.html` ֆաիլը և միացնել `styles.css` ֆաիլը

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="styles.css">
    <title>Title</title>
</head>
<body>
<p style="font-size: large">My name is Dawud</p>
<p class="tiny-blue">I am a developer</p>
<p class="tiny-blue">I Love Python</p>
</body>
</html>
```

### Css selector֊ում attribute֊ները կարող ենք գտնել մի քանի ձևով՝

- երբ հստակ գիտենք անունը՝

```text
[type="hidden"]
```

- երբ գիտենք որ պարունակում է օրինակ `"idde"` բայց չգիտենք սկիզբը և վերջը՝

```text
[type*="idde"]
```

- երբ գիտենք որ սկսվում ա `"hidd"`֊ով

```text
[type^="hidd"]
```

- երբ գիտենք որ վերջանում ա `"dden"`֊ով

```text
[type$="dden"]
```








