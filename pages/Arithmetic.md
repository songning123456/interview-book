#### 动态规划
* 状态转移方程步骤


```
明确**base case** -> 明确**状态** -> 明确**选择** -> 定义**dp数组/函数的定义**
```


* 基本模板


```
// 初始化base
dp[0][0][...] = base
// 进行状态转移
for 状态1 in 状态1的所有取值:
    for 状态2 in 状态2的所有取值:
        for ...
            dp[状态1][状态2][...] = 求最值(选择1, 选择2...)
```


#### 动态规划-斐波那契数列
![fib](/images/Arithmetic/fib.PNG)


* 解法一


```
public int fib(int N) {
    if (N == 0) {
        return 0;
    }
    if (N == 1) {
        return 1;
    }
    int[] dp = new int[N + 1];
    dp[0] = 0;
    dp[1] = 1;
    for (int i = 2; i <= N; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[N];
}
```


* 解法二


```
public int fib(int N) {
    if (N == 0) {
        return 0;
    }
    if (N == 1) {
        return 1;
    }
    int prev = 0, cur = 1;
    for (int i = 2; i <= N; i++) {
        int sum = prev + cur;
        prev = cur;
        cur = sum;
    }
    return cur;
}
```


#### 动态规划-凑零钱问题
![coinChange](/images/Arithmetic/coinChange.PNG)


* 解法一


```
public int coinChange(int[] coins, int amount) {
    return dp(coins, amount);
}

// 备忘录，避免重复计算
private Map<Integer, Integer> coinMap = new HashMap<>();

private int dp(int[] arr, int n) {
    // base case
    if (n == 0) {
        return 0;
    }
    if (n < 0) {
        return -1;
    }
    if (coinMap.containsKey(n)) {
        return coinMap.get(n);
    }
    // 求最小值，所以初始化正无穷
    int res = Integer.MAX_VALUE;
    for (int item : arr) {
        int subProblem = dp(arr, n - item);
        // 如果子问题无解，则直接跳过
        if (subProblem != -1) {
            res = Math.min(res, 1 + subProblem);
        }
    }
    if (res == Integer.MAX_VALUE) {
        coinMap.put(n, -1);
        return -1;
    } else {
        coinMap.put(n, res);
        return res;
    }
}
```


* 解法二


```
public int coinChange(int[] coins, int amount) {
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, amount + 1);
    dp[0] = 0;
    for (int i = 0; i < dp.length; i++) {
        for (int coin : coins) {
            if (i - coin >= 0) {
                dp[i] = Math.min(dp[i], 1 + dp[i - coin]);
            }
        }
    }
    if (dp[amount] == amount + 1) {
        return -1;
    } else {
        return dp[amount];
    }
}
```


#### 动态规划-最长递增子序列
![lengthOfLIS](/images/Arithmetic/lengthOfLIS.PNG)


* 解法一


```
public int lengthOfLIS(int[] nums) {
    int[] dp = new int[nums.length];
    Arrays.fill(dp, 1);
    for (int i = 0; i < nums.length; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
    }
    int res = 0;
    for (int value : dp) {
        res = Math.max(res, value);
    }
    return res;
}
```


#### 动态规划-最大子序和
![maxSubArray](/images/Arithmetic/maxSubArray.PNG)


* 解法一


```
public int maxSubArray(int[] nums) {
    int n = nums.length;
    if (n == 0) {
        return 0;
    }
    int[] dp = new int[n];
    dp[0] = nums[0];
    for (int i = 1; i < n; i++) {
        dp[i] = Math.max(nums[i], nums[i] + dp[i - 1]);
    }
    int res = Integer.MIN_VALUE;
    for (int i = 0; i < n; i++) {
        res = Math.max(res, dp[i]);
    }
    return res;
}
```


* 解法二


```
public int maxSubArray(int[] nums) {
    int n = nums.length;
    if (n == 0) {
        return 0;
    }
    int dp_0 = nums[0];
    int dp_1, res = dp_0;
    for (int i = 1; i < n; i++) {
        dp_1 = Math.max(nums[i], nums[i] + dp_0);
        dp_0 = dp_1;
        res = Math.max(res, dp_1);
    }
    return res;
}
```


