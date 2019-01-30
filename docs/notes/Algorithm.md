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

### 排序算法类的抽象模版

```java
public class Example
{
    public static void sort(Comparable[] a)
    {                                           }
    // 返回两个数中较小的数
    private static boolean less(Comparable v, Comparable w)
    {  return v.compareTo(w) < 0;  }
    // 交换数组中的两个元素
    private static void exch(Comparable[] a, int i, int j)
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
首先，找到数组中最小的那个元素，其次，将它和数组的第 一个元素交换位置(如果第一个元素就是最小元素那么它就和自己交换)。再次，在剩下的元素中 找到最小的元素，将它与数组的第二个元素交换位置。如此往复，直到将整个数组排序。这种方法 叫做选择排序，因为它在不断地选择剩余元素之中的最小者。

对于长度为 *N* 的数组，选择排序需要大约 *N*2/2 次比较和 *N* 次交换，选择排序是一种稳定的排序方法，多亿对于不同的输入都有同样的排序时间。

伪代码：

```java
public class Selection
{
    // 将a[]按升序排列
    public static void sort(Comparable[] a) { 
    // 数组长度
    int N = a.length;  
    // 将a[i]和a[i+1..N]中最小的元素交换
    for (int i = 0; i < N; i++)
    {   // 最小元素的索引 
        int min = i; 
        for (int j = i+1; j < N; j++)
           if (less(a[j], a[min])) min = j;
        exch(a, i, min);
    } 
}
// less()、exch()、isSorted()方法见“排序算法类模板”
```



java 实现代码：

```java
for (int i = 0; i < data.length;i++) {
            for (int j = i + 1;j < data.length;j++){
                if (data[j] < data[i]) {
                    int temp = data[i];
                    data[i] = data[j];
                    data[j] = temp;
                }
            }
        }
```
### 插入排序 ###

