# CSS Cheatsheet

CSS (Cascading Style Sheets) is used to style HTML elements. It controls layout, colors, fonts, spacing, animations, and more. CSS works alongside HTML and JavaScript to create visually engaging web pages.

---

## 1. Introduction

CSS separates content (HTML) from presentation (CSS).

**Example:**

```html
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="style.css">
    <title>CSS Example</title>
</head>
<body>
    <h1>Hello, World!</h1>
</body>
</html>
```

```css
/* style.css */
h1 {
    color: blue;
    text-align: center;
}
```

---

## 2. CSS Syntax and Selectors

**Basic syntax:**

```css
selector {
    property: value;
}
```

**Selectors:**

* **Element selector:** `p { color: red; }`
* **Class selector:** `.btn { padding: 10px; }`
* **ID selector:** `#header { background: yellow; }`
* **Universal selector:** `* { margin: 0; padding: 0; }`
* **Group selector:** `h1, h2, h3 { font-family: Arial; }`
* **Descendant selector:** `div p { color: green; }`
* **Child selector:** `ul > li { list-style: none; }`
* **Attribute selector:** `[type="text"] { border: 1px solid #ccc; }`

---

## 3. Colors and Fonts

**Colors:**

```css
body {
    color: #333;          /* Hex */
    background-color: rgb(240, 240, 240); /* RGB */
    border-color: hsl(120, 50%, 50%); /* HSL */
}
```

**Fonts:**

```css
p {
    font-family: "Arial", sans-serif;
    font-size: 16px;
    font-weight: bold;
    font-style: italic;
}
```

**Text styling:**

```css
h1 {
    text-align: center;
    text-decoration: underline;
    text-transform: uppercase;
    letter-spacing: 2px;
    line-height: 1.5;
}
```

---

## 4. Box Model

Every element is a rectangular box.

**Components:**

1. Content
2. Padding
3. Border
4. Margin

```css
div {
    width: 200px;
    height: 100px;
    padding: 10px;
    border: 5px solid black;
    margin: 20px;
}
```

**Tip:** Use `box-sizing: border-box;` to include padding and border in the total width.

---

## 5. Layout Techniques

* **Inline vs Block**: `span` is inline, `div` is block.
* **Display property**:

```css
div {
    display: block;
}

span {
    display: inline-block;
}
```

* **Visibility & Opacity**:

```css
.hidden {
    display: none;
}

.transparent {
    opacity: 0.5;
}
```

---

## 6. Positioning

* **Static (default)**: Follows normal flow.
* **Relative**: Moves relative to its normal position.
* **Absolute**: Positioned relative to the nearest positioned ancestor.
* **Fixed**: Stays fixed relative to viewport.
* **Sticky**: Acts like relative until a scroll threshold is reached.

```css
.relative {
    position: relative;
    top: 10px;
    left: 20px;
}
```

---

## 7. Flexbox

Flexbox is for one-dimensional layouts (row or column).

```css
.container {
    display: flex;
    flex-direction: row; /* row | column */
    justify-content: center; /* start | center | end | space-between | space-around */
    align-items: center; /* start | center | end | stretch */
}
```

**Flex items:**

```css
.item {
    flex: 1; /* grows to fill space */
}
```

---

## 8. Grid

CSS Grid is for two-dimensional layouts (rows and columns).

```css
.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 10px;
}

.item {
    background-color: lightblue;
}
```

---

## 9. Responsive Design

Use **media queries** to adapt to different screens:

```css
@media (max-width: 768px) {
    body {
        font-size: 14px;
    }
    .container {
        flex-direction: column;
    }
}
```

---

## 10. Pseudo-classes and Pseudo-elements

* **Pseudo-classes:** `:hover`, `:focus`, `:nth-child(2)`
* **Pseudo-elements:** `::before`, `::after`, `::first-letter`

```css
a:hover {
    color: red;
}

p::first-letter {
    font-size: 200%;
    color: blue;
}
```

---

## 11. Transitions and Animations

**Transitions**:

```css
button {
    background-color: blue;
    transition: background-color 0.3s ease;
}

button:hover {
    background-color: green;
}
```

**Animations**:

```css
@keyframes slide {
    from { transform: translateX(0); }
    to { transform: translateX(100px); }
}

.box {
    animation: slide 2s infinite alternate;
}
```

---

## 12. Advanced CSS Techniques

* **Custom properties (CSS variables):**

```css
:root {
    --main-color: #3498db;
}

p {
    color: var(--main-color);
}
```

* **Clipping and masking:** `clip-path`, `mask`
* **CSS functions:** `calc()`, `min()`, `max()`, `clamp()`
* **CSS shapes:** `border-radius`, `clip-path: circle()`
* **Filters:** `filter: blur(5px) brightness(120%)`
* **Blend modes:** `mix-blend-mode: multiply`

---

## 13. Best Practices

* Keep CSS modular with separate files or components.
* Use semantic class names.
* Avoid overusing `!important`.
* Use shorthand properties for readability.
* Test across multiple browsers.
* Use responsive units (`%, em, rem, vh, vw`) instead of fixed pixels when possible.

---

This tutorial covers **CSS from beginner to advanced level**. Once you master these, you can combine CSS with frameworks like **Tailwind, Bootstrap, or Material UI** for faster development.

---