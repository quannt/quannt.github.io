---
layout: post
title: "Vuejs - lỗi dễ gặp khi sử dụng v-model và nested object"
categories: [programming]
fullview: false
---

`v-model` chỉ  tự động tạo reactive properties cho các properties thuộc **1 level deep** của model

Code lỗi
```
<template>
	<input v-model="foo.bar"> // this will work
    <input v-model="foo.bar.baz"> // Exception: cannot read property 'baz' of undefined
</template>

<script>
export default {
  ....
  data() {
    return {
      foo: { }
    };
  }
}
</script>
```

Code đúng, phải khai báo toàn bộ các properties từ level 2 deep trở xuống nếu có.
```
<template>
	<input v-model="foo.bar"> // this will work
    <input v-model="foo.bar.baz"> // this will work
</template>

<script>
export default {
  ....
  data() {
    return {
      foo: {
        bar: {
          baz: undefined
         }
	   }
    };
  }
}
</script>
```

References:
1) https://github.com/vuejs/vue/issues/3732
2) https://github.com/vuejs/vue/issues/5932
