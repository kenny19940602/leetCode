# 算法

## 排序算法

排序算法可以分为 **内部排序** 和 **外部排序**

内部排序是数据记录在内存中进行排序。

而外部排序是因排序的数据很大，一次不能容纳全部的排序记录，在排序过程中需要访问外存。

常见的内部排序算法有： 插入排序、希尔排序、选择排序、冒泡排序、归并排序、快速排序、堆排序、基数排序...

用一张图该概括：

|排序算法|平均时间复杂度|最好情况|最坏情况|空间复杂度|排序方式|稳定性|
|---|---|---|---|---|---|---|
|冒泡排序|O(n²)|O(n)|O(n²)|O(1)|In-place|稳定|
|选择排序|O(n²)|O(n²)|O(n²)|O(1)|In-place|不稳定|
|插入排序|O(n²)|O(n)|O(n²)|O(1)|In-place|稳定|
|希尔排序|O(n log n)|O(n log² n)|O(n log² n)|O(1)|In-place|不稳定|
|归并排序|O(n log n)|O(n log n)|O(n log n)|O(n)|Out-place|稳定|
|快速排序|O(n log n)|O(n log n)|O(n²)|O(log n)|In-place|不稳定|
|堆排序|O(n log n)|O(n log n)|O(n log n)|O(1)|In-place|不稳定|
|计数排序|O(n + k)|O(n + k)|O(n + k)|O(k)|Out-place|稳定|
|桶排序|O(n + k)|O(n + k)|O(n²)|O(n + k)|Out-place|稳定|
|基数排序|O(n x k)|O(n x k)|O(n x k)|O(n + k)|Out-place|稳定|

<center>时间复杂度和空间复杂度</center>

**关于时间复杂度：**

* 平方阶(O(n²))排序各类简单排序：直接插入、直接选择排序和冒泡排序。
* 线性对数阶(O(nlog2n))排序 快速排序、堆排序和归并排序。
* O(n1+∮)排序，∮是介于0和1之间的常数。 希尔排序。
* 线性阶(O(n))排序 基数排序，此外还有桶、箱排序。

**关于稳定性：**

* 稳定的排序算法： 冒牌排序、插入排序、归并排序和基数排序。
* 不是稳定的排序算法： 选择排序、快速排序、希尔排序、堆排序。


> 1. 冒牌排序

### 1.1 算法步骤

* 比较相邻的元素。如果第一个比第二个大，就交换他们两。
* 对每一对相邻的元素。如果第一个比第二个大，就交换他们两个。
* 对每一对相邻做同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会使最大的数
* 针对所有的元素重复以上的步骤，除了最后一个。
* 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

###1.2 动画演示

![冒牌排序.gif](images/640.gif)

<center>冒泡动画演示</center>

###1.3 参考代码

```java
   int[] arr = {1, 3, 5123, 566, 123, 51};
        /**
         *比较轮数
         */
        for (int i = 1; i < arr.length; i++) {
            /**
             * 比较次数 外层循环一次 内层循环所有
             * 过程 取到第一个数 去和后面的依次数比较  比后面的数大则交换位置（所有不用比较最后一个数 这就是轮数比数组长度少一的原因）
             */
            for (int j = 0; j < arr.length - i; j++) {
                if (arr[j] > arr[j + 1]) {
                    int tmp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = tmp;
                }
            }
        }
        Arrays.stream(arr).forEach(System.out::println);

```

>2.选择排序

###2.1 算法步骤

* 首先在未排序序列中找到最小(大)元素，存放到排序序列起始位置
* 在从剩余未排序元素中继续寻找最小(大)元素，然后放到已排序序列的末尾
* 重复第二步，直到所有元素均排序完毕。

###2.2 动画演示

![选择排序.gif](images/选择排序.gif)

<center>选择排序动画演示</center>

###2.3参考代码