#### 动态规划-编辑距离
![minDistance](/images/Arithmetic/minDistance.PNG)


* 解法一


```
public int minDistance(String word1, String word2) {
    return mdDpFunc(word1, word2, word1.length() - 1, word2.length() - 1);
}

private Map<String, Integer> mdMap = new HashMap<>(2);
private int mdDpFunc(String word1, String word2, int i, int j) {
    if (i == -1) {
        return j + 1;
    }
    if (j == -1) {
        return i + 1;
    }
    if (mdMap.containsKey(i + "-" + j)) {
        return mdMap.get(i + "-" + j);
    }
    if (word1.charAt(i) == word2.charAt(j)) {
        int res = mdDpFunc(word1, word2, i - 1, j - 1);
        mdMap.put(i + "-" + j, res);
        return res;
    } else {
        int res = Math.min(mdDpFunc(word1, word2, i, j - 1) + 1, Math.min(mdDpFunc(word1, word2, i - 1, j) + 1, mdDpFunc(word1, word2, i - 1, j - 1) + 1));
        mdMap.put(i + "-" + j, res);
        return res;
    }
}
```


* 解法二


```
public int minDistance(String word1, String word2) {
    int m = word1.length(), n = word2.length();
    int[][] dp = new int[m + 1][n + 1];
    for (int i = 1; i <= m; i++) {
        dp[i][0] = i;
    }
    for (int j = 1; j <= n; j++) {
        dp[0][j] = j;
    }
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = Math.min(dp[i - 1][j] + 1, Math.min(dp[i][j - 1] + 1, dp[i - 1][j - 1] + 1));
            }
        }
    }
    return dp[m][n];
}
```


#### 动态规划-鸡蛋掉落
![superEggDrop](/images/Arithmetic/superEggDrop.PNG)


* 解法一


```
public int superEggDrop(int K, int N) {
    int[][] dp = new int[N + 1][K + 1];
    for (int i = 0; i < N + 1; i++) {
        for (int j = 0; j < K + 1; j++) {
            dp[i][j] = 0;
        }
    }
    for (int j = 1; j <= K; j++) {
        dp[1][j] = 1;
    }
    for (int i = 1; i <= N; i++) {
        dp[i][1] = i;
    }
    for (int i = 2; i <= N; i++) {
        for (int j = 2; j <= K; j++) {
            dp[i][j] = binaryValley(i, j, dp);
        }
    }
    return dp[N][K];
}

private int binaryValley(int floors, int eggs, int[][] dp) {
    int left = 1;
    int right = floors;
    while (left < right) {
        int mid = left + (right - left) / 2;
        int broken = dp[mid - 1][eggs - 1];
        int notBroken = dp[floors - mid][eggs];
        if (notBroken > broken) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    return Math.max(dp[right - 1][eggs - 1], dp[floors - right][eggs]) + 1;
}
```


#### 动态规划-最长公共子序列
![longestCommonSubsequence](/images/Arithmetic/longestCommonSubsequence.PNG)


* 解法一


```
public int longestCommonSubsequence(String text1, String text2) {
    int m = text1.length(), n = text2.length();
    // 构建 DP table 和 base case
    // dp[i][j] 表示： 字符串 str1[0:i] 和字符串 str2[0:j] 的最大公共子序列
    int[][] dp = new int[m + 1][n + 1];
    // 进行状态转移
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            // 若两个字符相等，必然可以构成子问题的最优解
            if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                // 这个字符存在于lcs之中
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                // 此时text1[i]!= text2[j]则表示至少有一个不在lcs中(要么text1[i]不在，要 text2[j]不在，或者都不在)
                // 所以当前结果就相当于之前结果的中最大的那一个
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    return dp[m][n];
}
```


#### LRU缓存机制
![LRU](/images/Arithmetic/LRU.PNG)


* 解法


