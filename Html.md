# Html Cheatsheet

HTML5 is the latest version of the HyperText Markup Language (HTML), used for structuring and presenting content on the web. It introduces new elements, APIs, and semantic improvements over previous versions.

---

## Table of Contents

1. [Introduction to HTML5](#introduction-to-html5)
2. [HTML5 Document Structure](#html5-document-structure)
3. [HTML5 Semantic Elements](#html5-semantic-elements)
4. [Text Content Elements](#text-content-elements)
5. [Multimedia Elements](#multimedia-elements)
6. [Forms in HTML5](#forms-in-html5)
7. [HTML5 APIs](#html5-apis)
8. [HTML5 Best Practices](#html5-best-practices)
9. [Conclusion](#conclusion)

---

## Introduction to HTML5

HTML5 is a **markup language** for creating web pages. Its main goals are:

* **Better structure** using semantic elements.
* **Rich multimedia support** without third-party plugins.
* **Improved interactivity** through APIs like canvas, local storage, and geolocation.
* **Backward compatibility** with older browsers.

**Advantages of HTML5:**

* Cleaner and more meaningful code.
* Better SEO due to semantic tags.
* Mobile-friendly and responsive.
* Supports audio, video, and graphics natively.

---

## HTML5 Document Structure

Every HTML5 page starts with a **DOCTYPE declaration** and follows a basic structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML5 Tutorial</title>
</head>
<body>
    <header>
        <h1>Welcome to HTML5</h1>
    </header>
    <main>
        <p>This is a sample HTML5 page.</p>
    </main>
    <footer>
        <p>© 2025 My Website</p>
    </footer>
</body>
</html>
```

**Key Points:**

* `<!DOCTYPE html>`: Declares HTML5.
* `<html lang="en">`: Sets language for accessibility.
* `<meta charset="UTF-8">`: Ensures proper text encoding.
* `<meta name="viewport">`: Makes pages responsive on mobile devices.

---

## HTML5 Semantic Elements

Semantic elements provide meaning to the page structure.

| Element     | Purpose                      | Example                                 |
| ----------- | ---------------------------- | --------------------------------------- |
| `<header>`  | Header of a page or section  | `<header><h1>Title</h1></header>`       |
| `<nav>`     | Navigation links             | `<nav><a href="#">Home</a></nav>`       |
| `<main>`    | Main content                 | `<main><p>Content goes here</p></main>` |
| `<article>` | Self-contained content       | `<article><h2>Blog Post</h2></article>` |
| `<section>` | Thematic grouping            | `<section><h3>Features</h3></section>`  |
| `<aside>`   | Side content, like a sidebar | `<aside><p>Related Links</p></aside>`   |
| `<footer>`  | Footer content               | `<footer><p>© 2025</p></footer>`        |

---

## Text Content Elements

HTML5 provides many elements for formatting and structuring text:

| Element        | Description      | Example                               |
| -------------- | ---------------- | ------------------------------------- |
| `<h1>`–`<h6>`  | Headings         | `<h1>Main Title</h1>`                 |
| `<p>`          | Paragraph        | `<p>This is a paragraph.</p>`         |
| `<strong>`     | Important text   | `<strong>Important!</strong>`         |
| `<em>`         | Emphasized text  | `<em>Emphasized</em>`                 |
| `<mark>`       | Highlighted text | `<mark>Highlighted</mark>`            |
| `<blockquote>` | Quoted content   | `<blockquote>Quote here</blockquote>` |
| `<code>`       | Inline code      | `<code>print("Hello")</code>`         |

---

## Multimedia Elements

HTML5 natively supports multimedia:

### Audio

```html
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
</audio>
```

### Video

```html
<video width="640" height="360" controls>
    <source src="video.mp4" type="video/mp4">
    Your browser does not support the video element.
</video>
```

### Canvas

Used for drawing graphics via JavaScript:

```html
<canvas id="myCanvas" width="200" height="100" style="border:1px solid #000;"></canvas>

<script>
    const canvas = document.getElementById('myCanvas');
    const ctx = canvas.getContext('2d');
    ctx.fillStyle = 'blue';
    ctx.fillRect(10, 10, 150, 80);
</script>
```

---

## Forms in HTML5

HTML5 forms include new input types, validation, and placeholders:

```html
<form>
    <label for="name">Name:</label>
    <input type="text" id="name" name="name" required>

    <label for="email">Email:</label>
    <input type="email" id="email" name="email" placeholder="example@mail.com" required>

    <label for="birthday">Birthday:</label>
    <input type="date" id="birthday" name="birthday">

    <input type="submit" value="Submit">
</form>
```

**New Input Types:**

* `email`, `url`, `number`, `range`, `date`, `color`, `tel`, `search`.

---

## HTML5 APIs

HTML5 introduces powerful APIs to enhance web functionality:

| API                 | Purpose                               |
| ------------------- | ------------------------------------- |
| **Canvas API**      | Draw graphics on a web page           |
| **Geolocation API** | Get user location                     |
| **LocalStorage**    | Store data on the client              |
| **SessionStorage**  | Store temporary session data          |
| **Drag & Drop API** | Enable drag-and-drop functionality    |
| **Web Workers**     | Run background scripts                |
| **WebSocket**       | Real-time communication with a server |

**Example – LocalStorage:**

```html
<script>
    localStorage.setItem('username', 'Chris');
    alert(localStorage.getItem('username'));
</script>
```

---

## HTML5 Best Practices

1. **Use semantic tags** to improve accessibility and SEO.
2. **Keep content responsive** using meta viewport and CSS.
3. **Use new form types** for better user experience.
4. **Validate HTML** using [W3C validator](https://validator.w3.org/).
5. **Use external CSS and JavaScript** to separate concerns.
6. **Optimize media** for faster page load.

---

## Conclusion

HTML5 is not just an update of HTML; it is a **complete improvement for modern web development**, offering better structure, multimedia support, APIs, and interactivity. Mastering HTML5 is essential for building modern, accessible, and responsive web applications.
