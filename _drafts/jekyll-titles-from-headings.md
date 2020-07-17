---
tags: jekyll
---

# ใช้ Header หรือ h1 ใน Markdown ให้เป็น title ใน jekyll

1. เพิ่ม `gem 'jekyll-titles-from-headings'` ลงใน Gemfile
1. รันคำสั่ง `bundle update` เพื่อติดตั้งมัน
1. เพิ่ม **jekyll-titles-from-headings** ลงใน plugins array ใน `_config.yml`
1. เพิ่มคำสั่งตั้งค่าใน `_config.yml`:

    ```yml
    titles_from_headings:
        enabled:     true
        strip_title: true
        collections: true
    ```

1. ทดสอบด้วยการสร้าง Post ใหม่ `_posts/2018-04-10-title-strip.md`

    ```md
    ---
    title: "Title Strip Test Post"
    ---

    # Title Override
    ```

ค่า `page.title` ที่ได้จะเป็น Title Override ตาม Header ใน .md


__Ref:__ 
- [Jekyll Titles from Headings](https://github.com/benbalter/jekyll-titles-from-headings)
- [How to use Jekyll Titles from Headings](https://talk.jekyllrb.com/t/title-derived-from-heading-appears-twice-on-top-of-page/1686/4)