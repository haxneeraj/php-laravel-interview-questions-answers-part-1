Excellent ⚡
Here’s **Part 4** of your eBook — formatted cleanly in Markdown (`.md`) for seamless merging with previous sections.
Save it as `PHP-Laravel-Interview-Guide-Part4.md`.

---

# **Part 4 — Advanced Laravel Interview Questions (Q91–Q120)**

---

### **Q91. What are Laravel Contracts?**

**Answer:**
Contracts are **interfaces** that define the core services provided by Laravel.
They ensure loose coupling by defining clear expectations between components.

**Example:**
`Illuminate\Contracts\Mail\Mailer` defines methods the mail service must implement.

---

### **Q92. What are Service Providers and their role?**

**Answer:**
Service Providers are the **foundation of Laravel bootstrapping**.
They register bindings, routes, and events.

**Located in:** `app/Providers/`
**Registered in:** `config/app.php`

**Example:**

```php
public function register() {
    $this->app->bind(UserRepositoryInterface::class, UserRepository::class);
}
```

---

### **Q93. What is the difference between bind() and singleton() in Laravel’s Service Container?**

| Method        | Description                                     |
| ------------- | ----------------------------------------------- |
| `bind()`      | Creates a new instance every time it’s resolved |
| `singleton()` | Creates only one instance (cached for reuse)    |

---

### **Q94. What are Laravel Macros and how do they work?**

**Answer:**
Macros allow you to extend Laravel’s core classes dynamically.

**Example:**

```php
Str::macro('maskEmail', fn($email) => preg_replace('/(.{3}).*@/', '$1***@', $email));
echo Str::maskEmail('neeraj@example.com'); // neer***@
```

---

### **Q95. What is the difference between Events and Jobs in Laravel?**

| Feature | Events                           | Jobs                    |
| ------- | -------------------------------- | ----------------------- |
| Purpose | Triggered when something happens | Handle background tasks |
| Example | `UserRegistered` event           | `SendWelcomeEmail` job  |

**Usage Example:**

```php
event(new UserRegistered($user));
dispatch(new SendWelcomeEmail($user));
```

---

### **Q96. What is Laravel Broadcasting?**

**Answer:**
Broadcasting sends real-time data to the frontend via WebSockets.

**Drivers:** Pusher, Ably, Redis, Laravel WebSockets.

**Example use-case:** live chat, notifications, dashboards.

---

### **Q97. What are Laravel Notifications?**

**Answer:**
A unified system for sending messages via multiple channels — email, SMS, Slack, database.

**Example:**

```php
$user->notify(new InvoicePaidNotification($invoice));
```

---

### **Q98. What is Laravel Horizon?**

**Answer:**
Horizon is a dashboard for **monitoring Laravel queues** in real time — shows job status, retry counts, and processing time.
Used with Redis queue driver.

---

### **Q99. What are API Resources in Laravel?**

**Answer:**
Resources transform Eloquent models into JSON responses.

**Example:**

```php
return new UserResource($user);
```

**Resource file example:**

```php
public function toArray($request) {
  return [
    'id' => $this->id,
    'name' => $this->name,
  ];
}
```

---

### **Q100. What are Laravel Policies and Gates?**

**Answer:**
They handle **authorization** logic.

| Term       | Description                          |
| ---------- | ------------------------------------ |
| **Gate**   | Closure-based authorization          |
| **Policy** | Class-based authorization for models |

**Example:**

```php
Gate::define('update-post', fn($user, $post) => $user->id === $post->user_id);
```

---

### **Q101. How does Authentication work in Laravel?**

**Answer:**
Laravel uses `auth` middleware and `guards` to handle authentication.
Default guard: `web` (session-based).
For APIs: use `sanctum` or `passport`.

---

### **Q102. What is Laravel Sanctum?**

**Answer:**
Sanctum provides lightweight API authentication using tokens — ideal for SPAs and mobile apps.

**Example:**

```php
$token = $user->createToken('api')->plainTextToken;
```

---

### **Q103. What is Laravel Passport?**

**Answer:**
Passport provides **OAuth2-based** authentication for complex API systems.

