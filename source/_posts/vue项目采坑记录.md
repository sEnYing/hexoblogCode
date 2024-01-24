---
title: vue项目采坑记录
date: 2023-09-25 14:59:04
description: 开发过程中遇到的坑记录，避免重复踩坑
tags: 
- bug
categories: 
- bug
---

```ts
// 1.tree全部展开折叠 
for (
    var i = 0;
    i < (this as any).$refs.tree.store._getAllNodes().length;
i++
) {
    (this as any).$refs.tree.store._getAllNodes()[
        i
        ].expanded = this.select.expandAll
}
// 2.el-tree回显默认选中项，set的各种方法不生效，需使用defaultCheckKeys
// 3.vue3引入图片需要用require，否则会报错
// 4.vue+axios下载pdf文件流时，打开文件内容为空白
// 在axios请求接口地址的位置添加配置项：
responseType: 'blob'

// 5.a-table中row-selection的hideDefaultSelections设置为true隐藏全选框不生效，应设置columnTitle=" "，columnTitle的值设置为空格。 
// 6.vue3使用echarts时，引入方式有变化，应为
import * as echarts from 'echarts'
// 缩放
const chartObj = echarts.init(
    document.getElementById(props.id) as HTMLElement,
)
chartObj.setOption(option)
window.addEventListener('resize', () => {
    chartObj.resize()
})
```

```ts
// 7.vue触发echarts图表交互事件，以及添加echars鼠标点击事件
const chartObj = echarts.init(
    document.getElementById('container') as HTMLElement,
)
chartObj.setOption(option)
window.addEventListener('resize', () => {
    chartObj.resize()
})
chartObj.on('click', (e: any) => {
    context.emit('mapClick', e.name)
})
chartObj.dispatchAction({ type: 'geoSelect', name: '新疆' })

dispatchAction({
    // 触发的什么交互事件
    type: 'geoSelect',
    // 可选，系列 index，可以是一个数组指定多个系列
    seriesIndex?: number|Array,
    // 可选，系列名称，可以是一个数组指定多个系列
    seriesName?: string|Array,
    // 数据的 index，如果不指定也可以通过 name 属性根据名称指定数据
    dataIndex?: number,
    // 可选，数据名称，在有 dataIndex 的时候忽略
    name?: string
})
```
