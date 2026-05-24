# Flask Blueprints

*A hands-on guide to modular Flask apps*

---

## Table of Contents

1. [What are blueprints?](#what-are-blueprints)
2. [Project structure](#project-structure)
3. [Creating a blueprint](#creating-a-blueprint)
4. [Advanced patterns](#advanced-patterns)

---

## 1. What are blueprints?

A **Blueprint** is Flask's way of organizing a group of related views, templates, and static files into a reusable component — think of it as a mini-application you plug into the main app.

> **The problem blueprints solve:** as your Flask app grows, putting everything in one `app.py` becomes unmanageable. Blueprints let you split features (auth, blog, API) into separate modules, each with their own routes, templates, and logic.

Key benefits:

- **Modularity** — group related routes and logic together
- **Reusability** — register the same blueprint across multiple apps
- **URL prefixing** — namespace routes cleanly per module
- **Own templates** — each blueprint can have its own template folder

### Blueprint lifecycle

1. Create a `Blueprint` object inside a module
2. Define routes (and other hooks) on that object
3. Import and `register_blueprint()` it in your factory or main app
4. Flask merges it into the app — routes, error handlers, everything

---

## 2. Project structure

A well-structured Flask project puts each feature in its own package. Here's a typical layout for an app with `auth`, `blog`, and `api` modules:

```
myapp/
├── auth/               # auth blueprint package
│   ├── __init__.py     # Blueprint object lives here
│   ├── routes.py
│   ├── forms.py
│   └── templates/
│       └── auth/
│           ├── login.html
│           └── register.html
├── blog/               # blog blueprint package
│   ├── __init__.py
│   ├── routes.py
│   └── templates/
│       └── blog/
├── api/                # api blueprint package
│   ├── __init__.py
│   └── routes.py
├── __init__.py         # app factory: create_app()
└── config.py
run.py                  # entry point
```

> **Why nest templates?** Keeping a `templates/auth/` subdirectory inside the blueprint means template names don't clash — you reference them as `auth/login.html` everywhere, even across blueprints.

> **Tip:** The `__init__.py` at the top level should contain a `create_app()` factory function — not a global `app` object. This makes testing and configuration much easier.

---

## 3. Creating a blueprint

Let's build the `auth` blueprint step by step.

### Step 1 — define the blueprint

```python
# myapp/auth/__init__.py

from flask import Blueprint

# name, import_name, optional url_prefix
auth = Blueprint(
    'auth',
    __name__,
    url_prefix='/auth',
    template_folder='templates',
)

from . import routes  # import routes so they register on 'auth'
```

### Step 2 — add routes

```python
# myapp/auth/routes.py

from flask import render_template, redirect, url_for
from . import auth  # the Blueprint object

@auth.route('/login', methods=['GET', 'POST'])
def login():
    # url is /auth/login  (prefix + route)
    return render_template('auth/login.html')

@auth.route('/logout')
def logout():
    return redirect(url_for('auth.login'))  # note: 'blueprint.endpoint'

@auth.route('/register', methods=['GET', 'POST'])
def register():
    return render_template('auth/register.html')
```

### Step 3 — register in the app factory

```python
# myapp/__init__.py

from flask import Flask

def create_app(config=None):
    app = Flask(__name__)
    app.config['SECRET_KEY'] = 'change-me'

    from .auth import auth as auth_bp
    from .blog import blog as blog_bp
    from .api  import api  as api_bp

    app.register_blueprint(auth_bp)                        # /auth/*
    app.register_blueprint(blog_bp, url_prefix='/blog')
    app.register_blueprint(api_bp,  url_prefix='/api/v1')

    return app
```

> **`url_for()` with blueprints:** always prefix the endpoint with the blueprint name — `url_for('auth.login')` not `url_for('login')`. For links inside a blueprint's own templates, you can use the shorthand `url_for('.login')`.

---

## 4. Advanced patterns

Once you're comfortable with basic blueprints, these patterns take them further.

### Blueprint-specific `before_request`

Run a function before every request to this blueprint — great for auth guards.

```python
@auth.before_request
def require_login():
    if not current_user.is_authenticated:
        return redirect(url_for('auth.login'))
```

### Blueprint error handlers

Handle errors per-blueprint so each module can respond differently.

```python
@api.errorhandler(404)
def api_not_found(e):
    return {'error': 'not found'}, 404

@api.errorhandler(403)
def api_forbidden(e):
    return {'error': 'forbidden'}, 403
```

### Blueprint static files

Each blueprint can serve its own static assets.

```python
auth = Blueprint(
    'auth', __name__,
    static_folder='static',
    static_url_path='/auth/static',
)

# In templates: url_for('auth.static', filename='logo.png')
```

### Registering a blueprint twice

You can register the same blueprint at different prefixes — useful for versioned APIs.

```python
app.register_blueprint(api_bp, url_prefix='/api/v1', name='api_v1')
app.register_blueprint(api_bp, url_prefix='/api/v2', name='api_v2')

# url_for('api_v2.users') → /api/v2/users
```

---

## Key takeaways

- Always use `url_for('blueprint_name.endpoint')` — the blueprint name prefix is required outside of templates belonging to that blueprint.
- Prefer the **app factory pattern** (`create_app()`) over a global `app` object — it avoids circular import issues.
- Keep each blueprint's templates in a subdirectory named after the blueprint (e.g. `templates/auth/`) so names never collide across modules.