```
public class LRUCache {

    class DLinkedNode {
        int key;
        int value;
        DLinkedNode prev;
        DLinkedNode next;

        public DLinkedNode() {
        }

        public DLinkedNode(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    private Map<Integer, DLinkedNode> cache = new HashMap<>();
    private int size;
    private int capacity;
    private DLinkedNode head, tail;

    public LRUCache(int capacity) {
        this.size = 0;
        this.capacity = capacity;
        head = new DLinkedNode();
        tail = new DLinkedNode();
        head.next = tail;
        tail.prev = head;
    }

    private void addToHead(DLinkedNode node) {
        node.prev = head;
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
    }

    private void removeNode(DLinkedNode node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void modeToHead(DLinkedNode node) {
        removeNode(node);
        addToHead(node);
    }

    private DLinkedNode removeTail() {
        DLinkedNode node = tail.prev;
        removeNode(node);
        return node;
    }


    public int get(int key) {
        DLinkedNode node = cache.get(key);
        if (node == null) {
            return -1;
        }
        modeToHead(node);
        return node.value;
    }

    public void put(int key, int value) {
        DLinkedNode node = cache.get(key);
        if (node == null) {
            DLinkedNode newNode = new DLinkedNode(key, value);
            cache.put(key, newNode);
            addToHead(newNode);
            ++size;
            if (size > capacity) {
                DLinkedNode tail = removeTail();
                cache.remove(tail.key);
                --size;
            }
        } else {
            node.value = value;
            modeToHead(node);
        }
    }
}
```


#### 二叉搜索树


* 定义


```
二叉搜索树(Binary Search Tree，简称BST)是一种很常用的二叉树。它定义
一个二叉树中，任意节点的值要大于等于左子树所有节点的值，且要小于等于右
边子树的所有节点的值。
```

* 基本模板


```
void traverse(TreeNode root) {
    // root 需要做什么？在这做。
    // 其他的不用 root 操心，抛给框架
    traverse(root.left);
    traverse(root.right);
}
```


* TreeNode类


```
public class TreeNode {
    public int val;
    public TreeNode left;
    public TreeNode right;

    public TreeNode() {
    }

    public TreeNode(int x) {
        val = x;
    }

    public TreeNode(int val, TreeNode left, TreeNode right) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```


#### 二叉搜索树-验证
![isValidBST](/images/Arithmetic/isValidBST.PNG)


* 解法


```
public boolean isValidBST(TreeNode root) {
    return isValidBST(root, null, null);
}

Boolean isValidBST(TreeNode root, TreeNode min, TreeNode max) {
    if (root == null) {
        return true;
    }
    if (min != null && root.val <= min.val) {
        return false;
    }
    if (max != null && root.val >= max.val) {
        return false;
    }
    return isValidBST(root.left, min, root) && isValidBST(root.right, root, max);
}
```


#### 二叉搜索树-相同的树
![isSameTree](/images/Arithmetic/isSameTree.PNG)


* 解法


```
public boolean isSameTree(TreeNode p, TreeNode q) {
    if (p == null && q == null) {
        return true;
    }
    if (p == null || q == null) {
        return false;
    }
    if (p.val != q.val) {
        return false;
    }

    return isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
}
```


#### 二叉搜索树-搜索
![searchBST](/images/Arithmetic/searchBST.PNG)


* 解法


```
public TreeNode searchBST(TreeNode root, int val) {
    if (root == null) {
        return null;
    }
    if (root.val == val) {
        return root;
    } else if (root.val < val) {
        root = searchBST(root.right, val);
    } else if (root.val > val) {
        root = searchBST(root.left, val);
    }
    return root;
}
```


#### 二叉搜索树-删除
![deleteNode](/images/Arithmetic/deleteNode.PNG)


* 解法


```
public TreeNode deleteNode(TreeNode root, int key) {
    if (root == null) {
        return null;
    }
    if (root.val == key) {
        if (root.left == null) {
            return root.right;
        }
        if (root.right == null) {
            return root.left;
        }
        TreeNode minNode = getMinNode(root.right);
        root.val = minNode.val;
        root.right = deleteNode(root.right, minNode.val);
    } else if (root.val > key) {
        root.left = deleteNode(root.left, key);
    } else if (root.val < key) {
        root.right = deleteNode(root.right, key);
    }
    return root;
}

private TreeNode getMinNode(TreeNode node) {
    while (node.left != null) {
        node = node.left;
    }
    return node;
}
```


