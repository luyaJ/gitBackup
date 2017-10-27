---
title: js几种常见排序算法
toc: true
date: 2017-08-10 16:09:46
tags: [JavaScript,算法] 
categories: Web前端
---

这里整理几种常用的排序，以后再加入其他的。

<!--more-->

## 插入排序

最普通的排序算法，把待排序的纪录按其关键码值的大小逐个插入到一个已经排好序的有序序列中，直到所有的纪录插入完为止，得到一个新的有序序列。

**原理图：** （插入操作要进行 `n-1` 次）

![插入排序](http://ot4r4qnml.bkt.clouddn.com/insertSort.png)

**代码：**

```js
function insertSort(array){
    //假设第0个元素是一个有序的数列，第1个以后的是无序的序列，
    //所以从第1个元素开始将无序数列的元素插入到有序数列中
    for(var i=1;i<array.length;i++){
        if(array[i] < array[i-1]){
            //取出无序数列中的第i个作为被插入元素
            var temp = array[i];
            //记住有序数列的最后一个位置，并且将有序数列位置扩大一个
            var j = i - 1;

            //比大小，找到被插入元素所在的位置
            while(j >= 0 && temp < array[j]){
                array[j+1] = array[j];
                j--;
            }
            //插入
            array[j+1] = temp;
        }
    }
}
//var array = new Array(36,27,15,20,45,10);
var array = [36,27,15,20,45,10];
document.write("before: " + array + "<br>");
insertSort(array);
document.write("after: " + array);
```

**算法分析：**

* 最佳情况：输入数组按升序排列。T(n) = O(n)
* 最坏情况：输入数组按降序排列。T(n) = O(n^2)
* 平均情况：T(n) = O(n^2)

## 二分插入排序(折半插入排序)

二分法插入排序是在插入第i个元素时，对前面的0～i-1元素进行折半，先跟他们中间的那个元素比，如果小，则对前半再进行折半，否则对后半进行折半，直到left<right，然后再把第i个元素前1位与目标位置之间的所有元素后移，再把第i个元素放在目标位置上。

**原理图：** （high<low，查找结束，插入位置为low或high+1）

![二分插入排序](http://ot4r4qnml.bkt.clouddn.com/binaryInsertSort.png)

**代码：**

```js
function BinaryInsertSort(array){
    for(var i=1;i<array.length;i++){
        var key = array[i];
        var low = 0, high = i - 1;
        while(low <= high){
            var mid = parseInt((low+high)/2);
            if(key<array[mid]){
                high = mid -1;
            } else {
                low = mid + 1;
            }
        }
        for(var j=i-1;j>=low;j--){
            array[j+1] = array[j];
        }
        array[low] = key;  //插入位置为low或high+1
    }
    return array;
}
var array = [53,27,36,15,69,42];
document.write("before: " + array + "<br>");
BinaryInsertSort(array);
document.write("after: " + array);
```

**算法分析：**

* 最佳情况：T(n) = O(nlogn)
* 最差情况：T(n) = O(n^2)
* 平均情况：T(n) = O(n^2)

## 冒泡排序(Bubble Sort)

它重复地走访过要排序的数列，一次比较两个元素，每次遍历就将最大（或最小）值推至最前。越往后遍历查询次数越少， 跟插入排序刚好相反。

**原理图：**

![冒泡排序gif](http://ot4r4qnml.bkt.clouddn.com/bubbleSort.gif)

![冒泡排序](http://ot4r4qnml.bkt.clouddn.com/bubbleSort.png)

**代码：**

```js
function bubbleSort(array){
    for(var i=0;i<array.length;i++){
        for(var j=array.length-1;j>i;j--){
            if(array[j] < array[j-1]){
                var temp = array[j-1];
                array[j-1] = array[j];
                array[j] = temp;
            }
        }
    }
    return array;
}
var array = [53,27,36,15,69,42];
document.write("before: " + array + "<br>");
bubbleSort(array);
document.write("after: " + array);
```

**算法分析：**

* 最佳情况：T(n) = O(n)
* 最差情况：T(n) = O(n^2)
* 平均情况：T(n) = O(n^2)

## 改进的冒泡排序

如果在某次的排序中没有出现交换的情况，那么说明在无序的元素现在已经是有序了，就可以直接返回了

**代码：**

```js
function bubbleSort(array){
    for(var i=0;i<array.length;i++){
        var exchange = 0;
        for(var j=array.length-1;j>i;j--){
            if(array[j] < array[j-1]){  //使较小的元素浮在上面
                var temp = array[j-1];
                array[j-1] = array[j];
                array[j] = temp;
                exchange = 1;
            }
        }
        if(!exchange) return array;
    }
    return array;
}
var array = [53,27,36,15,69,42];
document.write("before: " + array + "<br>");
bubbleSort(array);
document.write("after: " + array);
```

## 快速排序

在数据集之中，选择一个元素作为”基准”（pivot）。 所有小于”基准”的元素，都移到”基准”的左边；所有大于”基准”的元素，都移到”基准”的右边。 对”基准”左边和右边的两个子集，不断重复第一步和第二步，直到所有子集只剩下一个元素为止。

**原理图：**

![快速排序](http://ot4r4qnml.bkt.clouddn.com/quickSort.png)

**代码：**

```js
var array = [36,53,27,20,15,69,42];
    document.write("before: " + array + "<br>");
    function quickSort(array){
        if (array.length <= 1) {return array};
        var pivotIndex = Math.floor(array.length / 2);  //pivot基准
        var pivot = array.splice(pivotIndex,1);
        var left = [];
        var right = [];
        for (var i = 0; i < array.length; i++){
            if(array[i] < pivot) {
                left.push(array[i]);
            } else {
                right.push(array[i]);
            }
        }
        return quickSort(left).concat([pivot],quickSort(right));
    }
    document.write("after: " + quickSort(array));
```

**算法分析：**

* 最佳情况：T(n) = O(nlogn)
* 最差情况：T(n) = O(n^2)
* 平均情况：T(n) = O(nlogn)

## 选择排序

在无序区中选出最小的元素，然后将它和无序区的第一个元素交换位置。

**原理图：**

![选择排序](http://ot4r4qnml.bkt.clouddn.com/selectSort.png)

**代码：**

```js
function selectSort(array) {
     for(var i=0;i<array.length;i++){
         var min = array[i];
         for(var j = i+1;j<array.length-1;j++){
             if(min > array[j]){
                 var temp = min;
                 min = array[j];
                 array[j] = temp;
             }
         }
         array[i] = min;
     }
     return array;
}
var array = [53,27,36,15,69,42];
document.write("before: " + array + "<br>");
selectSort(array);
document.write("after: " + array);
```

**算法分析：**

* 最佳情况：T(n) = O(n^2)
* 最差情况：T(n) = O(n^2)
* 平均情况：T(n) = O(n^2)

## 参考资料

[知乎-浅谈前端](https://zhuanlan.zhihu.com/p/20177776)
[博客园-9种排序算法](http://www.cnblogs.com/JChen666/p/3360853.html)