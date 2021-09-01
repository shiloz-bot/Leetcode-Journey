<details open="open">
  <summary>Table of Content</summary>
  <ol>
    <li>
      <a href="#tree">Tree</a>
      <ul>
          <li><a href="#construct">Construct</a></li>
          <li><a href="#path,-depth,-reverse">Path, Depth, Reverse</a></li>
      </ul>
    </li>
    <li>
      <a href="#bst">BST</a>
    </li>
    <li>
      <a href="#trie">Trie</a>
    </li>

  </ol>
</details>

## Tree
### Construct
[108. Convert Sorted Array to Binary Search Tree](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)
```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return buildBST(nums, 0, nums.length - 1);
    }

    public TreeNode buildBST(int[] nums, int left, int right) {
        if (left == right) {
            return new TreeNode(nums[left]);
        }
        int mid = left + (right - left + 1) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = buildBST(nums, left, mid - 1);
        if (mid < right) {
            root.right = buildBST(nums, mid + 1, right);
        }
        return root;
    }
}
```

[105. Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
```java
class Solution {
    Map<Integer, Integer> indices;
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        indices = new HashMap<>();
        int n = inorder.length;
        for (int i = 0; i < n; i++) {
            indices.put(inorder[i], i);
        }
        return buildHelper(preorder, inorder, 0, n - 1, 0, n - 1);
    }

    public TreeNode buildHelper(int[] pre, int[] in, int preL, int preR, int inL, int inR) {
        if (preL == preR) {
            return new TreeNode(in[inL]);
        }
        int rootVal = pre[preL];
        TreeNode root = new TreeNode(rootVal);
        int ind = indices.get(rootVal);
        int leftLen = ind - inL;
        if (ind > inL) {
           root.left = buildHelper(pre, in, preL + 1, preL + leftLen, inL, ind - 1);
        }
        if (ind < inR) {
            root.right = buildHelper(pre, in, preL + leftLen + 1, preR, ind + 1, inR);
        }
        return root;
    }
}
```

[106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
```java
class Solution {
    Map<Integer, Integer> m;
    public TreeNode buildTree(int[] inorder, int[] postorder) {
         m = new HashMap<>();
        for (int i = 0; i < inorder.length; i++) {
            m.put(inorder[i], i);
        }
        return build(inorder, postorder, 0, inorder.length - 1, 0, postorder.length - 1);
    }

    public TreeNode build(int[] inorder, int[] postorder, int iL, int iR, int pL, int pR) {
        if (iL > iR || pL > pR) {
            return null;
        }
        TreeNode root = new TreeNode(postorder[pR]);
        int midIndex = m.get(root.val);
        int numLeft = midIndex - 1 - iL;
        root.left = build(inorder, postorder, iL, iL + numLeft, pL, pL + numLeft);
        root.right = build(inorder, postorder, midIndex + 1, iR, pL + numLeft + 1, pR - 1);
        return root;
    }
}
```

[889. Construct Binary Tree from Preorder and Postorder Traversal](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)
```java
class Solution {
    public TreeNode constructFromPrePost(int[] pre, int[] post) {
        return build(pre, post, 0, pre.length - 1, 0, post.length - 1);
    }

    public TreeNode build(int[] pre, int[] post, int prL, int prR, int poL, int poR) {
        if (prL > prR || poL > poR) {
            return null;
        }
        TreeNode root = new TreeNode(pre[prL]);
        if (prL == prR) {
            return root;
        }
        int div = 0;
        for (int i = poL; i <= poR; i++) {
            if (pre[prL + 1] == post[i]) {
                div = i;
            }
        }
        root.left = build(pre, post, prL + 1, prL + 1 + div - poL, poL, div);
        root.right = build(pre, post, prL + 2 + div - poL, prR, div + 1, poR - 1);
        return root;
    }
}
```

[1008. Construct Binary Search Tree from Preorder Traversal](https://leetcode-cn.com/problems/construct-binary-search-tree-from-preorder-traversal/submissions/)
```java
class Solution {
    public TreeNode bstFromPreorder(int[] preorder) {
        return build(preorder, 0, preorder.length - 1);
    }

    public TreeNode build(int[] preorder, int left, int right) {
        if (left > right) {
            return null;
        }
        int val = preorder[left];
        TreeNode root = new TreeNode(val);
        if (left == right) {
            return root;
        }
        int leftB = left + 1;
        int rightB = right;
        while (leftB < rightB) {
            int mid = leftB + (rightB - leftB) / 2;
            if (preorder[mid] > val) {
                rightB = mid;
            } else {
                leftB = mid + 1;
            }
        }
        if (preorder[leftB] > val) {
            root.left = build(preorder, left + 1, leftB - 1);
            root.right = build(preorder, leftB, right);
        } else {
            root.left = build(preorder, left + 1, right);
        }
        return root;
    }
}
```

### Path, Depth, Reverse
[437. Path Sum III](https://leetcode-cn.com/problems/path-sum-iii/)
* Time: O(n), Space: O(n)
* Prefix sum can be used to find the paths
* When two nodes are in the same path, and the difference between their prefix sum equals to the targetSum, the path between them counts as a valid path
* Use hashMap to quick access to the number of nodes share a same prefix sum
```java
class Solution {
    Map<Integer, Integer> m;
    public int pathSum(TreeNode root, int targetSum) {
        m = new HashMap<>();
        m.put(0, 1); // when node's value equals to targetSum
        return find(root, targetSum, 0);
    }

    public int find(TreeNode root, int targetSum, int preSum) {
        if (root == null) {
            return 0;
        }
        int currSum = preSum + root.val;
        int res = 0;
        if (m.containsKey(currSum - targetSum)) {
            res = m.get(currSum - targetSum);
        }
        m.put(currSum, m.getOrDefault(currSum, 0) + 1); // add current prefix sum to the hashmap
        res += find(root.left, targetSum, currSum);
        res += find(root.right, targetSum, currSum);
        m.put(currSum, m.get(currSum) - 1); // remove the current prefix sum when this branch has finished exploring
        return res;
    }
}

```

## BST
[108. Convert Sorted Array to Binary Search Tree](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)
```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return buildBST(nums, 0, nums.length - 1);
    }

    public TreeNode buildBST(int[] nums, int left, int right) {
        if (left == right) {
            return new TreeNode(nums[left]);
        }
        int mid = left + (right - left + 1) / 2;
        TreeNode root = new TreeNode(nums[mid]);
        root.left = buildBST(nums, left, mid - 1);
        if (mid < right) {
            root.right = buildBST(nums, mid + 1, right);
        }
        return root;
    }
}
```

## Trie
[720. Longest Word in Dictionary](https://leetcode-cn.com/problems/longest-word-in-dictionary/)
```java
class Solution {
    class TrieNode {
        TrieNode[] child;
        boolean exist;
        public TrieNode() {
            child = new TrieNode[26];
            exist = false;
        }
    }

    String res;
    public String longestWord(String[] words) {
        TrieNode root = new TrieNode();
        res = "";
        for (String word: words) {
            TrieNode iter = root;
            for (int i = 0; i < word.length(); i++) {
                int idx = word.charAt(i) - 'a';
                if (iter.child[idx] == null) {
                    iter.child[idx] = new TrieNode();
                }
                iter = iter.child[idx];
            }
            iter.exist = true;
        }
        findMaxDepth(new StringBuilder(), root);
        return res;
    }

    public void findMaxDepth(StringBuilder sb, TrieNode root) {
        for (char i = 'a'; i <= 'z'; i++) {
            int idx = i - 'a';
            if (root.child[idx] != null && root.child[idx].exist) {
                sb.append(i);
                if (sb.length() > res.length()) {
                    res = sb.toString();
                }
                findMaxDepth(sb, root.child[idx]);
                sb.deleteCharAt(sb.length() - 1);
            }
        }
    }
}
```

[692. Top K Frequent Words](https://leetcode-cn.com/problems/top-k-frequent-words/)
```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        Map<String, Integer> freq = new HashMap<>();
        for (String word: words) {
            freq.put(word, freq.getOrDefault(word, 0) + 1);
        }
        PriorityQueue<String> pq = new PriorityQueue<>(new Comparator<String>() {
            public int compare(String s1, String s2) {
                if (freq.get(s1) == freq.get(s2)) {
                    return s2.compareTo(s1);
                }
                return freq.get(s1) - freq.get(s2);
            }
        });

        for (String word: freq.keySet()) {
            pq.offer(word);
            if (pq.size() > k) {
                pq.poll();
            }
        }
        List<String> res = new ArrayList<>();
        while (!pq.isEmpty()) {
            res.add(pq.poll());
        }
        Collections.reverse(res);
        return res;
    }
}
```