#### 二叉搜索树-插入
![insertIntoBST](/images/Arithmetic/insertIntoBST.PNG)


* 解法


```
public TreeNode insertIntoBST(TreeNode root, int val) {
    if (root == null) {
        return new TreeNode(val);
    }
    if (root.val < val) {
        root.right = insertIntoBST(root.right, val);
    }
    if (root.val > val) {
        root.left = insertIntoBST(root.left, val);
    }
    return root;
}
```


#### 单调栈


* 基本模板


```
public int[] nextGreaterElement(int[] nums) {
    int[] ans = new int[nums.length];
    Stack<Integer> s = new Stack<>();
    for (int i = nums.length - 1; i >= 0; i--) {
        while (!s.isEmpty() && s.peek() <= nums[i]) {
            s.pop();
        }
        ans[i] = s.empty() ? -1 : s.peek();
        s.push(nums[i]);
    }
    return ans;
}
```


#### 单调栈-下一个更大元素I
![nextGreaterElementI](/images/Arithmetic/nextGreaterElementI.PNG)


* 解法


```
public int[] nextGreaterElement(int[] nums1, int[] nums2) {
    int[] convert = this.nextGreaterElement(nums2);
    Map<Integer, Integer> convertMap = new HashMap<>(2);
    for (int i = 0; i < nums2.length; i++) {
        convertMap.put(nums2[i], convert[i]);
    }
    int[] res = new int[nums1.length];
    for (int i = 0; i < nums1.length; i++) {
        res[i] = convertMap.get(nums1[i]);
    }
    return res;
}
```


#### 单调栈-下一个更大元素II
![nextGreaterElements2](/images/Arithmetic/nextGreaterElements2.PNG)


* 解法


```
public int[] nextGreaterElement(int[] nums) {
    int n = nums.length;
    int[] ans = new int[n];
    Stack<Integer> s = new Stack<>();
    for (int i = 2 * n - 1; i >= 0; i--) {
        while (!s.isEmpty() && s.peek() <= nums[i % n]) {
            s.pop();
        }
        ans[i % n] = s.empty() ? -1 : s.peek();
        s.push(nums[i % n]);
    }
    return ans;
}
```


#### 单调队列-滑动窗口最大值
![maxSlidingWindow](/images/Arithmetic/maxSlidingWindow.PNG)


* 解法


```
public class Solution {

    static class MonotonicQueue {

        private Deque<Integer> data;

        MonotonicQueue() {
            data = new ArrayDeque<>();
        }

        public void push(int n) {
            while (!data.isEmpty() && data.getLast() < n) {
                data.removeLast();
            }
            data.addLast(n);
        }

        public int max() {
            return data.getFirst();
        }

        public void pop(int n) {
            if (!data.isEmpty() && data.getFirst() == n) {
                data.removeFirst();
            }
        }
    }

    public int[] maxSlidingWindow(int[] nums, int k) {
        MonotonicQueue window = new MonotonicQueue();
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (i < k - 1) {
                window.push(nums[i]);
            } else {
                window.push(nums[i]);
                res.add(window.max());
                window.pop(nums[i - k + 1]);
            }
        }
        return res.stream().mapToInt(Integer::valueOf).toArray();
    }
}
```


#### 滑动窗口


* 基本模板


```
public void slidingWindow(String s, String t) {
    Map<Character, Integer> window = new HashMap<>(2);
    Map<Character, Integer> needs = new HashMap<>(2);
    for (int i = 0; i < t.length(); i++) {
        Character character = t.charAt(i);
        needs.put(character, needs.getOrDefault(character, 0) + 1);
    }
    int left = 0, right = 0;
    int valid = 0;
    while(right < s.size()) {
        // c是将移入窗口的字符
        Character c = s.charAt(right);
        // 右移窗口
        right++;
        // 进行窗口内数据的一系列更新
        // ...

        // 判断左侧窗口是否要收缩
        while(window needs shrink) {
            // d是将移除窗口的字符
            Character d = s.charAt(left);
            // 左移窗口
            left++;
            // 左移窗口内数据的一系列更新
            // ...
        }
    }
}
```


#### 滑动窗口-排列
![checkInclusion](/images/Arithmetic/checkInclusion.PNG)


* 解法


