**# Mục Lục-Part2**

## 1. Controller

## 2. Views

## 3. Blade Template

## 4. ISSUES



---

### 1. Controller

 #### 1.1 Introduction

 `Controller` là một lớp điều khiển trung tâm, xử lý tất cả các logic, **_tiếp nhận request_**

và xử lý dữ liệu ( được load từ bên `Models` ). Sau đó Controller trả response về `view`, một số trường hợp có thể trả về `JSON`, `XML` hoặc `Text`. 

#### 1.2 Create Comtroller

Các Controller được lưu trữ trong thư mục `app/Http/Controllers`.

Đặt tên file controller nên đặt tên theo nguyên tắc `PascalCase`.

Để tạo một Controller ta sử dụng câu lệnh

```php
php artisan make:controller NameController
```

Với *_NameController_* là tên file Controller.

Giả sử cần tạo file `UserController.php` nằm trong thư mục User. thư mục User nằm trong thư mục Controllers thì cú pháp sẽ như sau:

```php
php artisan make:controller User/UserController
```



Trong file UserController được khởi tạo từ câu lệnh trên sẽ được khai báo đầy đủ `namespace`, `use đường dẫn của lớp Request` và `tạo một class` có tên UserController trùng với tên file Controller chứa nó, class này kế thừa ( extend ) class Controller ( mặc định của Laravel)



#### 1.3 Định nghĩa Route đến phương thức trong Controller

```php
use App\Http\Controllers\UserController;

Route::get('/user/{id}', [UserController::class, 'show'])
```



#### 1.4 Single Action Controller

Trong một vài trường hợp nếu chỉ muốn một Controller Class thực thi một hành động duy nhất, thì chúng ta cũng có thể thêm một phương thức như phần trên rồi assign chúng vào Route. Hoặc có thể viết chúng trong phương thức `__invoke` rồi trong Route  sẽ không cần phải truyền thêm phương thức. 

```php
<?php

namespace App\Http\Controllers;

use App\Http\Controllers\Controller;
use App\Models\User;

class ProvisionServer extends Controller
{
    public function __invoke()
    {
        // ...
    }
}
```

Khi định nghĩa một Route đến phương thức `__invoke()` , ta không cần chỉ định phương thức, thay vào đó chỉ cần gọi tên Controller. Ví dụ :

```php
use App\Http\Controllers\ProvisionServer;

Route::post('/server', ProvisionServer::class)
```

> Chúng ta có thể tạo một `invokable controller` bằng lệnh  Artisan Command:
>
> ```php
> php artisan make:controller ProvisionServer --invokable
> ```



#### 1.5 Controller middleware



#### 1.6 Resource Controller 

Resource Controller trong Laravel là một dạng controller cung cấp sẵn các action `index`, `create`, `store`, `show`, `edit`, `update`, `destroy` để thực thi các hành động CRUD data

Do trường hợp sử dụng phổ biến này, Laravel đã tạo ra một Controller chứa đủ các phương thức create, read, update, và delete điển hình. Để tạo Controller chứa đủ các phương thức này ta sủ dụng lệnh Artisan Command:

```php
php artisan make:controller NameController --resource
```



Đăng kí một Resource Route trỏ đến Controller 

```php
use App\Http\Controllers\PhotoController;

Route::resource('photos', PhotoController::class);

```



Đăng kí nhiều Resource Route 

```php
Route::resources([
    'photos' => PhotoController::class,
    'posts' => PostController::class,
]);
```



#### 1.7 Partial Resource Route

Khi khai báo một Resource Route, chúng ta có thể chỉ định một tập hợp các `action mà Controller sẽ xử lý` thay vì tất cả như mặc định.

```php
use App\Http\Controllers\PhotoController;

Route::resource('photos', PhotoController::class)->only([
    'index', 'show'
]);

Route::resource('photos', PhotoController::class)->except([
    'create', 'store', 'update', 'destroy'
]);
```



#### 1.7.1 [API Resource Routes](https://laravel.com/docs/8.x/controllers#api-resource-routes) 

Trong trường hợp ứng dụng của bạn chỉ cần cung cấp các action như một API Resfull. Thì bạn có thể sử dụng API Resource Route. Lúc này các Route sẽ bỏ qua các action `create`, `edit`.



#### 1.8 Nested Resources

##### 1.8.1 [Scoping Nested Resources](https://laravel.com/docs/8.x/controllers#scoping-nested-resources) 

