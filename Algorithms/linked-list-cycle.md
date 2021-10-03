## 141. Linked List Cycle

### Descirption 
Given head, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Return true if there is a cycle in the linked list. Otherwise, return false.

#### Constraints
- The number of the nodes in the list is in the range [0, 10,000].
- -100,000 <= Node.val <= 100,000
- pos is -1 or a valid index in the linked-list.

### Sol 1: using hash map

```C++
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
    bool hasCycle(ListNode *head) {
        if(head==NULL)
            return false;
        unordered_map<ListNode*,int> map;
        while(head!=NULL){
            if(map.count(head)>0)
                return true;
            else
                map[head]=1;
            head=head->next;
        }
        return false;
    }
};
```
Note:
- Runtime: 20 ms
- Memory Usage: 11.1 MB


### Sol 2: using two pointer array

```C++
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
    bool hasCycle(ListNode *head) {
        if(!head)   return false;
        ListNode *slowptr = head;
        ListNode *fastptr = head;
        do{
            slowptr = slowptr->next;
            fastptr = fastptr->next;
            if(fastptr)
                fastptr = fastptr->next;
        }while(fastptr && slowptr!=fastptr);
        return fastptr ? true : false;
    }
};
```
Note:
- Runtime: 12 ms
- Memory Usage: 8.1 MB