```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        List<Integer> res = new ArrayList<>();
        int left = 0, right = 0;
        Map<Character, Integer> needs = new HashMap<>(2);
        Map<Character, Integer> window = new HashMap<>(2);
        for (int i = 0; i < s1.length(); i++) {
            Character character = s1.charAt(i);
            needs.put(character, needs.getOrDefault(character, 0) + 1);
        }
        int match = 0;
        while (right < s2.length()) {
            Character c1 = s2.charAt(right);
            if (needs.containsKey(c1)) {
                window.put(c1, window.getOrDefault(c1, 0) + 1);
                if (window.get(c1).equals(needs.get(c1))) {
                    match++;
                }
            }
            right++;
            while (match == needs.size()) {
                if (right - left == s1.length()) {
                    res.add(left);
                }
                char c2 = s2.charAt(left);
                if (needs.containsKey(c2)) {
                    window.put(c2, window.get(c2) - 1);
                    if (window.get(c2) < needs.get(c2)) {
                        match--;
                    }
                }
                left++;
            }
        }
        return res.size() != 0;
    }
}
```


#### 滑动窗口-最小覆盖子串
![minWindow](/images/Arithmetic/minWindow.PNG)


* 解法


```Java
class Solution {
    public String minWindow(String s, String t) {
        int start = 0, minLen = Integer.MAX_VALUE;
        int left = 0, right = 0;
        Map<Character, Integer> window = new HashMap<>(2);
        Map<Character, Integer> needs = new HashMap<>(2);
        for (int i = 0; i < t.length(); i++) {
            Character character = t.charAt(i);
            needs.put(character, needs.getOrDefault(character, 0) + 1);
        }
        int match = 0;
        while (right < s.length()) {
            Character c1 = s.charAt(right);
            if (needs.containsKey(c1)) {
                window.put(c1, window.getOrDefault(c1, 0) + 1);
                if (window.get(c1).equals(needs.get(c1))) {
                    match++;
                }
            }
            right++;
            while (match == needs.size()) {
                if (right - left < minLen) {
                    start = left;
                    minLen = right - left;
                }
                char c2 = s.charAt(left);
                if (needs.containsKey(c2)) {
                    window.put(c2, window.get(c2) - 1);
                    if (window.get(c2) < needs.get(c2)) {
                        match--;
                    }
                }
                left++;
            }
        }
        return minLen == Integer.MAX_VALUE ? "" : s.substring(start, start + minLen);
    }
}
```


#### 滑动窗口-异位词
![findAnagrams](/images/Arithmetic/findAnagrams.PNG)


* 解法


```java
class Solution {
    public List<Integer> findAnagrams(String s, String t) {
        List<Integer> res = new ArrayList<>();
        int left = 0, right = 0;
        Map<Character, Integer> needs = new HashMap<>(2);
        Map<Character, Integer> window = new HashMap<>(2);
        for (int i = 0; i < t.length(); i++) {
            Character character = t.charAt(i);
            needs.put(character, needs.getOrDefault(character, 0) + 1);
        }
        int match = 0;
        while (right < s.length()) {
            Character c1 = s.charAt(right);
            if (needs.containsKey(c1)) {
                window.put(c1, window.getOrDefault(c1, 0) + 1);
                if (window.get(c1).equals(needs.get(c1))) {
                    match++;
                }
            }
            right++;
            while (match == needs.size()) {
                if (right - left == t.length()) {
                    res.add(left);
                }
                char c2 = s.charAt(left);
                if (needs.containsKey(c2)) {
                    window.put(c2, window.get(c2) - 1);
                    if (window.get(c2) < needs.get(c2)) {
                        match--;
                    }
                }
                left++;
            }
        }
        return res;
    }
}
```


#### 滑动窗口-无重复最长子串
![lengthOfLongestSubstring](/images/Arithmetic/lengthOfLongestSubstring.PNG)


* 解法


```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int left = 0, right = 0;
        Map<Character, Integer> window = new HashMap<>(2);
        int res = 0;
        while (right < s.length()) {
            Character c1 = s.charAt(right);
            window.put(c1, window.getOrDefault(c1, 0) + 1);
            right++;
            while (window.get(c1) > 1) {
                Character c2 = s.charAt(left);
                window.put(c2, window.get(c2) - 1);
                left++;
            }
            res = Math.max(res, right - left);
        }
        return res;
    }
}
```


