---
title: js常用方法汇总-2
date: 2023-09-27 15:08:14
description: 项目内常用的js方法封装
tags:
  - javascript
categories:
  - javascript
---

<h1>base64加解密</h1>

```javascript
const encode = (v) => {
    v = typeof v === "object" ? JSON.stringify(v) : v;
    v = Base64.enc.Utf8.parse(v);
    v = Base64.enc.Base64.stringify(v);
    return v;
}
const decode = (v) => {
    v = Base64.enc.Base64.parse(v);
    v = v.toString(Base64.enc.Utf8);
    try {
        let nv = JSON.parse(v);
        typeof nv === "object" ? (v = nv) : null;
    } catch (e) {
        v = v;
    }
    return v;
}
```

<h1>深拷贝</h1>

```javascript
const deepClone = (source) => {
    if (!source && typeof source !== 'object') {
        throw new Error('error arguments', 'deepClone')
    }
    const targetObj = source.constructor === Array ? [] : {}
    Object.keys(source).forEach(keys => {
        if (source[keys] && typeof source[keys] === 'object') {
            targetObj[keys] = deepClone(source[keys])
        } else {
            targetObj[keys] = source[keys]
        }
    })
    return targetObj
}
```

<h1>将普通列表数据转化为tree结构</h1>

```javascript
/**
 * 将普通列表数据转化为tree结构
 * @param array tree数据
 * @param opt  配置参数
 * @param startPid 父节点
 */
const listToTree = (array, opt, startPid) => {
        const obj = {
            primaryKey: opt.primaryKey || 'key',
            parentKey: opt.parentKey || 'parentId',
            titleKey: opt.titleKey || 'title',
            startPid: opt.startPid || '',
            currentDept: opt.currentDept || 0,
            maxDept: opt.maxDept || 100,
            childKey: opt.childKey || 'children',
        };
        if (startPid) {
            obj.startPid = startPid;
        }
        return toTree(array, obj.startPid, obj.currentDept, obj);
    };
/**
 *  递归构建tree
 * @param list
 * @param startPid
 * @param currentDept
 * @param opt
 * @returns {Array}
 */
const toTree = (array, startPid, currentDept, opt) => {
    if (opt.maxDept < currentDept) {
        return [];
    }
    let child = [];
    if (array && array.length > 0) {
        child = array
            .map((item) => {
                // 筛查符合条件的数据（主键 = startPid）
                if (typeof item[opt.parentKey] !== 'undefined' && item[opt.parentKey] === startPid) {
                    // 满足条件则递归
                    const nextChild = toTree(array, item[opt.primaryKey], currentDept + 1, opt);
                    // 节点信息保存
                    if (nextChild.length > 0) {
                        item['isLeaf'] = false;
                        item[opt.childKey] = nextChild;
                    } else {
                        item['isLeaf'] = true;
                    }
                    item['title'] = item[opt.titleKey];
                    item['label'] = item[opt.titleKey];
                    item['key'] = item[opt.primaryKey];
                    item['value'] = item[opt.primaryKey];
                    return item;
                }
            })
            .filter((item) => {
                return item !== undefined;
            });
    }
    return child;
};
```

<h1>查找树结构</h1>

```ts
/**
 * 查找树结构
 * @param treeList
 * @param fn 查找方法
 * @param childrenKey
 */
const findTree = (treeList: any[], fn: Fn, childrenKey = 'children') => {
        for (let i = 0; i < treeList.length; i++) {
            let item = treeList[i];
            if (fn(item, i, treeList)) {
                return item;
            }
            let children = item[childrenKey];
            if (isArray(children)) {
                let findResult = findTree(children, fn, childrenKey);
                if (findResult) {
                    return findResult;
                }
            }
        }
        return null;
    }

```

<h1>过滤对象中为空的属性</h1>

```javascript
const filterObj = (obj) => {
    if (!(typeof obj == 'object')) {
        return;
    }
    for (let key in obj) {
        if (obj.hasOwnProperty(key) && (obj[key] == null || obj[key] == undefined || obj[key] === '')) {
            delete obj[key];
        }
    }
    return obj;
}
```