##### 1.8.2 [Shallow Nesting](https://laravel.com/docs/8.x/controllers#shallow-nesting) 

##### 1.8.3 [Shallow Nesting](https://laravel.com/docs/8.x/controllers#shallow-nesting) 



#### 1.9 [Naming Resource Routes](https://laravel.com/docs/8.x/controllers#restful-naming-resource-routes) 

Mặc định, tất cả resource controller actions có một route name; tuy nhiên, chúng ta có thể override các tên này bằng cách chuyển một mảng tên với route names mong muốn.

```php
use App\Http\Controllers\PhotoController;

Route::resource('photos', PhotoController::class)->names([
    'create' => 'photos.build'
]);
```



#### 1.10 [Naming Resource Route Parameters](https://laravel.com/docs/8.x/controllers#restful-naming-resource-route-parameters) 

#### 1.11 [Scoping Resource Routes](https://laravel.com/docs/8.x/controllers#restful-scoping-resource-routes) 

#### 1.12 [Localizing Resource URIs](https://laravel.com/docs/8.x/controllers#restful-localizing-resource-uris) 

#### 1.13 [Supplementing Resource Controllers](https://laravel.com/docs/8.x/controllers#restful-supplementing-resource-controllers) 

```php
use App\Http\Controller\PhotoController;

Route::get('/photos/popular', [PhotoController::class, 'popular']);
Route::resource('photos', PhotoController::class);
```



#### 1.14 [Dependency Injection & Controllers](https://laravel.com/docs/8.x/controllers#dependency-injection-and-controllers) 



---



## 2. Views

### 2.1 Introduction

### 2.2 Creating and Rendering View

Chúng ta có thể tạo một `view` bằng cách đặt file có phần mở rộng `.blade.php` trong thư mục `resources/views` . Phần mở rộng `blade.php` thông báo cho Laravel rằng tồn tại định dạng `blade template` trong `view` được tạo. 

Trong laravel, mặc định các `view` sẽ được đặt trong thư mục `resources/views`. Nếu như  muốn thay đổi thư mục ta có thể vào `config/view.php` thay đổi lại giá trị của `paths`.

Nếu trong thư mục có cùng các view trùng tên thì laravel sẽ ưu tiên theo thứ tự như sau: `.blade.php` , `.php`, `.css`, `.html`.

Khi đã tạo được `view` chúng ta có thể gọi nó từ application's routes hoặc controllers sử dụng  global `view` helper:

```php
Route::get('/', function () {
    return view('user', ['name' => 'sonnguyen']);
});
```



_Hoặc có thể sử dụng_ `View::make` 

```php
use Illuminate\Support\Facades\View;

return View::make('user', ['name' => 'sonnguyen']);
```



#### 2.2.1 [Nested View Directories](https://laravel.com/docs/8.x/views#nested-view-directories) 

Giả sử file profile.blade.php có cấu trúc thư mục như sau `resources/views/admin/profile.blade.php`. Chúng ta có thể return nó từ application's routes hoặc Controllers như sau:

```php
return view('admin.profile', $data);
```

#### 2.2.2 [Creating The First Available View](https://laravel.com/docs/8.x/views#creating-the-first-available-view) 

Trong một số trường hợp nếu muốn render ra view tồn tại đầu tiên trong danh sách view thì ta có thể sử dụng phương thức first trong `View` object với cú pháp như sau:

```php
use Illuminate\Support\Facades\View;

return View::first(['custom.admin', 'admin'], $data);
```



#### 2.2.3 [Determining If A View Exists](https://laravel.com/docs/8.x/views#determining-if-a-view-exists) 

Để check xem một view có tồn tại hay không các bạn sử dụng phương thức `exists` với cú pháp như sau:

```php
use Illuminate\Support\Facades\View;

View::exists($view);
```

`View::exists()` return `true` nếu tồn tại `view`



### 2.3 [Passing Data To Views](https://laravel.com/docs/8.x/views#passing-data-to-views) 

**Truyền vào một mảng với key và values**

```php
return view('user', ['name' => 'nguyenvanson']);
```



**Sử dụng hàm `compact`**

```php
$name = 'nguyenvanson';
$job = 'Dev';
return view('user', compact('name', 'job'));
```



**Sử dụng  hàm `with`**

```php
return view('user')
            ->with('name', 'nguyenvanson')
            ->with('job', 'dev');
```



```php
return view('user')->with(['name'=>$name, 'job'=>$job]);
```



