---
layout: article
title: 快速排序可以用一行代码写完
---

快速排序要一次性写成功是很困难的，我是办不到，我们通常写的快速排序是基于数组这个线性结构，通过移动交换来完成，这篇文章的标题叫做快速排序为什么可以用一行代码写完，请君往下看，我会在后面介绍另一种方法，依靠这种方法任何人都可以在不到一分钟的时间内写出正确的快速排序,因为它只需要一行代码。



我们先来完成传统的排序代码，假如我们按从小到大来排序，快速排序的基本思想是在数组中选择一个数作为基准数，一般是第一个数，然后遍历这个数组寻找一个位置让小的在这个数左边，大的在这个数的右边，我们数组下标是从0开始的，所以其实位置就是小于基准数的所有元素的个数，比如[4,-1,0,1,2,3,6,7]，选4作为基准，那么它应该出现的位置就是下标5，因为小于它的数一共有5个。

落实到程序，我们选一个指针用来指向基准位置，默认是第一个数的位置，然后我们从第二个数开始遍历，一旦发现有小于基准的数指针就要向后移动一下，如果没有发现，那么指针就不动，如果直到最后指针都没动说明这个数组第二个数开始都是大于第一个数的，同时一旦发现有小于基准的数就要交换到指针处，这样保证了指针走过的位置都是小于基准数的，最后把基准数交换到指针处。

代码实现如下
{% highlight java %}
public int partition(int[] array, int left, int right) {

        //假定第一个是基准
        int point = left;

        int compare = array[left];

        //从第二个开始向后看，如果发现比第一个小就要移动指针同时做交换
        for (int j = left + 1; j <= right; j++) {
            if (array[j] < compare) {
                point++;
                swap(array, point, j);
            }
        }

        //经过上述循环可能在其他地方找到了基准应该出现的位置,把第一个数交换到指针处
        swap(array, point, left);

        return point;
 }

{% endhighlight %}

上述代码完成了一次基准定位和元素移动，保证了两个子数组在基准的左右，我们再分别取出两个子数组再传递给快速排序函数，最后整个数组就排序完毕
代码如下
{% highlight java %}
public void quickSort(int[] array, int left, int right) {
        if (left < right) {
            int pivot = partition(array, left, right);
            quickSort(array, left, pivot - 1);
            quickSort(array, pivot + 1, right);
        }
}

/**

外部的调用如下
int[] array = {4,0,1,2,3,6,9};
quicSortk(array,0,array.length - 1);
*/
{% endhighlight %}


当left小于right的时候说明有数组长度为1，还需要处理，如果是相等的则说明是只有一个数的数组，不需要排序了，如果是大于的说明基准定位到了边界上，左或者右已经没有元素了。
OK，这样就完成了一个快速排序，当然写法有很多种，比如可以从中间选一个数作为基准。但是本质思想我们已经实现了。


透过上面的程序，我们发现，我们整个过程没有申请新的内存空间，都是在传进来的那个数组上不断的修改，程序复杂在循环，移动，交换，甚至于判断递归的结束条件，我们如果被要求写一个快速排序，更多的就可能在循环的边界，交换，递归条件上出错，其实完全有另外一种方式，我们经常在说避免重复代码，可是大家程序生涯中写了多少for语句？for语句本身其实已经比较复杂了，声明变量，步进，条件控制，可是其本质就在做一个事，遍历数组，我们又经常在说封装，如果数组是一个对象，为什么要把它的数据暴露出来，然后我们再用for去一个一个的解开？ 最好的方式难道不是数组直接提供一个遍历的方法吗？它遍历自己然后把遍历到的数据传给我们的程序，我们只负责拿数据然后做处理，这样的程序就简单得多了。

{% highlight java %}
for (int j = left + 1; j <= right; j++) {
            if (array[j] < compare) {
                point++;
                swap(array, point, j);
            }
}
{% endhighlight %}
上面这个代码的真正核心其实是那个if语句，swap甚至也应是数组的方法。
上面的快速排序还有一个复杂性就是我们不断的修改数组，始终要关注数组的下标位置，partition函数废那么大力气就是在数组中把数组分割成两个子数组，如果数组也自己有一个方法来完成选取子数组，那么这个函数也将相当简单而不出错。那么程序该怎么写呢？在java中实现是不可能的，我们来用ruby和scala语言，他们都提供了我们完成数组选取和遍历的的库函数,ruby的写法是array.select{|i| i < pivot}，scala的写法是a.filter(_ < pivot)，所以用这两种语言实现的代码如下：

Ruby:
{% highlight ruby %}
def quicksort a
  (pivot = a.pop) ? quicksort(a.select{|i| i < pivot}) + [pivot] + quicksort(a.select{|i| i > pivot}) : []
end
{% endhighlight %}

Scala:
{% highlight scala %}
def sort(a: List[Int]): List[Int] = {
    if (a.length < 2)
      a
    else {
      val pivot = a(a.length / 2)
      sort(a.filter(_ < pivot)) ::: a.filter(_ == pivot) ::: sort(a.filter(_ > pivot))
    }
}
{% endhighlight %}

这一切都是声明式的，没有修改内存，没有移动和交换，就这么简单，这就是面向函数的编程，但是又结合了面向对象，因为array.select()这种代码是调用对象的方法，假如一个完全不懂编程的人来写快速排序，是这个程序好懂呢还是上面开始的那个版本好懂？


### 总结：为什么这是可行的？

为什么解决一个问题存在两个完全不同的方式？我们从最开始非常繁琐的版本可以简化到只要一行代码，这其中到底做了什么事情？我们可以发现几个东西

1. 去掉重复代码，我们写太多for了，而真正我们只关心for里面那些代码
2. 代码必须能够传递出去运行，它不运行就只是代码，运行起来了就读取运行环境提供的数据
3. 专注核心逻辑或者业务，抽象是简化问题的强大工具