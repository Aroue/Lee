## 一、基础 ##
### 原始数据类型与表达式 ###
| 术语 | 例子 | 定义 |
| ------ | ------ | ------ |
| 原始数据类型 | int double boolean char | 一组数据和对其所能进行的操作的集合(Java 语言内置) |
| 标识符 | a abc Ab$ a_b ab123 lo hi | 由字母、数字、下划线和 $ 组成的字符串，首字符不能是数字 |
| 变量 | [任意标识符] | 表示某种数据类型的值 |
| 运算符 | + - */ | 表示某种数据类型的运算 |
| 字面量 | boolean    true false | 值在源代码中的表示 |
| 表达式 | boolean     lo <= hi | 字面量、变量或是能够计算出结果的一串字面量、变量和 运算符的组合 |

## 二、排序 ##

### 排序算法抽象类

```java
public abstract class Sort
{
    // 返回两个数中较小的数
    public boolean less(Comparable v, Comparable w)
    {  return v.compareTo(w) < 0;  }
    // 交换数组中的两个元素
    public void exch(Comparable[] a, int i, int j)
    {  Comparable t = a[i]; a[i] = a[j]; a[j] = t;  }
    // 在单行中打印数组
    private static void show(Comparable[] a) { 
        for (int i = 0; i < a.length; i++)
            StdOut.print(a[i] + " ");
        StdOut.println();
    }
    // 测试数组元素是否有序
    public static boolean isSorted(Comparable[] a) { 
        for (int i = 1; i < a.length; i++)
            if (less(a[i], a[i-1]))  return false;
        return true;
    }
}
```



### 选择排序 ###

首先，找到数组中最小的那个元素，其次，将它和数组的第 一个元素交换位置(如果第一个元素就是最小元素那么它就和自己交换)。再次，在剩下的元素中 找到最小的元素，将它与数组的第二个元素交换位置。如此往复，直到将整个数组排序。

对于长度为 *N* 的数组，选择排序需要大约 *N*2/2 次比较和 *N* 次交换，运行时间和输入无关，所以即使是已经有序的数组和相同数量级的输入的数组排序所需的时间也是一样的。

选择排序是一种不稳定的排序算法。

```java
public class Selection extends Sort
{
    // 将data[]按升序排列
    public void sort(Comparable[] data) { 
        
        int N = data.length;   // 数组长度
        // 将data[i]和data[i+1..N]中最小的元素交换
        for (int i = 0; i < N; i++)
        {
            int min = i;  // 最小元素的索引 
            for (int j = i+1; j < N; j++)
               if (less(data[j], data[min])) min = j;
            exch(a, i, min);
        } 
    }
}
// less()、exch()、isSorted()方法见“排序算法抽象类”
```


### 冒泡排序 ###

它重复地走访过要排序的元素列，依次比较两个相邻的元素，如果他们的顺序（如从大到小、首字母从A到Z）错误就把他们交换过来。走访元素的工作是重复地进行直到没有相邻元素需要交换，也就是说该元素已经排序完成。

```java
public class BubbleSort extends Sort{
    private void sort() {
        int N = data.length;
        boolean isSorted = false; // 数组是否已经有序
        for (int i = 0; i < N - 1 && !isSorted ;i++) {
            isSorted = true; // 若此轮循环没有发生交换，则数组已经有序
            for (int j = 0;j < N - 1 - i ;j++){
                if (less(data[j + 1], data[j])) {
                    isSorted = false; // 发生交换，数组未完成排序
                    exch(data,j + 1,j);
                }
            }
        }
    }
}
```



### 插入排序 ###

插入排序法，就是检查第i个数字，如果在它的左边的数字比它大，进行交换，这个动作一直继续下去，直到这个数字的左边数字比它还要小，就可以停止了。插入排序法主要的回圈有两个变数：i和j，每一次执行这个回圈，就会将第i个数字放到左边恰当的位置去。

```java
public class Insertion extends Sort{
    // 将data[]按升序排列
    private void sort(Comparable[] data) {
        int N = data.length;
        for (int i = 1; i < N; i++)
        {
            for (int j = i; j > 0 && less(data[j], data[j-1]); j--)
                exch(data, j, j-1);
        }
    }
}
```



