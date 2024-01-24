---
title: js常用方法汇总-1
date: 2023-09-21 17:31:52
description: 项目内常用的js方法封装
tags:
  - javascript
categories:
  - javascript
---

```javascript
    // 1.格式化金钱
const ThousandNum = num => num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
// 2.判断数据类型
const DataType = (tgt, type) => {
    const dataType = Object.prototype.toString.call(tgt).replace(/\[object (\w+)\]/, "$1").toLowerCase();
    return type ? dataType === type : dataType;
}
// 3.是否为空数组
const flag = Array.isArray(arr) && !arr.length
// 4.是否为空对象
const flag = DataType(obj, "object") && !Object.keys(obj).length;
// 5.去重数组
const arr = [...new Set([0, 1, 1, null, null])];
// 6.过滤空值
const arr = [undefined, null, "", 0, false, NaN, 1, 2].filter(Boolean);
// 7.删除无用对象
const obj = {a: 0, b: 1, c: 2}; // 只想拿b和c
const {a, ...rest} = obj;
// 8.获取url参数
const getWindonHref = () => {
    var sHref = window.location.href;
    var args = sHref.split('?');
    if (args[0] === sHref) {
        return '';
    }
    var hrefarr = args[1].split('#')[0].split('&');
    var obj = {};
    for (var i = 0; i < hrefarr.length; i++) {
        hrefarr[i] = hrefarr[i].split('=');
        obj[hrefarr[i][0]] = hrefarr[i][1];
    }
    return obj;
}
// 9.防抖函数
const debounce = (handle, delay) => {
    var timer = null;
    return function () {
        var _self = this,
            _args = arguments;
        clearTimeout(timer);
        timer = setTimeout(function () {
            handle.apply(_self, _args)
        }, delay)
    }
}
// 10.节流函数
const throttle = (handler, wait) => {
    var lastTime = 0;
    return function (e) {
        var nowTime = new Date().getTime();
        if (nowTime - lastTime > wait) {
            handler.apply(this, arguments);
            lastTime = nowTime;
        }
    }
}
// 11.格式化时间
const formatDate = (t, str) => {
    var obj = {
        yyyy: t.getFullYear(),
        yy: ("" + t.getFullYear()).slice(-2),
        M: t.getMonth() + 1,
        MM: ("0" + (t.getMonth() + 1)).slice(-2),
        d: t.getDate(),
        dd: ("0" + t.getDate()).slice(-2),
        H: t.getHours(),
        HH: ("0" + t.getHours()).slice(-2),
        h: t.getHours() % 12,
        hh: ("0" + t.getHours() % 12).slice(-2),
        m: t.getMinutes(),
        mm: ("0" + t.getMinutes()).slice(-2),
        s: t.getSeconds(),
        ss: ("0" + t.getSeconds()).slice(-2),
        w: ['日', '一', '二', '三', '四', '五', '六'][t.getDay()]
    };
    return str.replace(/([a-z]+)/ig, function ($1) {
        return obj[$1]
    });
}
```
