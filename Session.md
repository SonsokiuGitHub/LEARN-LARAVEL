## Session

#### 1. Khái Niệm

##### 1.1 Session

**Session** là thông tin về phiên làm việc cho từng khách truy cập, trong PHP nó tạo một file trong thư mục tạm (thư mục này cài đặt ở `php.ini : session.save_path`) để lưu thông tin này, thông tin này được dùng chung cho tất cả các trang mà khách truy cập.

#### 2. Cấu Hình Session

Bạn có thể cấu hình session trong file `config/session.php`.

Mặc định Laravel sẽ lưu trữ session bằng file, nhưng nếu như ứng dụng của bạn sửa dụng load balance thì cần config session qua Redis, hoặc database để các worker có thể connect được đồng thời.

#### 3. Session Driver Hỗ Trợ Trong Laravel

##### 3.1 File - Session

session sẽ được lưu trữ lại `storage/framework/sessions` (các bạn có thể thay đổi được thư mục).

##### 3.2 Cookie - Session

sessions sẽ được lưu trữ vào cookie và sẽ được mã hóa an toàn trước khi lưu vào cookie.

##### 3.3 Database - Session

sessions sẽ được lưu trữ vào database.

##### 3.4 Memcached / redis - Session

session sẽ được lưu trữ vào trong bộ nhỡ cache của các ứng dụng này.

##### 3.5 Dynamodb - Session

session sẽ được lưu trữ vào trong  AWS DynamoDB.

##### 3.6 Array - Session

session sẽ được lưu trữ vào một array trong PHP. Driver này chỉ nên dùng cho testing.



#### 4. Các Yêu Cầu Cho Từng Session Driver

##### 4.1 Database

Khi bạn sử dụng driver là database thì bạn cần phải tạo ra một `table` để lưu trữ thông tin session. Để tạo `table` các bạn chạy lệnh artisan sau:

```php
php artisan session:table
```

Laravel sẽ sinh ra cho các bạn một file migrate có dạng như sau `database/migrations/XXXX_XX_XX_XXXXXX_create_sessions_table.php`.

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateSessionsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('sessions', function (Blueprint $table) {
            $table->string('id')->primary();
            $table->foreignId('user_id')->nullable()->index();
            $table->string('ip_address', 45)->nullable();
            $table->text('user_agent')->nullable();
            $table->text('payload');
            $table->integer('last_activity')->index();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('sessions');
    }
}
```

Tiếp theo các bạn cần migrate nó vào database.

```php
php artisan migrate
```

##### 4.2 Redis

Trước khi sử dụng Redis làm driver trong Laravel, bạn cần phải cài đặt `redis` extension trong PHP (sử dụng PECL), hoặc sử dụng `predis/predis` package qua composer.

Trong đó thì mọi người nên sử dụng redis extension của PHP để cho performance tốt nhất.

#### 5. Sử Dụng Session

##### 5.1 Lấy Giữ Liệu

Trong Laravel, có hai cách chính để các bạn có thể lấy được session. Đó là lấy thông qua `session` helper hoặc sử dụng qua `Request` instance.

Để lấy dữ liệu trong session các bạn sử dụng phương thức `get`.

**VD**: các cách lấy dữ liệu session.

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Session;

class UserController extends Controller
{
    /**
     * Show the profile for the given user.
     *
     * @param  Request  $request
     * @param  int  $id
     */
    public function show(Request $request, $id)
    {
        $value = $request->session()->get('key');

        // hoặc

        $value = session('key');

        // hoặc

        $value = session()->get('key');

        // hoặc

        $value = Session::get('key');
    }
}
```

Trong đó, nếu session key không tồn tại thì hàm get sẽ trả về `null`. Nếu bạn muốn thay giá trị mặc định khi key không tồn tại thì có thể truyền vào tham số thứ 2 sẽ là giá trị mặc định khi key không tồn tại.

**VD**:

```php
$value = $request->session()->get('key', 'default');

$value = $request->session()->get('key', function () {
    return 'default';
});
```

Để lấy tất cả session data, các bạn sử dụng phương thức `all`.

**VD**:

