# **Part 5 — Performance, Security & Testing (Q121–Q150)**

### *Written by Neeraj Saini — Senior Software Engineer*

🔗 **Links**  
🧑‍💻 GitHub → [github.com/haxneeraj](https://github.com/haxneeraj)  
🌍 Portfolio → [www.haxneeraj.com](https://www.haxneeraj.com)  
💼 LinkedIn → [linkedin.com/in/hax-neeraj](https://www.linkedin.com/in/hax-neeraj/)


---

### **Q121. What are common ways to improve Laravel application performance?**

**Answer:**

* Use **caching** (routes, config, queries).
* Enable **OPcache** in PHP.
* Use **queues** for heavy tasks (emails, exports).
* Optimize **database queries** with eager loading.
* Minify assets (CSS/JS) with Laravel Mix or Vite.
* Use **Octane** for long-running app instances.
* Use **Redis** or **Memcached** for cache/session.

---

### **Q122. How does route caching improve performance?**

**Answer:**
`php artisan route:cache` compiles all routes into a single PHP file, reducing route registration time.

Use it only when your routes are **stable** (no closures).

---

### **Q123. What is Config Caching and when to use it?**

**Answer:**
Combines all config files into one cached file for faster loading.

```bash
php artisan config:cache
```

Use after deployment for maximum performance.

---

### **Q124. How to optimize database performance in Laravel?**

**Tips:**

* Use **Eager Loading** (`with()`) instead of lazy loading.
* Use **chunking** for large data exports.
* Optimize indexes in your database.
* Use **select()** to limit columns.
* Avoid **N+1 query problem**.

**Example:**

```php
$users = User::with('posts')->get();
```

---

### **Q125. What is the N+1 Query Problem in Laravel?**

**Answer:**
Occurs when each model triggers an additional query for its relation.
Use `with()` to fix it.

**Example (Bad):**

```php
foreach (User::all() as $user) {
  echo $user->posts->count();
}
```

**Good:**

```php
User::with('posts')->get();
```

---

### **Q126. What are Laravel Queues and how do they improve performance?**

**Answer:**
Queues move time-consuming tasks to the background (email, notifications, reports).

**Example:**

```php
dispatch(new SendEmailJob($user));
```

This keeps the request-response cycle fast and responsive.

---

### **Q127. How to monitor jobs and queues in Laravel?**

**Answer:**
Use **Laravel Horizon** for Redis queue monitoring.
It displays real-time metrics like job counts, failures, and processing times.

---

### **Q128. How does caching improve application speed?**

**Answer:**
Caching reduces redundant computations or database queries.

**Example:**

```php
$users = Cache::remember('users', 60, fn() => User::all());
```

---

### **Q129. What are different types of caching in Laravel?**

* **Route cache**
* **Config cache**
* **Query cache**
* **View cache**
* **Application cache**

---

### **Q130. What is Laravel Octane and when should you use it?**

**Answer:**
Laravel Octane serves requests using a **persistent application instance**, drastically reducing boot time.
It supports high-performance servers like **Swoole** and **RoadRunner**.

Use Octane for APIs, dashboards, or high-traffic SaaS apps.

---

### **Q131. How to optimize Laravel for production?**

**Checklist:**

* Use `php artisan optimize`
* Cache config and routes
* Disable debug mode (`APP_DEBUG=false`)
* Use `APP_ENV=production`
* Use Redis and queues
* Minify and version assets

---

### **Q132. What are Laravel’s built-in Security features?**

**Answer:**

* CSRF protection
* XSS protection via `{{ }}` escaping
* Password hashing (bcrypt, argon2)
* SQL injection prevention via parameter binding
* Encryption via `Crypt` facade
* Rate limiting

---

### **Q133. How does Laravel prevent SQL Injection?**

**Answer:**
All Eloquent queries use **prepared statements**, which auto-escape parameters.

**Example:**

```php
User::where('email', $email)->first();
```

---

### **Q134. How does Laravel handle Cross-Site Scripting (XSS)?**

**Answer:**
Blade automatically escapes all output using `{{ $variable }}`.
If HTML is safe, use `{!! $variable !!}`.

---

### **Q135. How to prevent Cross-Site Request Forgery (CSRF)?**

**Answer:**
Laravel auto-generates CSRF tokens in forms.

**Example:**

```blade
<form method="POST">@csrf</form>
```

---

### **Q136. How to encrypt and decrypt data in Laravel?**

**Answer:**
Use the `Crypt` facade.

**Example:**

```php
$encrypted = Crypt::encrypt('secret');
$decrypted = Crypt::decrypt($encrypted);
```

---

### **Q137. How to hash passwords securely in Laravel?**

**Answer:**
Laravel uses the `Hash` facade.

**Example:**

```php
Hash::make('password');
Hash::check('password', $hashed);
```

---

### **Q138. What is Laravel Rate Limiting?**

**Answer:**
Prevents abuse by limiting requests per IP or user.

**Example:**

```php
Route::middleware('throttle:60,1')->group(function () {
  Route::get('/api/posts', [PostController::class, 'index']);
});
```

---

### **Q139. How does Laravel’s CSRF Middleware work internally?**

**Answer:**
It checks the `_token` field in every POST, PUT, DELETE request.
If the token doesn’t match the session, Laravel throws a **TokenMismatchException**.

---

### **Q140. How does Laravel validate user input?**

**Answer:**

```php
$request->validate([
  'name' => 'required|string|max:255',
  'email' => 'required|email|unique:users'
]);
```

Validation errors are automatically returned as JSON for API routes.

---

### **Q141. What are Laravel’s Testing tools?**

**Answer:**
Laravel integrates with **PHPUnit** and provides:

* **Feature tests** (HTTP requests, routes)
* **Unit tests** (business logic)
* **Database testing helpers**

**Example:**

```php
public function test_homepage_loads_successfully() {
  $this->get('/')->assertStatus(200);
}
```

---

### **Q142. What is the difference between Unit and Feature tests?**

| Test Type   | Purpose                                        |
| ----------- | ---------------------------------------------- |
| **Unit**    | Tests individual methods or classes            |
| **Feature** | Tests a complete flow (controller → view → DB) |

---

### **Q143. What is the Laravel `RefreshDatabase` trait?**

**Answer:**
Automatically rolls back and migrates the database between tests.

**Example:**

```php
use RefreshDatabase;
```

---

### **Q144. What are Mocking and Fakes in Laravel testing?**

**Answer:**
Mocking replaces dependencies during testing.
**Fakes** like `Mail::fake()` or `Bus::fake()` simulate real actions without executing them.

**Example:**

```php
Mail::fake();
Mail::assertNothingSent();
```

---

### **Q145. How to test validation errors in Laravel?**

**Answer:**

```php
$response = $this->post('/register', ['email' => '']);
$response->assertSessionHasErrors(['email']);
```

---

### **Q146. What are Laravel Dusk and Pest?**

**Answer:**

* **Dusk:** For browser automation & UI testing.
* **Pest:** Elegant, human-friendly alternative to PHPUnit for Laravel.

---

### **Q147. What is Laravel Telescope used for during testing?**

**Answer:**
Telescope helps inspect requests, queries, logs, and exceptions during development — making debugging easier.

---

### **Q148. How to log errors and exceptions in Laravel?**

**Answer:**
All logs are stored in `/storage/logs/laravel.log`.
You can log custom messages using:

```php
Log::info('Job processed successfully');
Log::error('Failed to send email');
```

---

### **Q149. How to send errors to third-party services?**

**Answer:**
Integrate tools like **Sentry**, **Bugsnag**, or **Rollbar** using service providers and environment configs.

---

### **Q150. What are some common Laravel interview tips for developers?**

**Answer:**
✅ Master **Eloquent relationships**
✅ Understand **Service Container & Providers**
✅ Know **Auth, Middleware, Events, Queues**
✅ Practice **artisan commands & migrations**
✅ Be clear on **SOLID principles**
✅ Write clean, testable code using **repositories & services**
✅ Always explain **why** you use something — not just *what* it does

---

✅ **End of Part 5 (Performance, Security & Testing)**