#### 双指针-环形链表
![hasCycle](/images/Arithmetic/hasCycle.PNG)


* 解法


```java
public class Solution {
    static class ListNode {
        int val;
        ListNode next;

        ListNode(int x) {
            val = x;
            next = null;
        }
    }

    public boolean hasCycle(ListNode head) {
        ListNode fast, slow;
        fast = slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;

            if (fast == slow) {
                return true;
            }
        }
        return false;
    }
}
```


#### 双指针-环形链表II
![detectCycle](/images/Arithmetic/detectCycle.PNG)


* 解法


```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (!this.hasCycle(head)) {
            return null;
        }
        ListNode fast, slow;
        fast = slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) {
                break;
            }
        }
        slow = head;
        while (slow != fast) {
            fast = fast.next;
            slow = slow.next;
        }
        return slow;
    }

    private boolean hasCycle(ListNode head) {
        ListNode fast, slow;
        fast = slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;

            if (fast == slow) {
                return true;
            }
        }
        return false;
    }
}
```


#### 区间调度-交集
![intervalIntersection](/images/Arithmetic/intervalIntersection.PNG)


* 解法


```java
class Solution {
    public int[][] intervalIntersection(int[][] A, int[][] B) {
        int i = 0, j = 0;
        List<int[]> res = new ArrayList<>();
        while (i < A.length && j < B.length) {
            int a1 = A[i][0];
            int a2 = A[i][1];
            int b1 = B[j][0];
            int b2 = B[j][1];
            if (b2 >= a1 && a2 >= b1) {
                int[] temp = new int[2];
                temp[0] = Math.max(a1, b1);
                temp[1] = Math.min(a2, b2);
                res.add(temp);
            }
            if (b2 < a2) {
                j += 1;
            } else {
                i += 1;
            }
        }
        return res.toArray(new int[0][]);
    }
}
```


#### 区间调度-并集
![merge](/images/Arithmetic/merge.PNG)


* 解法


```java
class Solution {
    public int[][] merge(int[][] intervals) {
        if (intervals.length == 0) {
            return new int[0][];
        }
        Arrays.sort(intervals, (o1, o2) -> {
            if (o1[0] == o2[0]) {
                return o1[1] - o2[1];
            } else {
                return o1[0] - o2[0];
            }
        });
        List<int[]> res = new ArrayList<>();
        int[] cur, last;
        res.add(intervals[0]);
        for (int i = 1; i < intervals.length; i++) {
            cur = intervals[i];
            last = res.get(res.size() - 1);
            if (cur[0] <= last[1]) {
                last[1] = Math.max(last[1], cur[1]);
            } else {
                res.add(cur);
            }
        }
        return res.toArray(new int[0][]);
    }
}
```


#### 回溯法


* 基本模板


```
List result = [];
public void backtrack(路径, 选择列表) {
    if (满足结束条件) {
        result.add(路径);
        return;
    }
    for (选择 in 选择列表) {
        做选择;
        backtrack(路径, 选择列表);
        撤销选择;
    }
}
```


#### 回溯法-全排列
![backTrack](/images/Arithmetic/backTrack.PNG)


* 解法


```java
class Solution {
    List<List<Integer>> result = new ArrayList<>();

    public List<List<Integer>> permute(int[] nums) {
        LinkedList<Integer> track = new LinkedList<>();
        backTrack(nums, track);
        return result;
    }

    private void backTrack(int[] nums, LinkedList<Integer> track) {

        if (nums.length == track.size()) {
            result.add(new LinkedList<>(track));
            return;
        }
        for (int num : nums) {
            if (track.contains(num)) {
                continue;
            }
            track.add(num);
            backTrack(nums, track);
            track.removeLast();
        }
    }
}
```


#### 回溯法-全排列II
![permuteUnique](/images/Arithmetic/permuteUnique.PNG)


* 解法


```java
// https://leetcode-cn.com/problems/permutations-ii/
```


#### 回溯法-子集
![subsets](/images/Arithmetic/subsets.PNG)


