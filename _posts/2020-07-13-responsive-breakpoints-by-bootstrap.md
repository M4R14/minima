---
title:  "เขียนเว็บ Responsive แต่จำขนาดไม่ได้ก็ไปดูที่ Bootstrap"
tags: bootstrap
---

การกำหนดขนาดหน้าจอ Responsive 5 ขนาดในแบบ [Botstrap][2] ประกอบด้วย xl lg md sm และ xs จากหัวข้อ [Responsive breakpoints](1) สิ่งที่นำมาใช่อย่างได้ผลมีดังนี้

```css
// Extra small devices (portrait phones, less than 576px)
@media (max-width: 575.98px) { ... }

// Small devices (landscape phones, 576px and up)
@media (min-width: 576px) and (max-width: 767.98px) { ... }

// Medium devices (tablets, 768px and up)
@media (min-width: 768px) and (max-width: 991.98px) { ... }

// Large devices (desktops, 992px and up)
@media (min-width: 992px) and (max-width: 1199.98px) { ... }

// Extra large devices (large desktops, 1200px and up)
@media (min-width: 1200px) { ... }
```


[1]: https://getbootstrap.com/docs/4.1/layout/overview/#responsive-breakpoints
[2]: https://getbootstrap.com/
