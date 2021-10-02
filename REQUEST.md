## Request

#### 1 Khái niệm request

Request trong Laravel là một object chứa các thông tin liên quan đến HTTP request hiện tại. Dựa vào object này chúng ta có thể lấy được các thông tin như input, cookie, file,..

Class `Request` này nằm trong `Illuminate\Http\Request` core của Laravel. Object này được base trên `http-foundation` package của Symfony Laravel chỉ custom lại một chút và thêm một số phương thức.

#### 2 Tương Tác Với Request Trong Laravel

Mặc định `Request` object đã được binding vào service container của Laravel rồi, nên chúng ta muốn dùng nó thì có thể binding trực tiếp vào trong argument khi cần.

**VD**: Inject trong Route.

```php
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::get('/', function (Request $request) {
    // code
});
```

**VD**: Inject trong Controller

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class UserController extends Controller
{
    /**
     * Store a new user.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        $name = $request->input('name');

        //
    }
}
```

Lúc này ở route bạn sẽ không cần phải khai báo param `$request` nữa.

```php
use App\Http\Controllers\UserController;

Route::post('/user', [UserController::class, 'store']);
```

##### 2.1 Truy vấn thông tin URl/PATH

Để lấy path của request các bạn sử dụng method `path`.

**VD**:

```php
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::get('/', function (Request $request) {
    return "Path: " . $request->path();
});
```



Trong trường hợp bạn muốn lấy ra URL của request các bạn có thể sử dụng phương thức `url`.

**VD**:

```php
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::get('/', function (Request $request) {
    return "Path: " . $request->url();
});
```



Trong trường hợp ta muốn lấy ra full URL của request các bạn có thể sử dụng phương thức `fullUrl`.

**VD**:

```php
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::get('/', function (Request $request) {
    return "Path: " . $request->fullUrl();
});
```



Để kiểm tra path hiện tại có match với một path rule nào đó hay không các bạn có thể sử dụng phương thức `is`.

**VD**: Kiểm tra xem path hiện tại có phải bắt đầu bằng `admin` hay không?

```php
if ($request->is('admin/*')) {
    //
}
```



Bạn cũng có thể sử dụng phương thức `routeIs` để kiểm tra tương tự như phương thức `is`, nhưng là check qua route name.

**VD**: Kiểm tra xem path hiện tại có phải nằm trong route name bắt đầu bằng `admin` không?

```php
if ($request->routeIs('admin.*')) {
    //
}
```



##### 2.2 Truy vấn method của request

Để lấy ra method của request các bạn có thể sử dụng phương thức `method`.

**VD**:

```php
$request->method();
```



Chúng ta cũng có thể sử dụng phương thức `isMethod` để kiểm tra phương thức của request.

**VD**: Kiểm tra xem request có phải `POST` request không?

```php
if ($request->isMethod('post')) {
    //
}
```



##### 2.3 Truy vấn thông tin headers

Nếu muốn truy vấn thông tin liên quan đến header của request các bạn có thể sử dụng phương thức `header`.

**VD**: Lấy ra `user-agent` của reuquest.

```php
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::get('/', function (Request $request) {
    return 'User Agent: ' . $request->header('user-agent');
});
```



Trong trường hợp request header cần lấy ra không tồn tại thì phương thức header sẽ trả về `null`. Nếu muốn thay đổi giá trị default này bạn có thể truyền thêm tham số thứ 2 vào hàm header.

**VD**:

```php
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::get('/', function (Request $request) {
    return 'Header: ' . $request->header('toidicode', 'Làm gì có header này');
});
```



Bạn cũng có thể lấy ra bearer token của request bằng cách sử dụng phương thức `bearerToken`.

**VD**:

```php
$token = $request->bearerToken();
```

Cách viết trên tương tự với:

```php
$token = $request->header('Authorization', '');
```



##### 2.4 Lấy ra Ip của người dùng

Để lấy ra địa chỉ ip address của người dùng các bạn sử dụng phương thức `ip`.

**VD**:

```php
$ipAddress = $request->ip();
```



### 3. Nhận Dữ Liệu Input

Để nhận tất cả dữ liệu input gửi lên request, các bạn sử dụng phương thức `all`.

**VD**:

```php
$input = $request->all();
```



Trong trường hợp bạn muốn lấy ra dữ liệu của một input cụ thể (bao gồm payload và query string) bạn có thể sử dụng phương thức `input`.

**VD**:

```php
$name = $request->input('name');
```



Mặc định, Laravel sẽ trả về `null` nếu input bạn cần lấy không tồn tại. Bạn cũng có thể xác định giá trị mặc định cho input bằng cách truyền thêm tham số thứ 2 vào phương thức `input`.

**VD**:

```php
$name = $request->input('name', 'nguyenvanson');
```



Trong trường hợp dữ liệu bạn cần truy xuất là một mảng thì bạn có thể sử dụng "`.`" thay cho cách truy cập mảng.

**VD**:

```php
$name = $request->input('products.0.name');
// tương đương product[0]['name']

$names = $request->input('products.*.name');
// tương đương với lấy hết ra name của products
```



Nếu bạn không truyền tham số nào vào phương thức `input` thì Laravel sẽ trả về tất cả dữ liệu gửi lên request (tương tự như phương thức `all`).

**VD**:

```php
$input = $request->input();

// Tương đương với

$input = $request->all();
```



Trong một số trường hợp bạn có sử dụng các thẻ HTML như checkbok, radio,... Và bạn chỉ cần kiểm tra giá trị của nó là `true` hay `false` thôi, thì bạn có thể sử dụng phương thức `boolean`. Phương thức này sẽ nhận các giá trị `1`, "`1`", `true`, "`true`", "`on`", và "`yes`" là `true`, còn ngược lại sẽ là `false`.

**VD**:

```php
$archived = $request->boolean('archived');
```



Laravel cũng có apply magic method vào trong Request, nên bạn hoàn toàn có thể access đến một input dưới dạng property trong object. Đối với cách này thì Laravel sẽ ưu tiên lấy ra giá trị trong payload trước.

**VD**: Lấy ra dữ liệu của input name.

```php
$name = $request->name;
```



Đôi lúc bạn cần lấy ra một số input nhất định nào đó nhưng không phải tất cả thì bạn có thể sử dụng phương thức `only`.

**VD**: Chỉ lấy ra giá trị input username, password

```php
$input = $request->only(['username', 'password']);

// hoặc

$input = $request->only('username', 'password');
```



Hoặc ngược lại bạn muốn bỏ qua input nào đó còn lại sẽ lấy hết thì bạn có thể sử dụng phương thức `except`.

**VD**: Bỏ qua input credit_card còn lại lấy hết.

```php
$input = $request->except(['credit_card']);

// hoặc

$input = $request->except('credit_card');
```

#### 4. Nhận Dữ Liệu Query String

Trong một số trường hợp bạn chỉ muốn lấy ra query string của dữ liệu gửi lên request. Bạn có thể sử dụng phương thức `query`. Hàm này sẽ trả về một mảng data chứa các query string gửi lên request.

**VD**:

```php
$query = $request->query();
```



Nếu bạn cần lấy giá trị của một query tring cụ thể, bạn có thể truyền query string name vào phương thức `query`.

**VD**:

```php
$name = $request->query('name');
```



Mặc định, Laravel sẽ trả về `null` nếu query string bạn cần lấy không tồn tại. Bạn cũng có thể xác định giá trị mặc định cho input bằng cách truyền thêm tham số thứ 2 vào phương thức `query`.

**VD**:

```php
$name = $request->query('name', 'nguyenvanson');
```

#### 5. Kiểm Tra Dữ Liệu Input

Để kiểm tra dữ liệu gửi lên có input nào đó hay không các bạn sử dụng phương thức has. Phương thức này sẽ trả về true nếu input xuất hiện trong request và ngược lại sẽ là false.

**VD**:

```php
if ($request->has('name')) {
    //
}
```



Bạn cũng có thể kiểm tra nhiều input với phương thức `has` bằng cách truyền vào một mảng các input. Lúc này phương thức sẽ trả về `true` nếu tất cả các input cần kiểm tra đều xuất hiện trong request, ngược lại nó sẽ trả về `false`.

**VD**:

```php
if ($request->has(['name', 'email'])) {
    //
}
```

Cách viết trên tương tự với:

```php
if ($request->has('name') && $request->has('email')) {
    //
}
```



Nếu như bạn cần kiểm tra một mảng các input và sẽ trả về `true` nếu có một hoặc nhiều input xuất hiện trong danh sách đó. Bạn có thể sử dụng phương thức `hasAny`.

**VD**:

```php
if ($request->hasAny(['name', 'email'])) {
    //
}
```

Cách viết trên tương tự với:

```php
if ($request->has('name') || $request->has('email')) {
    //
}
```



Đôi lúc bạn cần xử lí một số hành động, logic khi input có xuất hiện trong request bạn có thể sử dụng hàm `whenHas`.

**VD**:

```php
$request->whenHas('name', function ($input) {
    //
});
```

Lúc này nếu trong request có input name thì callback function sẽ được thực thi.



Đôi lúc bạn cần kiểm tra thêm input nào đó có xuất hiện trong input và phải có giá trị hay không? Bạn có thể sử dụng phương thức `filled`. Phương thức này sẽ trả về `true` nếu input có xuất hiện trong input và có giá trị kèm theo.

**VD**:

```php
if ($request->filled('name')) {
    //
}
```



Tương tự, chúng ta cũng có thể dùng hàm `whenFilled` để thực thi logic khi input nào đó tồn tại và có giá trị.

**VD**:

```php
$request->whenFilled('name', function ($input) {
    //
});
```



Nếu bạn cần check xem một input nào đó không xuất hiện trong request, bạn có thể sử dụng phương thức `missing`. Phương thức này sẽ trả về `true` nếu input không xuất hiện trong request.

**VD**:

```php
if ($request->missing('name')) {
    //
}
```

#### 6. Old Input

Mặc định, Laravel sẽ lưu trữ lại giá trị các input của request để phục vụ request tiếp theo như recover lại dữ liệu khi validate sai,... Và quá trình này đã được làm tự động hết rồi. Nhưng ở đây mình muốn giới thiệu với mọi người thêm một số phương thức để sử dụng trong trường hợp cần thiết.

- `flash`: Phương thức này sẽ lưu hết dữ liệu input vào session. Và sẽ được xóa đi nếu bạn request lại lần tiếp theo.
- `flashOnly`: Phương thức này sẽ lưu trữ các input được xác định vào session, còn lại sẽ bỏ qua.
- `flashExcept`: Phương thức này sẽ bỏ qua các input được xác định vào session, còn lại sẽ lưu trữ.

**VD**:

```php
$request->flash();

$request->flashOnly(['username', 'email']);

$request->flashExcept('password');
```



Để lấy ra giá trị của input trước đó đã được store các bạn sử dụng phương thức `old`. Nếu dữ liệu không có giá trị phương thức sẽ trả về `null`.

**VD**:

```php
$username = $request->old('username');
```



Hoặc có thể sử dụng hàm `old` trong helper.

```php
old('username')
```



Bạn cũng có thể xác định thêm giá trị mặc định khi input không có giá trị bằng cách truyền vào tham số thứ 2.

**VD**:

```php
$username = $request->old('username', 'sonsokiu');
```

#### 7. Input Trimming & Normalization

Mặc định, Laravel sử dụng 2 middleware `App\Http\Middleware\TrimStrings` và `App\Http\Middleware\ConvertEmptyStringsToNull` để làm nhiệm vụ xử lí, chuẩn hóa dữ liệu trước khi đưa vào Request object.

- Middleware `TrimStrings` có tác dụng loại bỏ đi các khoảng trống (white space) ở các input gửi lên request.
- Middleware `ConvertEmptyStringsToNull` có tác dụng chuyển đổi string rỗng thành null.

Nếu như bạn không muốn tắt 2 middleware này đi thì có thể sửa trong `app/Http/Kernel.php`.

#### 8. File

Bạn cũng có thể lấy ra được file upload lên trên request qua phương thức file. Phương thức này sẽ trả về một Object `UploadedFile` (`Illuminate\Http\UploadedFile`) nếu file đó tồn tại và null nếu file đó không tồn tại.

Object `UploadedFile` này được kế thừa từ `SplFileInfo` mặc định của PHP, nên các bạn có thể sử dụng được các phương thức trong `SplFileInfo`.

**VD**: Lấy ra thông tin file photo upload lên request.

```php
$file = $request->file('photo');
```

Ngoài ra bạn cũng có thể trỏ trực tiếp đến file name qua property.

```php
$file = $request->photo;
```

Trong trường hợp bạn cần kiểm tra xem một file nào đó có xuất hiện trong request hay không thì bạn có thể sử dụng phương thức hasFile.

**VD**: Kiểm tra xem request gửi lên có file photo không?

```php
if ($request->hasFile('photo')) {
    //
}
```

Ngoài ra bạn cũng có thể kiểm tra xem file được upload lên có thành công hay không. Bằng cách sử dụng phương thức isValid.

**VD**:

```php
if ($request->file('photo')->isValid()) {
    //
}
```

