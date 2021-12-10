---
layout: post
title: "[Leetcode] Add two numbers"
date: 2021-12-10
excerpt: "You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list."
tags: [leetcode,c++]
category: [Solved]
comments: false
---
# Add two numbers
## 문제 설명
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

## 제한사항
* The number of nodes in each linked list is in the range [1, 100].
* 0 <= Node.val <= 9
* It is guaranteed that the list represents a number that does not have leading zeros.

## 풀이(linked list)
1. 입력받은 두 개의 리스트를 끝까지 순회하며 같은 위치의 값을 더한다.
	* 만약, 한 쪽이 먼저 끝나면 해당 값은 0으로 더해준다.
2. 더한 결과를 저장할 연결리스트를 생성하기 위해 노드를 생성한다.	
	* 출력을 위해 시작노드를 따로 저장해뒀다.
3. 더한 값이 10 이상일 경우 올림해 다음 노드의 값에 저장하고 현재 노드는 나머지를 갖는다.
4. 두 리스트를 모두 순회하면 종료한다.

## 풀이 코드
```
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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* ans=new ListNode();
        ListNode* header=ans;
        while(true){
            int tmp1,tmp2;
            bool flag1=l1!=nullptr,flag2=l2!=nullptr;
            
            tmp1=(flag1)?l1->val:0;
            tmp2=(flag2)?l2->val:0;
            
            ans->val+=tmp1+tmp2;
            
            l1=flag1?l1->next:l1;
            l2=flag2?l2->next:l2;
            
            if(ans->val>9){
                ans->next=new ListNode(1);
                ans->val=ans->val%10;
            }
            else if(l1!=nullptr||l2!=nullptr)
                ans->next=new ListNode();
            else
                break;
            
            ans=ans->next;
        }
        return header;
    }
};

```


