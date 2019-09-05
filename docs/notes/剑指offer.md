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

