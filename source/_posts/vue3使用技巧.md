---
title: vue3使用技巧
date: 2023-09-22 10:31:17
description: vue3的基础使用技巧，没记住的内容，省的使用时再去百度
tags: 
- vue
categories: 
- vue
---

```vue
// vue3的setup语法糖内使用props
import { defineProps } from "vue";
const props = defineProps(['userInfo', 'gameId']);
// vue3的setup语法糖内使用emit
import { defineEmit } from 'vue';
const emit = defineEmit(['kk', 'up']);
const handleClick = () => {
  emit('click', '点了我');
};
// vue3的setup语法糖内获取上下文
import { useContext } from 'vue'
const { slots, attrs } = useContext();
// 模块文件夹结构建议，例如：
@/views/HomePage/index.vue
@/views/HomePage/index.scss
@/views/HomePage/ts/add.ts
@/views/HomePage/ts/reduce.ts
@/views/HomePage/ts/review.ts
@/views/HomePage/ts/index.ts

// @/views/HomePage/ts/index.ts文件内容
const files = require.context('.', true, /\.ts/)
const modules: any[] = []
files.keys().forEach((key) => {
    if (key === './index.ts') {
      return
    }

    const mk = key.replace(/(^\.\/|\.ts$)/g, '')
    const mkArr = mk.split('/')
    const m = files(key)
  const setFile = function (json: any, arr: any, file: any) {
    if (arr.length > 1) {
      setFile(json, arr.slice(1, arr.length), file)
    } else {
      if (file && file.path) json.push(file)
  }
}
const file = Object.keys(m).reduce((s, e) => {
  if (e !== 'default') {
    s[e] = m[e]
  }
  return s
  }, m.default || {})
setFile(modules, mkArr, file)
})
export default modules
```
