---
layout: post
title:  "Review: Laravel Medialibrary - ไม่ต้องสร้าง Culumn เพิ่ม"
date:  2020-07-12 10:18:50 +0700
categories: laravel
---
<!-- # Review: Laravel-medialibrary -->

ผมใช้ php:7.4 และ Laravel:7 ในการสาธิต เพื่อสาธิตให้เข้าใจง่าย ผมจะสร้างเว็บ Blog อย่างง่าย คือ สามารถ Post ข้อความและรูปได้ เริ่มจาก สร้าง migration Posts Table

```php
// class CreatePostsTable
Schema::create('posts', function (Blueprint $table) {
    $table->id();
    $table->text('content');
    $table->timestamps();
});
```

## ติดตั้ง [laravel-medialibrary](https://docs.spatie.be/laravel-medialibrary/v8/introduction/)

```sh
composer require "spatie/laravel-medialibrary:^8.0.0"
php artisan vendor:publish --provider="Spatie\MediaLibrary\MediaLibraryServiceProvider" --tag="migrations"
php artisan vendor:publish --provider="Spatie\MediaLibrary\MediaLibraryServiceProvider" --tag="config"
php artisan migrate
```

การทำงานของ ****Laravel-medialibrary**** ทำงานเริ่มต้น public path จึงทำให้มันไม่พบ file เมื่อเรา addMedia สามารถแก้ได้ดังนี้ 

```php
// config/filesystems.php

'links' => [
    public_path('storage') => storage_path('app/public'),
    public_path('media') => storage_path('app/media'),
],
```

จากนั้น `php artisan storage:link` เพื่อสร้าง Link directory ให้เข้าถึงไฟล์ได้ผ่าน public 

```php
// config/media-library.php

'disk_name' => env('MEDIA_DISK', 'media'),
```

## การใช้งาน

เพิ่มความสามารถในการเก็บรูปใน Post Model ด้วย `implements Spatie\MediaLibrary\HasMedia` และ `use  Spatie\MediaLibrary\InteractsWithMedia`

```php
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Spatie\MediaLibrary\HasMedia;
use Spatie\MediaLibrary\InteractsWithMedia;

class Post extends Model implements HasMedia
{
    use InteractsWithMedia;
}
```

สร้าง UI สร้างสำหรับ เขียน Post และ Upload รูป สำคัญเลยต้องมี `enctype="multipart/form-data"` เพราะผมลืมบ่อยมาก

```html
<!-- resources/views/post/index.blade.php -->
....
<form enctype="multipart/form-data" action="{{ route('posts.store') }}" method="POST">
    @csrf
    <div class="card-body">
        <div class="form-group">
            <textarea class="form-control" name="content"></textarea>
        </div>
        <div class="form-group">
            <input type="file" name="images" >
        </div>
        <button class="btn btn-primary">submit</button>
    </div>
</form>
...
```

ผลที่ได้

![demo](/images/posts/laravel-medialibrary/demo-input.png)

แล้วส่งรูปและข้อความไปยัง function store ใน PostController

```php
// app/Http/Controllers/PostController.php

public function store(Request $request)
{
    $path = $request->file('images')->store('media');

    $post = new Post;
    $post->content = $request->content;
    $post->save();

    $post->addMedia($path)->toMediaCollection();

    return back();
}
```

`toMediaCollection` ตรงนี้สามารถ[กำหนดได้หลายรูปแบบ](https://docs.spatie.be/laravel-medialibrary/v8/working-with-media-collections/simple-media-collections/) เช่น 

```php
$post->addMedia($path)->toMediaCollection('cover');
$post->addMedia($path)->toMediaCollection('banners');
$post->addMedia($path)->toMediaCollection('attach-image');
```
ทำให้ระบุได้เลยว่า รูปชุดนี้ใช้ทำอะไร รูปชุดนั้นใช้ทำอะไร 

## การแสดงผล
ทำได้งานๆ โดย `$item->getMedia()->first()` ได้เลยตามนี้ หรือ [วิธีอื่นๆ](https://docs.spatie.be/laravel-medialibrary/v8/advanced-usage/rendering-media/)

```html
<!-- resources/views/post/index.blade.php -->
...
@foreach ($posts as $item)
    <div class="card mb-3">
        <div class="card-body pb-1">
            <p>
                {!! $item->content !!}
            </p>
        </div>
        <picture>
            {{ $item->getMedia()->first() }}
        </picture>
    </div>
@endforeach
...
```
ผลที่ได้
![demo](/images/posts/laravel-medialibrary/demo.png)

นอกจากนี้ยังมีการ [Resize Image](https://docs.spatie.be/laravel-medialibrary/v8/converting-images/defining-conversions/) อยู่ก็สามาทำง่ายเพียงแค่เพิ่ม function ลงใน Model 

```php
...
use Spatie\MediaLibrary\MediaCollections\Models\Media;
...
public function registerMediaConversions(Media $media = null): void
{
    $this->addMediaConversion('thumb')
            ->width(368)
            ->height(232)
            ->sharpen(10);
}
...
```

จากนั้นเมื่อ addMedia ทุกครั้งมันก็จะ Resize Image ให้ทั้งที และนำไปใช้ด้วย 

```php
$media->getPath();  // the path to the where the original image is stored
$media->getPath('thumb'); // the path to the converted image with dimensions 368x232

$media->getUrl();  // the url to the where the original image is stored
$media->getUrl('thumb'); // the url to the converted image with dimensions 368x232
```

เท่านี้ ครับไปลองใช้กันได้   

**REF**
- [Laravel filesystem](https://laravel.com/docs/7.x/filesystem)
- [Install laravel-medialibrary](https://docs.spatie.be/laravel-medialibrary/v8/installation-setup/)

