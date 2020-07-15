---
title: รันคำสั่ง Migration ของ Laravel บน url
tags: laravel
---

เพิ่มคำสั่งที่ `routes.php` แล้วก็เรียกใช้ผ่าน url `http://<domain>/migrate` 

```php
//Clear route cache:
Route::get('/migrate', function() {
    $exitCode = Artisan::call('migrate');
    return 'migration';
});
```

วิธีนี้ผมจะใช่กับ Server หรือ Host ที่ไม่ยอมให้่ `ssh` เข้าไป หรือใช้ `php artisan migrate` ไม่ได้
