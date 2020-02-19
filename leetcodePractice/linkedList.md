# 链表

链表节点的结构如下

```js
function ListNode(val) {
    this.val = val;
    this.next = null;
}
```

1. 删除一个节点。请编写一个函数，使其可以删除某个链表中给定的（非末尾）节点，你将只被给定要求被删除的节点。
   说明:

    - 链表至少包含两个节点。
    - 链表中所有节点的值都是唯一的。
    - 给定的节点为非末尾节点并且一定是链表中的一个有效节点。
    - 不要从你的函数中返回任何结果。
  
  ```js
/**
 * @param {ListNode} node
 * @return {void} Do not return anything, modify node in-place instead.
 */
var deleteNode = function(node) {};
  ```

<details>
<summary>答案</summary>

```js
var deleteNode = function(node) {
    node.val = node.next.val;
    node.next = node.next.next;
};
```
</details>
<br/>

2. 删除链表的倒数第N个节点。给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。
   说明:给定的 n 保证是有效的。
   示例：
   > 给定一个链表: 1->2->3->4->5, 和 n = 2.
     <br/>
     当删除了倒数第二个节点后，链表变为 1->2->3->5.

   进阶：你能尝试使用一趟扫描实现吗？
  
  ```js
/**
 * @param {ListNode} head
 * @param {number} n
 * @return {ListNode}
 */
var removeNthFromEnd = function(head, n) {};
  ```

<details>
<summary>答案1:扫描两次</summary>

```js
var removeNthFromEnd = function(head, n) {
   var len = 1, current = head;
    while (current.next !== null) {
      current = current.next;
      len++;
    }
    if (len !== n) {
      current = head;
      for (var i = 1; i < len - n; i++) {
        current = current.next;
      }
      current.next = current.next != null ? current.next.next : null;
      return head;
    }
    else {
      current = head.next;
      return current;
    }
};
```
</details>

<details>
<summary>答案2:扫描一次</summary>

```js
var removeNthFromEnd = function(head, n) {
    let current = head;
    let len = 0;
    let arr = [];
    while (current) {
      arr.push(current);
      current = current.next;
      len++;
    }
    if (len === n) {
      return head.next;
    }
    const item = arr[len - n - 1];
    if (item == null || item.next == null) return null;
    item.next = item.next.next;
    return head;
};
```
</details>
<br/>

3. 反转链表。反转一个单链表。
   示例：
   > 输入: 1->2->3->4->5->NULL
     <br/>
     输出: 5->4->3->2->1->NULL

   进阶:
   你可以迭代或递归地反转链表。你能否用两种方法解决这道题？
  
  ```js
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function(head) {};
  ```

<details>
<summary>答案1：迭代</summary>

```js
var reverseList = function(head) {
    var arr = [], current = head;
    if (head == null) return null;
    while(current) {
      arr.push(current);
      current = current.next;
    }
    var rst, tmp;
    for(var i = arr.length - 1; i >= 0; i--) {
      if (rst == null) {
        rst = new ListNode(arr[i].val);
        tmp = rst;
      }
      else {
        tmp.next = new ListNode(arr[i].val);
        tmp = tmp.next;
      }
    }
    return rst;
};
```
</details>
<details>
<summary>答案2：递归</summary>

```js
var reverseList = function(head) {
  if (!head) return null;
  function reverse(point) {
    if (point.next == null) {
      return {result: point, current: point};
    }
    const {result, current} = reverse(point.next);
    current.next = point;
    point.next = null;
    return {result, current: current.next};
  }
  return reverse(head).result;
};
```
</details>
<br/>

4. 合并两个有序链表。将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。
   示例：
   > 输入：1->2->4, 1->3->4
     <br/>
     输出：1->1->2->3->4->4
  
  ```js
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function(l1, l2) {};
  ```

<details>
<summary>答案</summary>

```js
var mergeTwoLists = function(l1, l2) {
    var rst, l1Current = l1, l2Current = l2, point;
    if (l1Current == null) return l2Current;
    if (l2Current == null) return l1Current;

    if (l2Current.val <= l1Current.val) {
      rst = new ListNode(l2Current.val);
      l2Current = l2Current.next;
      point = rst;
    }
    else if (l1Current.val < l2Current.val) {
      rst = new ListNode(l1Current.val);
      l1Current = l1Current.next;
      point = rst;
    }
    while (l1Current || l2Current) {
      if (l2Current != null && (l1Current == null || l1Current != null && l2Current.val <= l1Current.val)) {
        point.next = new ListNode(l2Current.val);
        l2Current = l2Current.next;
        point = point.next;
      }
      else if (l1Current != null && (l2Current == null || l2Current != null && l1Current.val < l2Current.val)) {
        point.next = new ListNode(l1Current.val);
        l1Current = l1Current.next;
        point = point.next;
      }
    }
    return rst;
};
```
</details>
<br/>

4. 回文链表.请判断一个链表是否为回文链表。
   示例：
   > 输入: 1->2
     <br/>
     输出: false
     <br/>
     输入: 1->2->2->1
     <br/>
     输出: true

   进阶：
   你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？
  
  ```js
/**
 * @param {ListNode} head
 * @return {boolean}
 */
var isPalindrome = function(head) {};
  ```

<details>
<summary>答案</summary>

```js
var isPalindrome = function(head) {
  const arr = [];
  let current = head;
  while(current) {
    arr.push(current.val);
    current = current.next;
  }
  return String(arr) === String(arr.reverse());
};
```
</details>
<br/>

5. 环形链表。给定一个链表，判断链表中是否有环。为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。
   示例：
   > 输入：head = [3,2,0,-4], pos = 1
     <br/>
     输出：true
     <br/>
     解释：链表中有一个环，其尾部连接到第二个节点。

   进阶：
   你能用 O(1)（即，常量）内存解决此问题吗？
  
  ```js
/**
 * @param {ListNode} head
 * @return {boolean}
 */
var hasCycle = function(head) {};
  ```

<details>
<summary>答案</summary>

```js
var hasCycle = function(head) {
    let point1 = head;
    let point2 = head && head.next;
    while (point1 != null && point2 != null) {
        point1 = point1.next;
        point2 = point2.next && point2.next.next;

        if (point1 === point2) return true;
    } 
    return false;
};
```
</details>
<br/>