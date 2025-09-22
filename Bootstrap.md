# Bootstrap Cheatsheet

## **1. Introduction**

**Bootstrap** is a popular front-end framework for developing responsive and mobile-first websites. It provides:

* **CSS classes** for typography, layout, colors, and components.
* **JavaScript plugins** for interactive elements like modals, dropdowns, carousels.
* A **grid system** to create responsive layouts.

**Advantages:**

* Rapid development
* Cross-browser compatibility
* Mobile-first design
* Large community and documentation

---

## **2. Setting Up Bootstrap**

### **2.1 Using CDN (Quick Setup)**

Add these links in your `<head>` for CSS and at the end of `<body>` for JS.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Bootstrap Example</title>
  <!-- Bootstrap CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>

  <!-- Bootstrap JS Bundle (includes Popper) -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

### **2.2 Using NPM**

```bash
npm install bootstrap
```

Then import in your main JS or SCSS file:

```javascript
import 'bootstrap/dist/css/bootstrap.min.css';
import 'bootstrap/dist/js/bootstrap.bundle.min.js';
```

---

## **3. Bootstrap Grid System**

Bootstrap uses a **12-column grid system**. It’s mobile-first and responsive.

### **3.1 Basic Layout**

```html
<div class="container">
  <div class="row">
    <div class="col">Column 1</div>
    <div class="col">Column 2</div>
    <div class="col">Column 3</div>
  </div>
</div>
```

* `container` → adds padding and centers content.
* `row` → wraps columns, provides negative margin for alignment.
* `col` → auto-sized columns.

### **3.2 Responsive Columns**

```html
<div class="row">
  <div class="col-sm-6 col-md-4 col-lg-3">Responsive Column</div>
</div>
```

Breakpoints:

| Prefix | Min-width |
| ------ | --------- |
| `sm`   | 576px     |
| `md`   | 768px     |
| `lg`   | 992px     |
| `xl`   | 1200px    |
| `xxl`  | 1400px    |

---

## **4. Typography & Utilities**

### **4.1 Headings & Paragraphs**

```html
<h1 class="display-1">Display Heading</h1>
<p class="lead">This is a lead paragraph.</p>
```

### **4.2 Text Utilities**

```html
<p class="text-center text-primary">Centered blue text</p>
<p class="text-end text-muted">Right aligned muted text</p>
```

### **4.3 Spacing**

* Margin: `m-1, m-2, m-3, m-4, m-5`
* Padding: `p-1, p-2, p-3, p-4, p-5`
* Sides: `t=top, b=bottom, s=start, e=end, x=horizontal, y=vertical`

Example:

```html
<div class="p-3 mb-2 bg-light text-dark">Padded div</div>
```

---

## **5. Components**

### **5.1 Buttons**

```html
<button class="btn btn-primary">Primary</button>
<button class="btn btn-outline-success">Outline Success</button>
<button class="btn btn-danger btn-lg">Large Danger</button>
```

### **5.2 Navbar**

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <a class="navbar-brand" href="#">Brand</a>
  <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
    <span class="navbar-toggler-icon"></span>
  </button>
  <div class="collapse navbar-collapse" id="navbarNav">
    <ul class="navbar-nav">
      <li class="nav-item"><a class="nav-link active" href="#">Home</a></li>
      <li class="nav-item"><a class="nav-link" href="#">About</a></li>
      <li class="nav-item"><a class="nav-link" href="#">Contact</a></li>
    </ul>
  </div>
</nav>
```

### **5.3 Cards**

```html
<div class="card" style="width: 18rem;">
  <img src="image.jpg" class="card-img-top" alt="Image">
  <div class="card-body">
    <h5 class="card-title">Card Title</h5>
    <p class="card-text">Some quick example text.</p>
    <a href="#" class="btn btn-primary">Go somewhere</a>
  </div>
</div>
```

### **5.4 Alerts**

```html
<div class="alert alert-success" role="alert">
  This is a success alert!
</div>
<div class="alert alert-danger alert-dismissible fade show" role="alert">
  This is a dismissible danger alert!
  <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
</div>
```

### **5.5 Modals**

```html
<button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal">
  Launch Modal
</button>

<div class="modal fade" id="exampleModal" tabindex="-1">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">Modal title</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
      </div>
      <div class="modal-body">Modal content here</div>
      <div class="modal-footer">
        <button class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
      </div>
    </div>
  </div>
</div>
```

---

## **6. Forms**

### **6.1 Basic Form**

```html
<form>
  <div class="mb-3">
    <label for="email" class="form-label">Email address</label>
    <input type="email" class="form-control" id="email">
  </div>
  <div class="mb-3">
    <label for="password" class="form-label">Password</label>
    <input type="password" class="form-control" id="password">
  </div>
  <button type="submit" class="btn btn-primary">Submit</button>
</form>
```

### **6.2 Inline Forms**

```html
<form class="row g-3">
  <div class="col-auto">
    <input type="text" class="form-control" placeholder="Name">
  </div>
  <div class="col-auto">
    <button type="submit" class="btn btn-primary mb-3">Submit</button>
  </div>
