<details open="open">
  <summary>Table of Content</summary>
  <ol>
    <li>
      <a href="#about-linkedlist">About LinkedList</a>
    </li>
    <li>
      <a href="#singly-linkedlist">Singly LinkedList</a>
      <ul>
          Build/Reverse/Add/Delete
      </ul>
    </li>
  </ol>
</details>

## About LinkedList

## Singly LinkedList

### Build/Reverse/Add/Delete 
[92. Reverse Linked List II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)
```java
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        ListNode dummy = new ListNode(0, head);
        ListNode curr = dummy;
        for (int i = 1; i < left; i++) {
            curr = curr.next;
        }
        ListNode prev = curr;
        curr = curr.next;
        for (int i = left; i < right; i++) {
            ListNode nextNode = curr.next;
            curr.next = curr.next.next;
            nextNode.next = prev.next;
            prev.next = nextNode;
        }
        return dummy.next;
    }
}
```

[82. Remove Duplicates from Sorted List II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)
* Time: O(n), Space: O(1)
* Use dummy variable cause head may change
* use a temp node to traverse the list, and always keep temp node points to the list that is modified already
* comparing the next two nodes and delete the duplicate nodes when necessary
```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode dummy = new ListNode(0, head);
        ListNode curr = dummy;
        while (curr.next != null && curr.next.next != null) {
            if (curr.next.val == curr.next.next.val) {
                int val = curr.next.val;
                while (curr.next != null && curr.next.val == val) {
                    curr.next = curr.next.next;
                }
            } else {
                curr = curr.next;
            }
        }
        return dummy.next;
    }
}
```
