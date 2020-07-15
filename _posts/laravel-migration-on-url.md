---
title: รันคำสั่ง migration บน url
tags: laravel
---

เพิ่มคำสั่งที่ `routes.php`

```php
//Clear route cache:
Route::get('/migration', function() {
    $exitCode = Artisan::call('migration');
    return 'migration';
});
```
