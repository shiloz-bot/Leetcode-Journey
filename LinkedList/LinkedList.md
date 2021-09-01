<details open="open">
  <summary>Table of Content</summary>
  <ol>
    <li>
      <a href="#things-about-linkedlist">Things About LinkedList</a>
    </li>
    <li>
      <a href="#singly-linkedlist">Singly LinkedList</a>
      <ul>
          <li><a href="#modify">Modify</a></li>
          <li><a href="#reverse">Reverse</a></li>
          <li><a href="#add">Add</a></li>
          <li><a href="#delete">Delete</a></li>
      </ul>
    </li>
    <li>
    <a href="#doubly-linkedlist">Doubly LinkedList</a>
    </li>
  </ol>
</details>

## Things About LinkedList
* A linked list consists of nodes where each node contains a data field and a reference to the next node.:speech_balloon:
* Dummynode is a great technique to use while we are modifying the list, and the head of the list may get changed.
* While doing linked list problem, we should try to create temperary listnode to save the reference to the node we want to keep track, and simplify to minimize the temp nodes after getting the optimal solution.
* If we want to find the middle node in a list in one pass, we could create fast and slow pointers, and slow pointer points to the middle node when fast pointer reaches to the end.

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
[147. Insertion Sort List](https://leetcode-cn.com/problems/insertion-sort-list/)
```java
class Solution {
    public ListNode insertionSortList(ListNode head) {
        ListNode set = head;
        ListNode dummy = new ListNode(0, head);
        while (set.next != null) {
            ListNode iter = dummy;
            ListNode nextInsert = set.next;
            while (iter.next.val < nextInsert.val && iter != set) {
                iter = iter.next;
            }
            set.next = set.next.next;
            ListNode post = iter.next;
            iter.next = nextInsert;
            nextInsert.next = post;
            if (set.next == nextInsert) {
                set = set.next;
            }
        }
        return dummy.next;
    }
}
```
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
[2. Add Two Numbers](https://leetcode-cn.com/problems/add-two-numbers/)
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        int carry = 0;
        ListNode dummy = new ListNode();
        ListNode curr = dummy;
        while (l1 != null || l2 != null || carry != 0) {
            if (l1 != null) {
                carry += l1.val;
                l1 = l1.next;
            }
            if (l2 != null) {
                carry += l2.val;
                l2 = l2.next;
            }
            curr.next = new ListNode(carry % 10);
            carry /= 10;
            curr = curr.next;
        }
        return dummy.next;
    }
}
```
[1669. Merge In Between Linked Lists](https://leetcode-cn.com/problems/merge-in-between-linked-lists/)
```java
class Solution {
    public ListNode mergeInBetween(ListNode list1, int a, int b, ListNode list2) {
        ListNode start = list1;
        ListNode end = list1;
        ListNode tail = list2;
        while (tail.next != null) {
            tail = tail.next;
        }
        for (int i = 0; i <= b; i++) {
            if (i == a - 1) {
                start = end;
            }
            end = end.next;
        }
        start.next = list2;
        tail.next = end;
        return list1;
    }
}
```