* 解法


```java
class Solution {
    private List<List<Integer>> list = new ArrayList<>();

    public List<List<Integer>> subsets(int[] nums) {
        LinkedList<Integer> track = new LinkedList<>();
        backtrack(0, nums, track);
        return list;
    }

    private void backtrack(int i, int[] nums, LinkedList<Integer> track) {
        list.add(new ArrayList<>(track));
        for (int j = i; j < nums.length; j++) {
            track.add(nums[j]);
            backtrack(j + 1, nums, track);
            track.removeLast();
        }
    }
}
```


#### 回溯法-子集II
![subsetsWithDup](/images/Arithmetic/subsetsWithDup.PNG)


* 解法


```java
// https://leetcode-cn.com/problems/subsets-ii/
```


#### 回溯法-组合
![combine](/images/Arithmetic/combine.PNG)


* 解法


```java
class Solution {
    private List<List<Integer>> combineList = new ArrayList<>();

    public List<List<Integer>> combine(int n, int k) {
        backtrack(1, n, k, new LinkedList<>());
        return combineList;
    }

    private void backtrack(int i, int n, int k, LinkedList<Integer> track) {
        if (track.size() == k) {
            combineList.add(new LinkedList<>(track));
            return;
        }
        for (int j = i; j <= n; j++) {
            track.add(j);
            backtrack(j + 1, n, k, track);
            track.removeLast();
        }
    }
}
```


#### 回溯法-组合II
![combinationSum2](/images/Arithmetic/combinationSum2.PNG)


* 解法


```java
// https://leetcode-cn.com/problems/combination-sum-ii/
```


#### 回溯法-N皇后
![solveNQueens](/images/Arithmetic/solveNQueens.PNG)


* 解法


```java
class Solution {
   private List<List<String>> result = new ArrayList<>();

    public List<List<String>> solveNQueens(int n) {
        List<String> border = new LinkedList<>();
        for (int i = 0; i < n; i++) {
            StringBuilder stringBuilder = new StringBuilder();
            for (int j = 0; j < n; j++) {
                stringBuilder.append(".");
            }
            border.add(stringBuilder.toString());
        }
        backtrack(border, 0);
        return result;
    }

    private void backtrack(List<String> border, int row) {
        if (row == border.size()) {
            List<String> temp = new ArrayList<>(border);
            result.add(temp);
            return;
        }
        int n = border.get(row).length();
        for (int col = 0; col < n; col++) {
            if (!isValid(border, row, col)) {
                continue;
            }
            this.replace(border, row, col, 'Q');
            backtrack(border, row + 1);
            this.replace(border, row, col, '.');
        }

    }

    private boolean isValid(List<String> border, int row, int col) {
        int n = border.size();
        for (int i = 0; i < n; i++) {
            if (border.get(i).charAt(col) == 'Q') {
                return false;
            }
        }
        for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (border.get(i).charAt(j) == 'Q') {
                return false;
            }
        }
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (border.get(i).charAt(j) == 'Q') {
                return false;
            }
        }
        return true;
    }

    private void replace(List<String> params, int row, int col, Character newVal) {
        String oldStr = params.get(row);
        StringBuilder sb = new StringBuilder(oldStr);
        sb.setCharAt(col, newVal);
        params.set(row, sb.toString());
    }
}
```


#### 字符串相乘
![multiply](/images/Arithmetic/multiply.PNG)


* 解法


```java
class Solution {
    public String multiply(String num1, String num2) {
        int m = num1.length(), n = num2.length();
        int[] res = new int[m + n];
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                int mul = (num1.charAt(i) - '0') * (num2.charAt(j) - '0');
                int p1 = i + j, p2 = i + j + 1;
                int sum = mul + res[p2];
                res[p2] = sum % 10;
                res[p1] += sum / 10;
            }
        }
        int i = 0;
        while (i < res.length && res[i] == 0) {
            i++;
        }
        StringBuilder stringBuilder = new StringBuilder();
        while (i < res.length) {
            stringBuilder.append(res[i]);
            i++;
        }
        return stringBuilder.length() == 0 ? "0" : stringBuilder.toString();
    }
}
```









