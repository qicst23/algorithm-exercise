# Merge Two Sorted Lists

## Source

- lintcode: [(165) Merge Two Sorted Lists](http://www.lintcode.com/en/problem/merge-two-sorted-lists/)
- leetcode: [Merge Two Sorted Lists | LeetCode OJ](https://leetcode.com/problems/merge-two-sorted-lists/)

```
Merge two sorted linked lists and return it as a new list.
The new list should be made by splicing together the nodes of the first two lists.

Example
Given 1->3->8->11->15->null, 2->null , return 1->2->3->8->11->15->null
```

## 题解

此题为两个链表的合并，合并后的表头节点不一定，故应联想到使用`dummy`节点。链表节点的插入主要涉及节点`next`指针值的改变，两个链表的合并操作则涉及到两个节点的`next`值变化，若每次合并一个节点都要改变两个节点`next`的值且要对`NULL`指针做异常处理，势必会异常麻烦。嗯，第一次做这个题时我就是这么想的... 下面看看相对较好的思路。

首先`dummy`节点还是必须要用到，除了`dummy`节点外还引入一个`lastNode`节点充当下一次合并时的头节点。在`l1`或者`l2`的某一个节点为空指针`NULL`时，退出`while`循环，并将非空链表的头部链接到`lastNode->next`中。

### C++

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode *dummy = new ListNode(0);
        ListNode *lastNode = dummy;
        while ((NULL != l1) && (NULL != l2)) {
            if (l1->val < l2->val) {
                lastNode->next = l1;
                l1 = l1->next;
            } else {
                lastNode->next = l2;
                l2 = l2->next;
            }

            lastNode = lastNode->next;
        }

        // do not forget this line!
        lastNode->next =  (NULL != l1) ? l1 : l2;

        return dummy->next;
    }
};
```

### 源码分析

1. 异常处理，包含在`dummy->next`中。
2. 引入`dummy`和`lastNode`节点，此时`lastNode`指向的节点为`dummy`
3. 对非空l1,l2循环处理，将l1/l2的较小者链接到`lastNode->next`，往后递推`lastNode`
4. 最后处理l1/l2中某一链表为空退出while循环，将非空链表头链接到`lastNode->next`
5. 返回`dummy->next`，即最终的首指针

注意`lastNode`的递推并不影响`dummy->next`的值，因为`lastNode`和`dummy`是两个不同的指针变量。

> **Note** 链表的合并为常用操作，务必非常熟练，以上的模板非常精炼，有两个地方需要记牢。1. 循环结束条件中为条件与操作；2. 最后处理`lastNode->next`指针的值。

## Reference

- [Merge Two Sorted Lists | 九章算法](http://www.jiuzhang.com/solutions/merge-two-sorted-lists/)
