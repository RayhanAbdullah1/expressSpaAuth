# expressSpaAuth

A minimal CodeIgniter-inspired MVC framework built on top of Express.js.
Flexible and unopinionated by design — start small, structure it your way,
and scale it to enterprise level without rewriting anything.
Zero build step. Minimal dependencies. Runs anywhere Node.js runs.

> Built for developers who want Laravel's structure without PHP's overhead.
> Runs on minimal server resources, deploys anywhere Node.js runs.

---

## Why

A standard Laravel app needs PHP 8+, Composer, and at least 512MB RAM.
This runs on Node.js with a fraction of that. For small client projects —
a lightweight admin panel, a simple business tool, an internal dashboard —
this is a practical alternative with familiar patterns.

---

## Architecture

```
├── index.js                        # Entry point
├── env.js                          # Environment & DB config
├── app/
│   ├── routes/
│   │   └── web.js                  # All application routes (like Laravel's web.php)
│   ├── controllers/
│   │   ├── PageControllers.js      # Public page controllers
│   │   └── app/
│   │       ├── backendPageControllers.js
│   │       └── productControllers.js
│   ├── middlewares/
│   │   └── authValidation.js       # Session auth & route guards
│   ├── libraries/
│   │   ├── routeWrapper.js         # Express app bootstrap
│   │   ├── componentHelper.js      # Server-side component serving
│   │   ├── mysqlORM.js             # Knex.js MySQL connection
│   │   ├── mysqlORMWrapper.js      # ORM query helpers
│   │   ├── helper.js               # Utility functions
│   │   └── debugHelper.js          # Debug utilities
│   ├── state/
│   │   └── auth.js                 # Auth state
│   ├── enums/
│   │   └── statusEnum.js
│   ├── utils/
│   │   └── databaseSeeder.js
│   └── database/
│       └── node_auth.sql           # Database schema
└── public/
    ├── pages/
    │   ├── index.html
    │   ├── auth/
    │   │   └── register.html
    │   └── app/
    │       ├── dashboard.html
    │       └── component/
    │           ├── layout/
    │           │   └── nav.html
    │           └── product/
    │               ├── productList.html
    │               ├── productCreate.html
    │               └── productEdit.html
    └── js/
        └── app/
            └── componentHelper.js  # Client-side component loader
```

---

## How It Works

### Routing

Routes are defined in `app/routes/web.js` — clean and readable, just like Laravel's `web.php`:

```js
app.get('/dashboard', authCheckMiddleware, dashboardPageHandler);
app.get('/product/edit/:id', authCheckMiddleware, csrfProtection, productEditPageHandler);
app.post('/product/edit/:id', authCheckMiddleware, csrfProtection, productUpdateHandler);
```

### Middleware

Route-level middleware works exactly like Laravel — pass it as an argument before the handler:

```js
app.get('/dashboard', authCheckMiddleware, dashboardPageHandler);
//                    ^^^^^^^^^^^^^^^^^^^
//                    runs before handler, like Laravel's route middleware
```

### Controllers

Each feature has its own controller file. Controllers handle request logic and return responses:

```js
// app/controllers/app/productControllers.js
async function productPageHandler(req, res) {
    const products = await db('products').where('user_id', req.user.id);
    // render or respond
}
```

### Component System

Components are plain HTML files in `public/pages/app/component/`.
The client fetches them at runtime via `/component/:path`, injects HTML into a target
DOM element, extracts and executes any `<script>` tags in context, then calls a
matching `init{ComponentName}()` initializer — passing app state and params.
No virtual DOM. No bundler. No build step.

```js
// Load a component into a DOM element
await loadComponentAndInject(appData, 'product/productList.html', 'main-content');
```

### Auth System

Session-based authentication using cookies and a `sessions` table in MySQL:

- Login sets a secure `auth_token` cookie
- `authCheckMiddleware` validates token against DB on every protected route
- Checks expiry, revocation, and attaches user data to `req.user`
- CSRF protection via `csurf` on all state-changing routes
- `loggedInSession` redirects already-authenticated users away from auth pages

---

## Stack

| Layer | Technology |
|---|---|
| Server | Node.js + Express.js |
| Database | MySQL + Knex.js |
| Auth | Cookie sessions + bcryptjs |
| Security | csurf (CSRF protection) |
| Frontend | Vanilla JS + HTML (zero framework) |

---

## Getting Started

**1. Clone the repo**

```bash
git clone https://github.com/RayhanAbdullah1/expressSpaAuth
cd expressSpaAuth
```

**2. Install dependencies**

```bash
npm install
```

**3. Configure environment**

```bash
cp env.example.js env.js
```

Edit `env.js` with your MySQL credentials:

```js
const mysqlCredentials = {
    host    : 'localhost',
    port    : 3306,
    user    : 'root',
    password: 'your_password',
    database: 'node_auth'
}
```

**4. Import the database schema**

```bash
mysql -u root -p node_auth < app/database/node_auth.sql
```

**5. Start the server**

```bash
node index.js
```

App runs at `http://localhost:5050`

---

## Design Philosophy

- **Laravel patterns, Node.js speed** — familiar MVC structure without PHP overhead
- **Zero build step** — no Webpack, no Vite, no compilation. Edit and refresh.
- **Minimal dependencies** — only what is necessary, nothing more
- **Deployable anywhere** — runs on the smallest VPS with minimal RAM
- **Readable code** — structure that any Laravel developer can follow immediately

---

## Roadmap

- [ ] Error handling middleware (like Laravel's `Handler.php`)
- [ ] CLI scaffolding — `node artisan make:controller ProductController`
- [ ] Config file system to replace `env.js`
- [ ] Clean query builder interface that abstracts Knex completely
- [ ] Blade-inspired templating helpers

---

## Author

**Rayhan Abdullah** — Senior Laravel Engineer & SaaS Architect

[rayhan.dotodd.tech](https://rayhan.dotodd.tech) · [LinkedIn](https://linkedin.com/in/rayhanabdullah1)