#### 2.3.x [Sharing Data With All Views](https://laravel.com/docs/8.x/views#sharing-data-with-all-views) 

### 2.4 [View Composers](https://laravel.com/docs/8.x/views#view-composers) 

### 2.5 [Optimizing Views](https://laravel.com/docs/8.x/views#optimizing-views) 

---



## 3. Blade Template

#### 3.1 Introduction

#### 3.2 Displaying Data

Để hiển thị dữ liệu trong Blade Template ta sử dụng cú pháp sau : 

```php
{{code}}
```

Trong cặp dấu` {{}} `sẽ là code php.

Ví dụ :

```php
Route::get('/', function () {
    return view('welcome', ['name' => 'sonnguyen']);
});
```



Sử dụng nội dung trong file view welcome 

```php
Hello, {{ $name }}.
```

Kết quả hiển thị ra màn hình : `Hello, sonnguyen`

Cặp dấu `{{}}` tượng trưng cho câu lệnh echo trong PHP kết hợp với hàm htmlspecialchars để phòng chống lỗi XSS attack. Tức là ta sẽ không thể in ra mã HTML sử dụng cặp dấu `{{}}` này được.

Mặc định Laravel sẽ bật chế độ double encoding trong hàm `htmlspecialchars` nếu bạn muốn tắt nó bạn có thể sử dụng `Blade::withoutDoubleEncoding` để tắt chúng đi. Và bạn nên đặt nó ở trong Provider để cho nó hoạt động tốt.

**ví dụ tắt double encoding trong Blade đi.**

```php
<?php

namespace App\Providers;

use Illuminate\Support\Facades\Blade;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        Blade::withoutDoubleEncoding();
    }
}
```



Trong trường hợp ta muốn hiển thị ra dữ liệu nguyên bản của dữ liệu bạn có thể sử dụng cặp dấu `{!!!!}`.

```php
/* routes/web.php */
use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view('welcome', ['name' => "<span style='color: red'>sonnguyen singer</span>"]);
})->name('home');
```

```php
Hello, {!! $name !!}
```

```php
Hello, sonnguyen singer    // sonnguyen singer có màu đỏ :v
```

**Chú ý**: Vì các cặp `{{}}` , `{!!!!}` chỉ tương ứng với câu lệnh `echo` trong PHP, nên nó chỉ nhận string truyền vào thôi.

Để hiển thị dữ liệu dạng json endcode trong Blade các bạn có thể sử dụng `@json` với cú pháp sau:

```php
@json($array, $flag)
```

**Trong đó**:
\- `$array` là mảng dữ liệu bạn muốn encode.
\- `$flag` là kiểu encode bạn muốn, trường này bạn có thể bỏ trống.

##### 3.2.x [Blade & JavaScript Frameworks](https://laravel.com/docs/8.x/blade#blade-and-javascript-frameworks) 

Vì hiện này khá nhiều các framework JS cũng sử dụng cáp cặp `{{}}` để in ra dữ liệu. Nên nếu bạn không muốn Blade compile đoạn code nào đó thì bạn chỉ cần thêm ký tự `@` vào trước là được.

```php
Hello, @{{ $name }}
```

Kết quả in ra màn hình : `Hello, @{{ $name }}`



Trong trường hợp bạn không muốn Blade biên dịch nhiều dòng code thì bạn có thể sử dụng `@verbatim` directive.

```php
@verbatim
    <div class="container">
        Hello, {{ name }}.
    </div>
@endverbatim

```

#### 3.3 Blade Directives

##### 3.3.1 If Statements

- @unless

- @isset
- @empty
- @auth
- @guest
- @production
- @env
- @hasSection
- @sectionMissing

##### 3.3.2 Switch Statements

##### 3.3.3 Loops

##### 3.3.4 The Loop Variable

##### 3.3.5 Conditional Classes

```php
@php
    $isActive = false;
    $hasError = true;
@endphp

<span @class([
    'p-4',
    'font-bold' => $isActive,
    'text-gray-500' => ! $isActive,
    'bg-red' => $hasError,
])></span>

<span class="p-4 text-gray-500 bg-red"></span>
```



##### 3.3.6 Including Subviews

**@include**

Blade template cho phép bạn có thể include các view khác vào trong view hiện tại một cách đơn giản bằng việc sử dụng `@include` directive. Lúc này ở các view được nhúng cũng có thể sử dụng được tất cả các biến có trong view hiện tại.