---

### **Q104. What is Laravel Socialite?**

**Answer:**
Socialite provides OAuth authentication for third-party services like Google, Facebook, and GitHub.

**Example:**

```php
return Socialite::driver('github')->redirect();
```

---

### **Q105. What are Laravel Middlewares used for in APIs?**

**Answer:**
They filter API requests for rate-limiting, CORS, authentication, etc.

**Example:**
`api.php` routes often use:

```php
Route::middleware('auth:sanctum')->get('/user', fn(Request $r) => $r->user());
```

---

### **Q106. What is Laravel Route Caching?**

**Answer:**
Optimizes performance by caching routes.

**Commands:**

```bash
php artisan route:cache
php artisan route:clear
```

---

### **Q107. What is Laravel Config Caching?**

**Answer:**
Combines all config files into one cached file for faster loading.

```bash
php artisan config:cache
```

---

### **Q108. What is Query Caching in Laravel?**

**Answer:**
You can cache query results using:

```php
$users = Cache::remember('users', 60, fn() => User::all());
```

---

### **Q109. What is Laravel’s Event Broadcasting used for?**

**Answer:**
For real-time applications (like chat or notifications) using WebSocket connections.

**Example:**
Broadcast event:

```php
class MessageSent implements ShouldBroadcast {}
```

---

### **Q110. What are Laravel Job Queues?**

**Answer:**
Background processing system that delays or distributes tasks like emails, notifications, etc.

**Queue Drivers:** Database, Redis, Amazon SQS.

**Example:**

```php
dispatch(new ProcessOrderJob($order));
```

---

### **Q111. What are Laravel Observers?**

**Answer:**
Observers listen for model lifecycle events and perform actions automatically.

**Example:**
In `UserObserver`:

```php
public function created(User $user) {
    Mail::to($user)->send(new WelcomeMail());
}
```

---

### **Q112. What is Laravel Telescope?**

**Answer:**
Telescope is a debugging assistant for Laravel — tracks requests, queries, exceptions, and queue jobs.

---

### **Q113. What are Laravel Pipelines?**

**Answer:**
Pipelines pass data through a series of steps (pipes).
Useful for request modification and middleware-like processes.

**Example:**

```php
Pipeline::send($user)
    ->through([CheckAge::class, VerifyEmail::class])
    ->thenReturn();
```

---

### **Q114. What are Laravel Events and Listeners used for?**

**Answer:**
They decouple logic.
Events define “what happened”; listeners define “what to do.”

**Example:**
UserRegistered → SendWelcomeMail

---

### **Q115. What are Laravel Commands and how do you create one?**

**Answer:**
Custom Artisan commands automate repetitive tasks.

**Example:**

```bash
php artisan make:command SendReportCommand
```

---

### **Q116. What is Laravel Localization?**

**Answer:**
Used to support multiple languages.

**Example:**
Create `resources/lang/en/messages.php` and `resources/lang/hi/messages.php`.
Use:

```php
__('messages.welcome');
```

---

### **Q117. What are Laravel Packages?**

**Answer:**
Reusable modules or features distributed via Composer.
You can create custom packages in `packages/vendor-name/package-name`.

---

### **Q118. What is Laravel’s Event Loop in Octane?**

**Answer:**
Octane keeps the application in memory to serve multiple requests quickly — reduces boot time per request.
Supports **Swoole** and **RoadRunner**.

---

### **Q119. What are Laravel API Rate Limiters?**

**Answer:**
Control how many requests a user can make.

**Example:**

```php
Route::middleware(['throttle:60,1'])->group(function() {
  Route::get('/posts', [PostController::class, 'index']);
});
```

---

### **Q120. What is Laravel Cashier?**

**Answer:**
A package that manages **subscriptions and billing** with Stripe or Paddle.

---

✅ **End of Part 4 (Advanced Laravel)**

Next → **Part 5: Performance, Security & Testing (Q121–Q150)**
This section will include caching strategies, testing techniques, optimization commands, Laravel security practices, and interview pro tips.

Would you like me to continue with **Part 5** now?
