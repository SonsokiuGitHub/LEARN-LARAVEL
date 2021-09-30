**# Mục Lục**

\1. Các Version Laravel

\1. Cấu Trúc Request LifeCirle Trong Laravel

\1. Route Là Gì ? Cách Sử Dụng

\---

**## 1. Các Version Laravel**

**####** **`Laravel 1`**

Bản Laravel beta đầu tiên được phát hành vào ngày 9/6/2011, tiếp đó là Laravel 1 phát hành trong cùng tháng. Laravel 1 bao gồm các tính năng như xác thực, bản địa hóa, model, view, session, định tuyến và các cơ cấu khác, nhưng vẫn còn thiếu controller, điều này làm nó chưa thật sự là một MVC framework đúng nghĩa.



**####** **`Laravel 2`**

Được phát hành vào tháng 9 năm 2011, mang đến nhiều cài tiến từ tác giả và cộng đồng. Tính năng đáng kể bao gồm hỗ trợ controller, điều này thực sự biến Laravel 2 thành một MVC framework hoàn chỉnh, hỗ trợ Inversion of Control (IoC), hệ thống template Blade. Bên cạnh đó, có một nhược điểm là hỗ trợ cho các gói của nhà phát triển bên thứ 3 bị gỡ bỏ.



**####** **`Laravel 3`**

Được phát hành vào tháng 2 năm 2012, với một tấn tính năng mới bao gồm giao diện dòng lệnh (CLI) tên “Artisan”, hỗ trợ nhiều hơn cho hệ thống quản trị cơ sở dữ liệu, chức năng ánh xạ cơ sở dữ liệu Migration, hỗ trợ “bắt sự kiện” trong ứng dụng,  và hệ thống quản lý gói gọi là “Bundles”. Lượng người dùng và sự phổ biến tăng trưởng mạnh kể từ phiên bản Laravel 3.



**####** **`Laravel 4`**

Tên mã `“Illuminate”`, được phát hành vào tháng 5 năm 2013. Lần này thực sự là sự lột xác của Laravel framework, di chuyển và tái cấu trúc các gói hỗ trợ vào một tập được phân phối thông qua Composer, một chương trình quản lý gói thư viện phụ thuộc độc lập của PHP. Bố trí mới như vậy giúp khả năng mở rộng của Laravel 4 tốt hơn nhiều so với các phiên bản trước. Ra mắt lịch phát hành chính thức mỗi sáu tháng một phiên bản nâng cấp nhỏ. các tính năng khác trong Laravel 4 bao gồm tạo và thêm dữ liệu mẫu (database seeding), hỗ trợ hàng đợi, các kiểu gởi mail, và hỗ trợ “xóa mềm”  (soft-delete: record bị lọc khỏi các truy vấn từ Eloquent mà không thực sự xóa hẳn khỏi DB).



**####** **`Laravel 5`**

Được phát hành trong tháng 2 năm 2015, như một kết quả thay đổi đáng kể cho việc kết thúc vòng đời nâng cấp Laravel lên 4.3. Bên cạnh một loạt tính năng mới và các cải tiến như hiện tại, Laravel 5 cũng giới thiệu cấu trúc cây thư mục nội bộ cho phát triển ứng dụng mới. Những tính năng mới của Laravel 5 bao gồm hộ trợ lập lịch định kỳ thực hiện nhiệm vụ thông qua một gói tên là “Scheduler”, một lớp trừu tượng gọi là “Flysystem” cho phép điều khiển việc lưu trữ từ xa đơn giản như lưu trữ trên máy local – dễ thấy nhất là mặc định hỗ trợ dịch vụ Amazone S3, cải tiến quản lý assets thông qua “Elixir”, cũng như đơn giản hóa quản lý xác thực với các dịch vụ bên ngoài bằng gói “Socialite”.



**####** **`Laravel 5.1`**

Phát hành vào tháng 6 năm 2015, là bản phát hành đầu tiên nhận được hỗ trợ dài hạng (LTS) với một kết hoạch fix bug lên tới 2 năm vào hỗ trợ vá lỗi bảo mật lên tới 3 năm. Các bản phát hành LTS của Laravel được lên kế hoạch theo mỗi 2 năm



**####** **`Laravel 5.3`**

Được phát hành vào ngày 23 tháng 8 năm 2016. Các tính năng mới trong 5.3 tập trung vào việc cải thiện tốc độ phát triển bằng cách bổ sung thêm các cải tiến cho các tác vụ phổ biến



**####** **`Laravel 5.4`**

Phiên bản này có nhiều tính năng mới, như Laravel Dusk, Laravel Mix, Blade Components và Slots, Markdown Emails, Automatic Facades, Route Improvements, Higher Order Messaging cho Collections, và nhiều thứ khác



**####** **`Laravel 5.5`**

Phát hành vào ngày 30 tháng 8 năm 2017 là phiên bản LTS thứ 2



**####** **`Laravel 5.6`**

