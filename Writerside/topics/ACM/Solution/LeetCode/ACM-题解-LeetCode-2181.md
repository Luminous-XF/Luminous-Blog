# LeetCode 2181. 合并零之间的节点

## 原题地址

[](https://leetcode.cn/problems/merge-nodes-in-between-zeros)

<br/>

## 题目类型

链表、模拟

## 代码

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode *mergeNodes(ListNode *head) {
        ListNode* pre = head;
        ListNode* now = head->next;
        while(now->next != nullptr) {
            if (now->val == 0) {
                pre->next = now;
                pre = now;
            } else {
                pre->val += now->val;
            }
            now = now->next;
        }

        pre->next = nullptr;
        
        return head;
    }
};
```