**VD**: Giả sử ta có 2 view với path và code như sau: 

\- `resources/views/shared/notify.blade.php`

```php
<div class="alert">{{ $alertMessage }}</div>
```

\- `resources/views/home.blade.php`

```php
<h1>{{ $title }}</h1>
@include('shared.notify')
```

\- `routes/web.php`

```php
use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return view('home', ['title' => 'sonnguyen singer', 'alertMessage' => 'livestream facebook at 9pm everyday']);
})->name('home');
```

View-source code page home

```php+HTML
<h1>
  sonnguyen singer
</h1>
<div class="alert">
  livestream facebook at 9pm everyday
</div>
```

**@includeIf**

Trong blade template nếu bạn `@include` một view không tồn tại thì Laravel sẽ throw ra một error. Nếu bạn muốn bỏ qua exception đó thì bạn có thể dùng `@includeIf`. Directive này sẽ check nếu như view tồn tại thì mới thực thi việc include view.

Còn cách sử dụng thì tương tự như đối với `@include`.

**@includeWhen**

Trong một số trường hợp bạn muốn kiểm tra điều kiện trước khi nhúng view thì bạn có thể sử dụng `@includeWhen` với cú pháp:

```php
@includeWhen($boolean, 'view.name', $data)
```

**Trong đó**: `$boolean` trả về `true` thì view sẽ được include và ngược lại `false` thì view sẽ không được include.

**@includeUnless**

Đây là directive phủ định của `@includeWhen`. Nghĩa là `$boolean` trả về `true` thì view sẽ không được include và ngược lại `false` thì view sẽ được include.

**Cú pháp**:

```php
@includeUnless($boolean, 'view.name', $data)
```

**@includeFirst**

Directive này cho phép chúng ta truyền vào một list view. Và nó sẽ kiểm tra xem nếu view nào tồn tại đầu tiên trong list thì nó sẽ nhúng view đó. Các view phía sau sẽ không được nhúng nữa.

**Cú pháp**:

```php
@includeFirst(['view.name', 'view.name1', 'view.name2'], $data)
```



Bạn cũng có thể nhúng view qua mỗi lần lặp một mảng hay một collection trong Blade template với directive `@each` với cú pháp:

**Trong đó**:

- `view.name` là view bạn muốn nhúng vào view hiện tại.
- `$array` là mảng, collection data bạn muốn lặp.
- `item` là giá trị sẽ được assign qua mỗi lần lặp.
- `view.empty` là view sẽ được nhúng khi `$array` trống. **Giá trị này có thể bỏ qua**.

##### 3.3.7 The @once Directive

Directive này cho phép chúng ta thực thi hành động bên trong nó một lần duy nhất khi render view. Ví dụ có 2 chỗ cùng nhúng một đoạn code nếu như sử dụng directive `@once` thì các đoạn code phía sau once đầu tiên được thực sẽ không được thực thi nữa.

```php+HTML
@once
    @push('scripts')
        <script>
            // Your custom JavaScript...
        </script>
    @endpush
@endonce
```



##### 3.3.8 Raw PHP

Trong một số trường hợp bạn muốn sử dụng code PHP chứa logic code trong blade template bạn có thể sử dụng directive `@php` với cú pháp như sau:

```php
@php
    $counter = 1;
@endphp
```

##### 3.3.9 Comments

Để comment trong Blade template các bạn sử dụng cặp dấu `{-- content --} `với content là nội dung bạn muốn comment.

#### 3.4 Components 

#### 3.5 Building Layouts

#### 3.6 Forms

- [CSRF Field](https://laravel.com/docs/8.x/blade#csrf-field)
- [Method Field](https://laravel.com/docs/8.x/blade#method-field)
- [Validation Errors](https://laravel.com/docs/8.x/blade#validation-errors)

#### 3.7 Stacks

#### 3.8 Service Injection

#### 3.9 Extending Blade 

##### 3.9.1 Custom Echo Handlers 

##### 3.9.2 Custom If Statements



#### 4. ISSUES

- Chưa tìm hiểu kĩ và chi tiết về middleware và middleware trong controller

- Đã đọc toàn bộ document về Blade Template nhưng chưa nhớ được hết, hiểu bài nhưng vì nó dài và chưa biết nên trình bày sao cho đẹp trên file markdown nên chưa thể hiện nội dung . 

- Nắm được 70% - 80% nội dung Components trong Blade Template

  