```php
use Illuminate\Support\Facades\Session;

$data = Session::all();
```

##### 5.2 Kiểm Tra Session

### Kiểm tra session

Để kiểm tra xem một sesion nào đó có tồn tại hay không các bạn sử dụng phương thức `has`. Phương thức này sẽ trả về `true` nếu session có tồn tại và khác `null`.

**VD**: kiểm tra session `users` có tồn tại và khác `null` không?

```php
if ($request->session()->has('users')) {
    //
}
```

Nếu bạn chỉ cần kiểm tra xem session có tồn tại hay không, còn giá trị của nó là gì không quan tâm thì có thể sử dụng phương thức `exists`. Phương thức này sẽ trả về true nếu trong session tồn tại key đó.

**VD**: Kiểm tra trong session có tồn tại key `users`.

```php
if ($request->session()->exists('users')) {
    //
}
```



##### 5.3 Lưu Trữ Session

Để lưu trữ sesion các bạn sử dụng phương thức `put` với cú pháp:

```php
session()->put($key, $value);
```

**Trong đó**:

- `$key` là key của session các bạn muốn lưu trữ.
- `$value` là giá trị của session các bạn muốn lưu trữ.

**VD**:

```php
use Illuminate\Support\Facades\Session;

$sessions = Session::put("cart.products", [['id' => 1, 'item' => 1]]);
```

Trong trường hợp session key của bạn có giá trị là array và bạn muốn push thêm data vào session key đó, bạn có thể sử dụng phương thức `push`.

**VD**: Push thêm product vào session key `cart.products` ở ví dụ trên.

```php
use Illuminate\Support\Facades\Session;

$sessions = Session::push("cart.products", ['id' => 2, 'item' => 1]);
```

Trong trường hợp, bạn muốn lấy ra session data xong xóa luôn key đó khỏi session. Bạn có thể sử dụng phương thức `pull`.

**VD**: Lấy ra `cart.products` và xóa luôn data khỏi session.

```php
use Illuminate\Support\Facades\Session;

$sessions = Session::pull("cart.products");
```

Đối với phương thức này bạn cũng có thể set thêm giá trị default nếu như session key không tồn tại, bằng cách sử dụng tham số thứ 2.

Bạn cũng có thể tăng giảm giá trị của session bằng cách sử dụng phương thức `increment` và `decrement`.

**VD**:

```php
$request->session()->increment('count'); 

$request->session()->increment('count', $incrementBy = 2);

$request->session()->decrement('count');

$request->session()->decrement('count', $decrementBy = 2);
```

##### 5.4 Flash Session

Flash session là một dạng session chỉ tồn tại được trong một request, rồi sẽ bị xóa. Loại session này thường được sử dụng để làm các chức năng như validate, flash notify.

Để sử dụng flash session, các bạn sử dụng phương thức `flash` với cú pháp:

```php
use Illuminate\Support\Facades\Session;

$sessions = Session::flash($key, $value);
```

**Trong đó**:

- `$key` là key của session các bạn muốn lưu trữ.
- `$value` là giá trị của session các bạn muốn lưu trữ.

Trong trường hợp bạn muốn giữ flash session tồn tại thêm một request nữa, bạn có thể sử dụng phương thức `reflash`.

**VD**:

```php
$request->session()->reflash();
```

##### 5.5 Xoá Dữ Liệu

Bạn có thể sử dụng phương thức `forget` để xóa session trong Laravel.

**VD**:

```php
// Forget a single key...
$request->session()->forget('name');

// Forget multiple keys...
$request->session()->forget(['name', 'status']);
```

Nếu bạn muốn xóa hết session data, bạn có thể sử dụng phương thức `flush`.

**VD**:

```php
$request->session()->flush();
```

#### 6. Khởi Tạo Lại Session ID

Trong một vài trường hợp nếu như bạn muốn thay đổi lại session ID, bạn có thể sử dụng phương thức `regenerate`.

**VD**:

```php
$request->session()->regenerate();
```

Nếu như bạn muốn thay đổi session ID và xóa hết session data, bạn có thể sử dụng phương thức `invalidate`.

**VD**:

```php
$request->session()->invalidate();
```

