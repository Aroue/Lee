# **剑指offer**

#### 1、输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。（链表反转）

```java
public class Solution {
     ArrayList<Integer> ret = new ArrayList<>();
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) {
        
        if (listNode != null) {
          // 递归
            this.printListFromTailToHead(listNode.next);
            ret.add(listNode.val);
        }
        return ret;
    }
}
```



#### 2、



### 11、二进制中1的个数

输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

#### n&(n-1)

该位运算去除 n 的位级表示中最低的那一位。

```
n       : 10110100
n-1     : 10110011
n&(n-1) : 10110000
```

```java
public int NumberOf1(int n) {
  int cnt  = 0;
  while(n != 0){
    n &= (n - 1);
    cnt++;
  }
  return cnt;
}
// 调用 Integer 的静态方法 bitCount()
public int NumberOf1(int n) {
    return Integer.bitCount(n);
}
```



### 12、数值的整数次方

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

保证base和exponent不同时为0





```java
public double Power(double base, int exponent) {
        
 }
```



### 14、链表中倒数第k个节点（双指针）

#### 题目描述

输入一个链表，输出该链表中倒数第k个结点。

#### 解题思路

设链表的长度为 N。设置两个指针 P1 和 P2，先让 P1 移动 K 个节点，则还有 N - K 个节点可以移动。此时让 P1 和 P2 同时移动，可以知道当 P1 移动到链表结尾时，P2 移动到第 N - K 个节点处，该位置就是倒数第 K 个节点。

```java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode FindKthToTail(ListNode head,int k) {
       if (head == null)
            return null;
        ListNode P1 = head, P1 = head;
        while(P1 != null && k-- > 0)
            P1 = P1.next;
        if (k > 0)
            return null;
        while(P1 != null) {
            P1 = P1.next;
            P2 = P2.next;
        }
        return P2;
    }
}
```



### 26、两个链表的第一个公共节点

####题目描述

输入两个链表，找出它们的第一个公共结点。

####解题思路

![img](https://cs-notes-1256109796.cos.ap-guangzhou.myqcloud.com/5f1cb999-cb9a-4f6c-a0af-d90377295ab8.png)

设 A 的长度为 a + c，B 的长度为 b + c，其中 c 为尾部公共部分长度，可知 a + c + b = b + c + a。

当访问链表 A 的指针访问到链表尾部时，令它从链表 B 的头部重新开始访问链表 B；同样地，当访问链表 B 的指针访问到链表尾部时，令它从链表 A 的头部重新开始访问链表 A。这样就能控制访问 A 和 B 两个链表的指针能同时访问到交点。



```java
public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        ListNode p1 = pHead1, p2 = pHead2;
        while(p1 != p2) {
            p1 = p1 == null ? pHead2 : p1.next;
            p2 = p2 == null ? pHead1 : p2.next;
        }
        return p1;
    }
```



### 29、最小的k个数

#### 题目描述

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

#### 解题思路

#### 快速选择

- 复杂度：O(N) + O(1)
- 只有当允许修改数组元素时才可以使用

快速排序的 partition() 方法，会返回一个整数 j 使得 a[l..j-1] 小于等于 a[j]，且 a[j+1..h] 大于等于 a[j]，此时 a[j] 就是数组的第 j 大元素。可以利用这个特性找出数组的第 K 个元素，这种找第 K 个元素的算法称为快速选择算法。

```java
   public ArrayList<Integer> GetLeastNumbers_Solution(int[] nums, int k) {
        ArrayList<Integer> res = new ArrayList<>();
        if (k > nums.length || k <= 0)
            return res;
        findKthSmallest(nums, k - 1);
        /* findKthSmallest 会改变数组，使得前 k 个数都是最小的 k 个数 */
        for (int i = 0; i < k; i++)
            res.add(nums[i]);
        return res;
    }

    public void findKthSmallest(int[] nums, int k) {
        int l = 0,h = nums.length - 1;
        while(l < h) {
            int p = partition(nums,l,h);
            if (p == k)
                break;
            else if (p > k)
                h = p - 1;
            else
                l = p + 1;
        }
    }

    private int partition(int[] nums, int l, int h) {
        int p = nums[l];
        int i = l,j = h + 1;
        while(true) {
            while(i != h && nums[++i] < p);
            while(j != l && nums[--j] > p);
            if (i >= j)
                break;
            swap(nums,i,j);
        }
        swap(nums,l,j);
        return j;
    }

    private void swap(int[] nums, int i, int j) {
        int t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }
```



#### 大小为 k 的大顶堆

- 复杂度：O(NlogK) + O(K)

- 特别适合处理海量数据

维护一个大小为 K 的最小堆过程如下：在添加一个元素之后，如果大顶堆的大小大于 K，那么需要将大顶堆的堆顶元素去除。

```java
public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        if (k > input.length || k <= 0)
            return new ArrayList<>();
        PriorityQueue<Integer> heap = new PriorityQueue<>((o1,o2) -> o2 - o1);
        for (int value : input) {
            heap.add(value);
            if (heap.size() > k)
                heap.poll();

        }
        return  new ArrayList<>(heap);
    }
```



### 62、二叉查找树的第K个节点

#### 题目描述

给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）    中，按结点数值大小顺序第三小结点的值为4。

#### 解题思路

利用二叉搜索树的中序遍历有序的特点求解。

```java
public class Solution {
    private TreeNode res = null;
    private int cnt = 0;
    public TreeNode KthNode(TreeNode pRoot, int k)
    {
        inOrder(pRoot,k);
        return res;
    }
    
    private void inOrder(TreeNode root, int k) {
        if (root == null)
            return;
        inOrder(root.left,k);
        cnt++;
        if (cnt == k)
            res = root;
        inOrder(root.right,k);
        
    }
}
```





### 64、滑动窗口的最大值（PriorityQueue）

#### 题目描述

给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}； 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。

#### 解题思路

利用数据结构 PriorityQueue 维护一个大小为 size 大顶堆

```java
public ArrayList<Integer> maxInWindows(int[] num, int size)
    {
        ArrayList<Integer> res = new ArrayList<>();
        if (size > num.length || size < 1)
            return res;
        PriorityQueue<Integer> heap = new PriorityQueue<>((o1, o2) -> o2 - o1);
        for (int i = 0;i < size;i++) {
            heap.add(num[i]);
        }
        res.add(heap.peek());

        for (int i = 0,j = i + size;j < num.length;j++,i++) {
            heap.remove(num[i]);
            heap.add(num[j]);
            res.add(heap.peek());
        }
        return res;
    }
```

