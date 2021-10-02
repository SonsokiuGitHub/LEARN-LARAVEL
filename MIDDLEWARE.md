### Middleware

#### 1 Khái Niệm Middleware

Trong Laravel, middleware cung cấp một cơ chế thuận tiện để kiểm tra và lọc các HTTP request đến ứng dụng của bạn.

Trong Laravel, tất cả các middleware sẽ được đặt trong thư mục `app/Http/Middleware`.

#### 2 Khai Báo Middleware

Để tạo middleware trong Laravel các bạn sử dụng command:

```bash
php artisan make:middleware MiddlewareName
```

**Trong đó**: `MiddlewareName` là tên của middleware các bạn muốn tạo.

#### 3 Đăng Kí Middleware

Để đăng ký middlware vào trong app các bạn khai báo chúng ở trong file `app/Http/Kernel.php`. Mặc định, Laravel có 3 loại middleware chính là:

- Global middleware: Các middlware được khai báo trong thuộc tính `$middleware` là global middleware, các middleware này sẽ được thực thi cho tất cả request.
- Group middleware: Các middlware được khai báo trong thuộc tính `$middlewareGroups` sẽ được thực thi khi chúng ta gọi chúng. Mặc định thì Laravel định nghĩa ra 2 group là web và api tương ứng với các route trong web.php và api.php.
- Route middleware: Các middlware được khai báo trong thuộc tính `$routeMiddleware` sẽ được thực thi khi chúng ta gọi tên chúng.

**VD**: Mình sẽ khai báo middleware `EnsureTokenIsValid` vào trong `$routeMiddleware` với name là '`validate_token`'.

```php
/**
 * The application's route middleware.
 *
 * These middleware may be assigned to groups or used individually.
 *
 * @var array
 */
protected $routeMiddleware = [
    //...
    //...
    'validate_token' => \App\Http\Middleware\EnsureTokenIsValid::class
];
```

Lúc này nếu route nào cần dùng chỉ cần khai báo middleware là '`validate_token`' là được.

**VD**:

```php
Route::get('/user', function () {
    //
})->middleware('validate_token');
```

#### 4 Mức Độ Ưu Tiên Middleware

Thông thường middleware sẽ được thực thi theo thứ tự từ trên xuống. Nhưng nếu bạn muốn thay đổi thứ tự ưu tiên của một middleware nào đó. Bạn có thể thêm vào trong thuộc tính `$middlewarePriority` trong file `app/Http/Kernel.php`.

**VD**:

```php
protected $middlewarePriority = [
    \Illuminate\Cookie\Middleware\EncryptCookies::class,
    \Illuminate\Session\Middleware\StartSession::class,
    \Illuminate\View\Middleware\ShareErrorsFromSession::class,
    \Illuminate\Contracts\Auth\Middleware\AuthenticatesRequests::class,
    \Illuminate\Routing\Middleware\ThrottleRequests::class,
    \Illuminate\Session\Middleware\AuthenticateSession::class,
    \Illuminate\Routing\Middleware\SubstituteBindings::class,
    \Illuminate\Auth\Middleware\Authorize::class,
];
```

**Chú ý**: `$middlewarePriority` chỉ work đối với middleware không phải global.

#### 5 Middleware Parameter

Trong một số trường hợp bạn muốn truyền thêm tham số vào middleware để thực thi các logic cho từng tham số đó. Bạn có thể tham khảo ví dụ sau.

**VD**: thêm tham số `$redirectTo` vào middleware `EnsureTokenIsValid` ở trên.

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class EnsureTokenIsValid
{
    /**
     * Handle an incoming request.
     *
     * @param \Illuminate\Http\Request $request
     * @param Closure $next
     * @param string $redirectTo
     * @return mixed
     */
    public function handle(Request $request, Closure $next, $redirectTo = 'home')
    {
        if ($request->input('token') !== 'toidicode.com') {
            return redirect($redirectTo);
        }

        return $next($request);
    }
}
```

Lúc này khi gọi middleware các bạn có thể truyền thêm tham số vào middleware các bạn sử dụng như sau:

```php
Route::get('/', function () {
    return redirect()->back();
})->middleware('validate_token:/login');
```

Trong đó: đằng sau dấu `:` sẽ là tham số truyền vào middleware, nếu middleware có nhiều tham số truyền vào bạn có thể sử dụng dấu `,` để ngăn cách các biến.

