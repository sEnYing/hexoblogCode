---
title: js基础算法-2
date: 2023-09-27 16:05:28
description: 前端js的基础算法内容，链表、树
tags:
  - javascript
  - 算法
categories:
  - javascript
  - 算法
---

<h1>链表</h1>
<h3>链表的实现</h3>

```javascript
// 节点类
class LinkedNode {
    constructor(value) {
        this.value = value
        // 用于存储下一个节点的引用
        this.next = null
    }
}

// 链表类
class LinkedList {
    constructor() {
        this.count = 0
        this.head = null
    }

    // 添加节点 (尾）
    addAtTail(value) {
        // 创建新节点
        const node = new LinkedNode(value)
        // 检测链表是否存在数据
        if (this.count === 0) {
            this.head = node
        } else {
            // 找到链表尾部节点，将最后一个节点的 next 设置为 node
            let cur = this.head
            while (cur.next != null) {
                cur = cur.next
            }
            cur.next = node
        }
        this.count++
    }

    // 添加节点（首）
    addAtHead(value) {
        const node = new LinkedNode(value)
        if (this.count === 0) {
            this.head = node
        } else {
            // 将 node 添加到 head 的前面
            node.next = this.head
            this.head = node
        }
        this.count++
    }

    // 获取节点（根据索引）
    get(index) {
        if (this.count === 0 || index < 0 || index >= this.count) {
            return
        }
        // 迭代链表，找到对应节点
        let current = this.head
        for (let i = 0; i < index; i++) {
            current = current.next
        }
        return current
    }

    // 添加节点（根据索引）
    addAtIndex(value, index) {
        if (this.count === 0 || index >= this.count) {
            return
        }
        // 如果 index <= 0，都添加到头部即可
        if (index <= 0) {
            return this.addAtHead(value)
        }
        // 后面为正常区间处理
        const prev = this.get(index - 1)
        const next = prev.next

        const node = new LinkedNode(value)
        prev.next = node
        node.next = next

        this.count++
    }

    // 删除（根据索引）
    removeAtIndex(index) {
        if (this.count === 0 || index < 0 || index >= this.count) {
            return
        }
        if (index === 0) {
            this.head = this.head.next
        } else {
            const prev = this.get(index - 1)
            prev.next = prev.next.next
        }
        this.count--
    }
}

// 测试代码
const l = new LinkedList()
l.addAtTail('a')
l.addAtTail('b')
l.addAtTail('c')

```

<h3>反转链表</h3>

```javascript
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function (head) {
        // 声明变量记录 prev、cur
        let prev = null
        let cur = head
        // 当 cur 是节点时，进行迭代
        while (cur) {
            // 先保存当前节点的下一个节点
            const next = cur.next
            cur.next = prev
            prev = cur
            cur = next
        }
        return prev
    };
```

<h3>递归反转链表</h3>

```javascript
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function (head) {
        if (head === null || head.next === null) {
            return head
        }
        const newHead = reverseList(head.next)
        // 能够第一次执行这里的节点为 倒数第二个 节点
        head.next.next = head
        // head 的 next 需要在下一次递归执行时设置。当前设置为 null 不影响
        //   - 可以让最后一次（1）的 next 设置为 null
        head.next = null
        return newHead
    };
```

<h3>环路检测</h3>

```javascript
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var detectCycle = function (head) {
        if (head === null || head.next === null) {
            return null
        }
        // 声明快慢指针
        let slow = head
        let fast = head

        while (fast !== null) {
            // 慢每次指针移动一位
            slow = slow.next
            // 如果满足条件，说明 fast 为尾部结点，不存在环
            if (fast.next === null) {
                return null
            }
            // 快指针每次移动两位
            fast = fast.next.next

            // 检测是否有环
            if (fast === slow) {
                // 找到环的起点位置
                let ptr = head
                while (ptr !== slow) {
                    ptr = ptr.next
                    slow = slow.next
                }
                // ptr 和 slow 的交点就是环的起始节点
                return ptr
            }
        }
        // while 结束，说明 fast 为 null，说明链表没有环
        return null
    };
```

<h1>树</h1>
<h3>二叉树的前序遍历</h3>

```javascript
/**
 * @param {TreeNode} root
 * @return {number[]}
 */
const preorderTraversal = function (root) {
        const res = []
        const stk = []
        while (root || stk.length) {
            while (root) {
                // 右子结点入栈
                stk.push(root.right)
                // 记录根节点
                res.push(root.val)
                // 下一步处理左子节点
                root = root.left
            }
            // 左子树处理完毕，将 stk 出栈，处理右子树
            root = stk.pop()
        }
        return res
    }
```

<h3>二叉树的最大深度</h3>

```javascript
/**
 * @param {TreeNode} root
 * @return {number}
 */
var maxDepth = function (root) {
        if (!root) {
            return 0
        }
        return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1
    };
```

<h3>二叉树的层序遍历</h3>

```javascript
/**
 * @param {TreeNode} root
 * @return {number[][]}
 */
var levelOrder = function (root) {
        const res = []
        if (!root) {
            return res
        }
        // 声明队列用于存储后续数据
        const q = []
        q.push(root)

        // 遍历队列
        while (q.length !== 0) {
            // 针对本轮操作，创建一个新的二维数组
            res.push([])
            let len = q.length
            for (let i = 0; i < len; i++) {
                // 将本次操作的结点出队
                const node = q.shift()
                res[res.length - 1].push(node.val)
                // 检测是否存在左右子结点，如果有，入队即可
                if (node.left) {
                    q.push(node.left)
                }
                if (node.right) {
                    q.push(node.right)
                }
            }
        }
        return res
    };
```

<h3>验证二叉搜索树</h3>

```javascript
var isValidBST = function (root) {
    let stk = []
    // 用于记录上一次取得的节点值，BST 中这个值应小于当前节点
    // 设置默认值为 -Infinity 避免对比较结果产生干扰
    let oldNode = -Infinity

    while (root || stk.length) {
        while (root) {
            stk.push(root)
            root = root.left
        }
        root = stk.pop()
        // 如果任意节点比上个节点值小，说明二叉树不是 BST
        if (root.val <= oldNode) {
            return false
        }
        // 通过比较，记录当前节点值
        oldNode = root.val
        root = root.right
    }
    return true
};
```
