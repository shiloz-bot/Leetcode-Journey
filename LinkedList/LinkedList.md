<details open="open">
  <summary>Table of Content</summary>
  <ol>
    <li>
      <a href="#about-linkedlist">About LinkedList</a>
    </li>
    <li>
      <a href="#singly-linkedlist">Singly LinkedList</a>
      <ul>
          <li><a href="#modify">Modify</a></li>
          <li><a href="#modify">Reverse</a></li>
          <li><a href="#modify">Add</a></li>
          <li><a href="#modify">Delete</a></li>
      </ul>
    </li>
    <li>
    <a href="#doubly-linkedlist">Doubly LinkedList</a>
    </li>
  </ol>
</details>

## About LinkedList

## Singly LinkedList
### Modify
[725. Split Linked List in Parts](https://leetcode-cn.com/problems/split-linked-list-in-parts/)
* Time: O(n), Space: O(1)
* Calculate the length of list and number of nodes in a parts, and the earlier parts will equally share the remainder of the length / k
* Keep track the remainder and cut the list to parts
```java
class Solution {
    public ListNode[] splitListToParts(ListNode head, int k) {
        ListNode curr = head;
        int length = 0;
        while (curr != null) {
            length++;
            curr = curr.next;
        }
        ListNode[] res = new ListNode[k];
        int width = length / k;
        int remain = length % k;
        curr = head;
        for (int i = 0; i < k; i++) {
            if (curr == null) {
                break;
            }
            res[i] = curr;
            int currWidth = remain-- > 0? width + 1: width;
            while (currWidth > 1) {
                curr = curr.next;
                currWidth--;
            }
            ListNode prev = curr;
            curr = curr.next;
            prev.next = null;
        }
        return res;
    }
}
```
### Reverse
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
[25. Reverse Nodes in k-Group](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)
```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummy = new ListNode(0, head);
        ListNode curr = dummy;
        while (curr.next != null) {
            ListNode tail = curr.next;
            ListNode nextCurr = tail;
            for (int i = 0; i < k; i++) {
                if (tail == null) {
                    return dummy.next;
                }
                tail = tail.next;
            }
            ListNode iter = curr.next;
            ListNode prev = tail;
            while (iter != tail) {
                ListNode nextNode = iter.next;
                iter.next = prev;
                prev = iter;
                iter = nextNode;
            }
            curr.next = prev;
            curr = nextCurr;
        }
        return dummy.next;
    }
}
```
### Add
### Delete
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



## Doubly LinkedList
