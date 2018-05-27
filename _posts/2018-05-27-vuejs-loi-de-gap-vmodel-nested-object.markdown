---
layout: post
title: "Vuejs - lỗi dễ gặp khi sử dụng v-model và nested object"
categories: [programming]
fullview: false
---

`v-model` chỉ  tự động tạo reactive properties cho các properties thuộc **1 level deep** của model

Code lỗi
<script src="https://gist.github.com/quannt/531eedbd300dc4706c08018da53b667a.js"></script>

Code đúng, phải khai báo toàn bộ các properties từ level 2 deep trở xuống nếu có.
<script src="https://gist.github.com/quannt/d1662ca2cc69ff2a7c66c3175de2d247.js"></script>

References:
* [https://github.com/vuejs/vue/issues/3732](https://github.com/vuejs/vue/issues/3732)
* [https://github.com/vuejs/vue/issues/5932](https://github.com/vuejs/vue/issues/5932)