</form>
```

---

## **7. Utilities**

### **7.1 Colors**

```html
<p class="text-primary">Primary text</p>
<p class="bg-warning text-dark p-2">Warning background</p>
```

### **7.2 Display & Visibility**

```html
<p class="d-none d-md-block">Visible only on md and above</p>
```

### **7.3 Flex & Grid Utilities**

```html
<div class="d-flex justify-content-between">
  <div>Flex item 1</div>
  <div>Flex item 2</div>
</div>
```

---

## **8. Customization**

### **8.1 Using SCSS**

Bootstrap can be customized via SCSS variables. Example:

```scss
$primary: #ff5733;
@import "node_modules/bootstrap/scss/bootstrap";
```

### **8.2 Theming**

* Override colors, spacing, fonts.
* Create dark/light themes easily using variables.

---

## **9. Best Practices**

1. **Mobile-first**: Use responsive classes and test on multiple devices.
2. **Semantic HTML**: Keep markup accessible.
3. **Avoid inline styles**: Use utility classes for maintainability.
4. **Bundle JS properly**: Only include what you need in production.
5. **Use container classes properly**: `container` for fixed width, `container-fluid` for full width.

---

## **10. Next Steps / Advanced**

* **JavaScript Components**: Carousel, Tooltip, Popover, Offcanvas.
* **Responsive utilities**: Show/hide content per device.
* **Custom plugins**: Extend Bootstrap with your own JS modules.
* **Performance**: Purge unused CSS for production using tools like PurgeCSS.

---

This tutorial gives you a **solid foundation** and covers most of Bootstrap’s core features.

# Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Bootstrap Example Page</title>
  <!-- Bootstrap CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>

<!-- Navbar -->
<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
  <div class="container">
    <a class="navbar-brand" href="#">MySite</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNav">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item"><a class="nav-link active" href="#">Home</a></li>
        <li class="nav-item"><a class="nav-link" href="#">About</a></li>
        <li class="nav-item"><a class="nav-link" href="#">Contact</a></li>
      </ul>
    </div>
  </div>
</nav>

<!-- Hero Section -->
<section class="bg-light py-5 text-center">
  <div class="container">
    <h1 class="display-4">Welcome to My Bootstrap Page</h1>
    <p class="lead">A fully responsive page using Bootstrap 5 components</p>
    <button class="btn btn-primary btn-lg" data-bs-toggle="modal" data-bs-target="#exampleModal">Learn More</button>
  </div>
</section>

<!-- Cards Grid -->
<section class="py-5">
  <div class="container">
    <h2 class="mb-4 text-center">Our Features</h2>
    <div class="row g-4">
      <div class="col-md-4">
        <div class="card h-100">
          <img src="https://via.placeholder.com/350x150" class="card-img-top" alt="Feature 1">
          <div class="card-body">
            <h5 class="card-title">Feature 1</h5>
            <p class="card-text">This is a short description of feature 1.</p>
          </div>
          <div class="card-footer">
            <a href="#" class="btn btn-outline-primary w-100">Read More</a>
          </div>
        </div>
      </div>
      <div class="col-md-4">
        <div class="card h-100">
          <img src="https://via.placeholder.com/350x150" class="card-img-top" alt="Feature 2">
          <div class="card-body">
            <h5 class="card-title">Feature 2</h5>
            <p class="card-text">This is a short description of feature 2.</p>
          </div>
          <div class="card-footer">
            <a href="#" class="btn btn-outline-primary w-100">Read More</a>
          </div>
        </div>
      </div>
      <div class="col-md-4">
        <div class="card h-100">
          <img src="https://via.placeholder.com/350x150" class="card-img-top" alt="Feature 3">
          <div class="card-body">
            <h5 class="card-title">Feature 3</h5>
            <p class="card-text">This is a short description of feature 3.</p>
          </div>
          <div class="card-footer">
            <a href="#" class="btn btn-outline-primary w-100">Read More</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- Contact Form -->
<section class="py-5 bg-light">
  <div class="container">
    <h2 class="mb-4 text-center">Contact Us</h2>
    <form class="row g-3">
      <div class="col-md-6">
        <label for="name" class="form-label">Name</label>
        <input type="text" class="form-control" id="name" placeholder="Your Name">
      </div>
      <div class="col-md-6">
        <label for="email" class="form-label">Email</label>
        <input type="email" class="form-control" id="email" placeholder="Your Email">
      </div>
      <div class="col-12">
        <label for="message" class="form-label">Message</label>
        <textarea class="form-control" id="message" rows="4" placeholder="Your Message"></textarea>
      </div>
      <div class="col-12 text-center">
        <button type="submit" class="btn btn-primary btn-lg">Send Message</button>
      </div>
    </form>
  </div>
</section>

<!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1">
  <div class="modal-dialog modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">About This Page</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
      </div>
      <div class="modal-body">
        This page demonstrates how to build a responsive website using Bootstrap 5. It includes a navbar, hero section, card grid, contact form, and modal.
      </div>
      <div class="modal-footer">
        <button class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
      </div>
    </div>
  </div>
</div>

<!-- Footer -->
<footer class="bg-dark text-white text-center py-3">
  &copy; 2025 MySite. All rights reserved.
</footer>

<!-- Bootstrap JS Bundle -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

### ✅ **Features included**

* **Responsive navbar** with collapsible menu
* **Hero section** with call-to-action button
* **Card grid** for features/services
* **Contact form** with responsive layout
* **Modal popup** for additional info
* **Footer**