Phát hành vào ngày 7 tháng 2 năm 2018



**####** **`Laravel 5.7`**

Phát hành vào ngày 4 tháng 9 năm 2018 với những cập nhật.



\- Cải thiện thông báo lỗi

\- Callable Action URLs

\- Email Verification

\- Bổ sung phương thức mới cho tùy chỉnh phân trang

\- Thay đổi cấu trúc thư mục Resource



**####** **`Laravel 6`**

Phát hành vào ngày 3 tháng 9 năm 2019. Đây là version LTS. Phiên bản này có gì mới??



\- Đổi versioning scheme sang Semantic Versioning

\- Cải thiện Exceptions thông qua Ignition

\- Cải thiện Authorization Responses

\- Job Middleware

\- Lazy Collections

\- Cải tiến Eloquent subquery



**####** **`Laravel 7`**

Ra mắt ngày 3 tháng 3 năm 2020 với nhiều tính năng cũng như cải thiện tốc độ



\- Laravel Airlock

\- Tùy chỉnh Eloquent Casts

\- Blade Component Tags & Improvements

\- HTTP Client

\- Fluent String Operations

\- Cải thiện Route Model Binding

\- Multiple Mail Drivers

\- Cải thiện tốc độ Route Caching

\- CORS Support

\- Query Time Casts

\- Cải thiện MySQL 8+ Database Queue

\- Lệnh Artisan test

\- Cải thiện Markdown Mail Template

\- Tùy chỉnh Stub

\- Queue maxExceptions Configuration