```java
    int[] arr = {1, 3, 5123, 566, 123, 51};
      /**
         * 总共要经过 N-1 轮比较
         */
        for (int i = 0; i < arr.length; i++) {
            int min = i;
            for (int j = arr.length-1; j>0+i ; j--) {
                if (arr[min] > arr[j]) {
                    min = j;
                }
            }
            int tmp = arr[i];
            arr[i] = arr[min];
            arr[min] = tmp;
        }
        Arrays.stream(arr).forEach(System.out::println);
        //第一个的缺点。操作数组频繁导致效率低

        /**
         * 总共要经过 N-1 轮比较
         */
        for (int i = 0; i < arr.length - 1; i++) {
            int min = i;
            /**
             *  每轮需要比较的次数 N-i
             */
            for (int j = i + 1; j < arr.length; j++) {
                if (arr[min]>arr[j]){
                    // 记录目前能找到的最小值元素的下标
                    min = j;
                }
                // 将找到的最小值和i位置所在的值进行交换
                if (min != i) {
                    int tmp = arr[min];
                    arr[min] = arr[i];
                    arr[i] = tmp;
                }
            }
        }
        Arrays.stream(arr).forEach(System.out::println);

```

>3.插入排序

###3.1 算法步骤

* 将第一待排序序列第一个元素看做一个有序序列，把第二个元素到最后一个元素当成是未排序序列。
* 从头到尾依次扫描未排序序列，将扫描到的每个元素插入有序序列的适当位置。(如果待插入的元素与有序序列的某个元素相等，则将插入元素插入到相等元素的后面。)

###3.2 动画演示

![插入排序.gif](images/插入排序.gif)

<center>插入排序动画演示</center>

###3.3 参考代码

```java
  int[] arr = {4, 3, 5123, 566, 123, 51};
        /**
         * 从下标为1的元素开始选择合适的位置插入，因为下标为0的只有一个元素，默认是有**序的
         */
        for (int i = 1; i < arr.length; i++) {
             // 记录要插入的数据
            int tmp = arr[i];
             // 从已经排序的序列最右边的开始比较，找到比其小的数
            int j = i;
            while (j > 0 && arr[j] < arr[j - 1]) {
                arr[j] = arr[j-1];
                arr[j - 1] = tmp;
                j--;
            }
        }
        Arrays.stream(arr).forEach(System.out::println);

        //第一个的缺点，只要后一个比前一个小，就插入。插入频繁导致效率低

         // 从下标为1的元素开始选择合适的位置插入，因为下标为0的只有一个元素，默认是有序的
        for (int i = 1; i < arr.length; i++) {

            // 记录要插入的数据
            int tmp = arr[i];

            // 从已经排序的序列最右边的开始比较，找到比其小的数
            int j = i;
            while (j > 0 && tmp < arr[j - 1]) {
                arr[j] = arr[j - 1];
                j--;
            }
            // 存在比其小的数，插入
            if (j != i) {
                arr[j] = tmp;
            }
        }
        Arrays.stream(arr).forEach(System.out::println);
```

>4.希尔排序

###4.1 算法步骤

* 选择一个增量序列t1,t2,....,tk,其中ti>tj,tk=1。
* 按增量序列个数k，对序列进行k趟排序。
* 每趟排序，根据对应的增量ti，将待排序列分割成若干长度为m的子序列，分别对各个表进行直接插入排序。仅增量因子为1时，整个序列作为一个表来处理，表示长度即为整个序列的长度。

###4.2 动画演示

![希尔排序.gif](images/希尔排序.gif)

<center>希尔排序动画演示</center>

```java
  int gap = 1;
        while (gap < arr.length) {
            gap = gap*3 + 1;
        }
        while (gap > 0) {
            for (int i = gap; i < arr.length; i++) {
                int tmp = arr[i];
                int j = i - gap;
                while (j >= 0 && arr[j] > tmp) {
                    arr[j + gap] = arr[j];
                    j -= gap;
                }
                arr[j + gap] = tmp;
            }
            gap = (int) Math.floor(gap / 3);
        }
        Arrays.stream(arr).forEach(System.out::println);
        //分组插入思想，数组越有序效率越高
```

