[Tham khảo chi tiết các cập nhật mới laravel 7 tại đây](https://viblo.asia/p/laravel-7-co-gi-moi-1Je5EndmKnL?fbclid=IwAR2X4pJI1wjRE6IMpM9t4VGu9ZEJ3raTgxM0duhVO76LjuMdnvy6pQG2v1Y)





**####** **`Laravel 8`**

Phát hành vào 8/9/2020 với nhiều cải tiến và tính năng mới



\- Trang đích mới

\- Thư Mục app/models mặc định

\- Gỡ bỏ Controller namespace prefix

\- Cải thiện route caching

\- Thuộc tính Blade component

\- Cú pháp Event Listener dựa trên Closure rõ ràng hơn

\- Truy cập bí mật: Maintenance mode

\- Trang pre-render trong chế độ bảo trì (Maintenance mode)

\- Xử lý lỗi queued closure

\- Tùy chỉnh thời gian thử lại sau mỗi job failed

\- Gửi job batching vào queue

\- Cải thiện Rate Limiting

\- Kiết xuất database schema

\- Model Factory mới

\- Laravel JetStream và Laravel Fortify



[Tham khảo chi tiết các cải tiến, tính năng mới của Laravel 8 tại đây](https://viblo.asia/p/laravel-8-co-gi-moi-eW65G1aLZDO)



\---

**## 2. Cấu Trúc LifeCirle Trong Laravel**



\>***\****_Request Lifecycle in Laravel_****\***

\>>Vòng đời một request trong Laravel bắt đầu từ khi người dùng thực hiện một thao tác gửi một request lên ứng dụng laravel cho đến khi nhận được kết quả trả về (response) từ server



\- Bước đầu tất cả các `request` gửi lên từ `clients` đều thông qua thư mục public và chạy vào file index.php .

File index.php sẽ load tất cả các thư viện cần thiết trong autoload.php . Một instance app được khởi tạo ra trong bootstrap/app.php để chuẩn bị cho việc khởi động ứng dụng. Một số interfaces quan trọng sẽ được binding tại đây từ trong Service Container. `Kernel` sẽ được khởi tạo để handle request thông qua đoạn code



\- Tiếp theo, request được gửi đến `Kernel (HTTP hoặc Console)`, tùy thuộc vào môi trường đang thực thi là webapp hay console.

Kernel tập trung rất nhiều middleware liên quan. Nhiệm vụ của Kernel là khai báo tất cả các middleware mà request cần phải pass qua (xác thực hoặc đơn giản chỉ là ghi log – logging) trước khi request được tiếp tục xử lý phần logic. Kernel còn tập hợp các bootstrappers cần phải chạy trước khi request được tiếp tục xử lý: cấu hình xử lý lỗi (error handling), ghi log (logging)…

Về cơ bản, Kernel có nhiệm vụ khá đơn giản là một chiếc hộp đen nhận request và trả response để tiếp tục vòng đời request. Và một nhiệm vụ tối thượng khác của Kernel là load tất cả các `Service Providers`.

\- Các `Service Providers` được cấu hình trong file config/app.php, Kernel sẽ giúp tải trước các bootstraper nào cần dùng (các service providers core của Laravel và các thư viện bên thứ 3 mình sẽ sử dụng). Quá trình load các service provider trải qua 2 bước:



Đăng ký service provider (Register service provider)

Khởi động service provider (Bootstrap service provider)

Service Provider sẽ giúp khởi động các core service của Laravel như `router, validation, database…` Đây là lý do nó là trái tim của Laravel.



\- `Router` Sau khi load Service Provider, request sẽ tiếp tục gửi đến Route. Nhiệm vụ của Route dễ hiểu như chính cái tên của nó: điều hướng tất cả các request được gửi đến và quyết định hướng xử lý, có 2 hướng rẽ:



Route -> Middleware -> Controller/Action

Route -> Controller/Action

Trong Route, chúng ta có thể ràng buộc request đi qua chốt chặn Middleware tự tạo, hoặc đi thẳng vào Controller/Action.



\- `Middleware` là một giải pháp khá hay của Laravel để filtering các request một lần nữa trước khi bạn xử lý thuần logic. Một ví dụ đơn giản: bạn cần chuyển hướng người dùng sang đến màn hình đăng nhập khi chưa login, và nếu user đã login, request sẽ tiếp tục xử lý tùy thuộc vào controller/action. Nếu như ở một số kiến trúc cũ, đa phần việc này được viết thẳng ở Controllers (trong before action hoặc xử lý ở request trước khi vào controller), thay vì đó trong Laravel bạn có thể lập một Middleware để thực hiện xác thực việc này. Với mô hình phân tách này, code xử lý khá dễ hiểu và đơn giản hơn nhiều.

Ngoài ra, Middleware còn được sử dụng cho nhiều mục đích khác.



\- `Controller/Action, Views` Từ request lifeCirle trong laravel, ta thấy response có thể trả về người dùng từ 2 cách: response thông qua View hoặc không thông qua View. View trong Laravel chứa các template sẵn, và cần xử lý kèm theo các biến. 



**## 3. Route Là Gì ? Cách Sử Dụng Route.**

***\****_Route_****\*** trong laravel cũng như các framework khác, đều có chức năng định tuyến ra các `action` cho các sự kiện tương ứng.



Trong Laravel hỗ trợ chúng ta 3 loại route chính đó là:



\- Route dành cho web.

\- Route dành cho cli (commnad).

\- Route dành cho broadcast.



**### Sử Dụng Route cho web**

Để khai báo các route dành cho web ta khai báo trong 2 file routes/web.php hoặc routes/api.php. Trong đó, nếu khai báo route trong ***\*routes/api.php\**** thì các path sẽ được thêm tiền tố api ở phía trước.



Ngoài ra nếu như muốn khai báo route trong file khác thì cần khai báo file đó *_app/Providers/RouteServiceProvider.php._*



*_Route trong Laravel hỗ trợ chúng ta định nghĩa cho tất cả các HTTP request method như:_*



\- `Route::get($uri, $callback)` - nhận resquest với phương thức GET.

\- `Route::post($uri, $callback)` - nhận resquest với phương thức POST.

\- `Route::put($uri, $callback)` - nhận resquest với phương thức PUT.

\- `Route::patch($uri, $callback)` - nhận resquest với phương thức PATCH.

\- `Route::delete($uri, $callback)` - nhận resquest với phương thức DELETE.

\- `Route::options($uri, $callback)` - nhận resquest với phương thức OPTIONS.



\>Hoặc nếu như chúng ta muốn ***\*đăng kí một route hỗ trợ nhiều method\**** thì bạn có thể dụng phương thức `match` theo cú pháp:

\```

Route::match($method, $uri, $callback)

\```

\---



\>Hoặc là ***\*nhận tất cả các HTTP method\**** với phương thức `any`:

\```

Route::any($uri, $callback)

\```

\---

\>Nếu như bạn muốn đưa logic vào trong controller thì bạn có thể sửa dụng cú pháp sau:



\```

Route::routeMethod($uri,[$controllerName, $method]);

\```

Trong đó:



\- `routeMethod` là method action của route.

\- `$uri` là path mà bạn muốn xử lí.

\- `$controllerName` là controller chứa logic.

\- `$method` là phương thức trong controller chứa logic để xử lí cho route đó.

\---



\>Ngoài ra Laravel cũng hỗ trợ thêm các loại route khác như:



***\*Route redirect\****

\```

Route::redirect($uri, $redirectTo, $status)

\```

Trong đó: 





\- `$uri` là path mà bạn muốn xử lí.

\- `$redirectTo` là path mà bạn muốn redirect đến.

\- `$status` là http status mà các bạn muốn thiết lập, mặc định $status là ***\*302\****



\---

***\*Route view\**** 

\```

Route::view($uri, $viewName, $data)

\```

Trong đó:



\- `$uri` là path mà bạn muốn xử lí.

\- `$viewName` là view mà bạn muốn render.

\- `$data` data mà bạn muốn truyền vào view. 

\- Bạn còn có thể truyền thêm 2 tham số khác vào đó là http status và header.



***\****_Chú Ý_****\*** Route view chỉ work với HTTP request `GET` hoặc `HEAD`.