# Algorithms-Data-structures
算法（ACM）和数据结构总结&lt;本>
#一，
#排序算法[baidu](http://baike.baidu.com/link?url=4KBTW3C3Mtm28l5sddOUvHnc6AnYebw6pga6WqARZssLj9LNrw32cZPfpC9k411tOfTRGSKfISaLfSzW2BWDUZEt3Um3n-eCAid4nnadFBlxM_uMre5V1DxZkUq3Oow6)
[动态理解参考1](http://blog.csdn.net/left_la/article/details/8648133)
[动态理解参考2](http://blog.jobbole.com/11745/)
现在我们涉及到的排序算法大都是内排序（待排序的数据全都放在内存当中），还有一种排序为[外排序](http://baike.baidu.com/link?url=LrgAFCqJzO8gc2Xgqmg_XzLyts5DDqRRcyKCzaWGqshuBrFKG-rwQpv_ueouJCHBN-ebt01ip46fEgPZzF9zNg9f35INuvEUk7gPvTTNQCRPvvU3-MZHY-Pm28ramF8e)（大部分数据因为太多放在外存当中）——百度搜索引擎等一些大数据的处理，一般用后缀（组）实现。
# 1，[插入排序](http://baike.baidu.com/link?url=Z9Ppdy8MbZjKBIdhP2W-oDhnzIjnPEXL0rjjNnz_zbzGJo7KkQx9H4EZh9K6ZOFMTFbJbdjA6S1ITRJbsfcVGDb1RVT3ACY7JY1q2m-xdjWPW5RPoPZ7qd5mYFG2fAEh)

1.1 直接插入排序InsertSort（sort函数）
伪代码及特征：单键，数组，我们讨论正序排序，r[0]起到暂存待插关键码和哨兵（防越界，减掉），一般初始存入的数据从r[1]开始，排序时从r[2]开始暂存于r[0]，然后和前面一个r[1]开始比较，后面依次从后面开始向前面一个一个比较。比较稳定，空间上只需要一个辅助空间单元r[0],时间复杂度O(n)-O(n^2),可对该算法进行优化改进，后面的查找改为折半查找，但会失去稳定性（排序前后相同的数据是否相同），查找的次数减少但移动的次数不会改变，可根据具体的应用来改进。

> viod InsertSort(int r[],int n)//0号单元用作暂存单元和监视哨兵
> {
>  for(i = 2; i <= n; i++)
>  {
>    r[0]=r[i];  //暂存关键码  
>    for(j = i-1; r[0] < r[j]; j--)//寻找插入位置
>    r[j+1] = r[j];//记录后移
>    r[j+1] = r[0];
>  }
> }
适用范围：当前记录基本有序或待排序记录比较少时。

1.2 希尔排序shell sort伪代码
伪代码及特征：待排序列记录按关键码基本有序（基本有序不是局部有序，必须分清楚），其实它是直接插入排序一种改进，只是把后面一个一个开始比较改为了按段（子序列）比较，跳跃式查找待查位置，如序列r[n],n=9,d=「n/2」(理想的是最接近于n/2的2次幂方)，我们这里取d=4；则1、5、9一组比较排序，2、6一组排序，3、7一组排序，4、8一组排序；这才是第一趟排序，第二趟d=「d/2」，及d=2，按相同的方式开始排序，第三趟则是d=1，就是简单的直接插入排序。这里r[0]只是暂存单元，不起哨兵作用，所以要判断越界条件。时间复杂度为O(n^2)-O(n㏒2 n)。不稳定，空间辅助空间也只需要一个r[0]。
> void ShellSort(int r[],int n)//0号单元作用暂存单元
> {
> for(d = n/2; d >= 1; d =d/2)//以增量为d进行直接插入排序
>  {
>  for(i =d+1; i <= n; i++)
>    {
>    r[0] = r[i];//暂存被插入记录
>   for(j = i-d; j > 0 && r[0] < r[j]; j = j-d;)
>    r[j+d] = r[j];//记录后移d个位置  
>    r[j+d] = r[0];
>    
>    }
>  }
> }

# 2，交换排序([冒泡排序](http://baike.baidu.com/link?url=ZS4KzjXYKoJ2tmkH5VY0Aha-bSPboyzlmSGREJETEoVX0J15X1U4eUVjrzRes-NTBVCokCVR8cCeDNZjPFn0OiVNL-AmVNB0W967c9BaEZYFXXs9uEkG8WgoJCFN-j2W)和[快速排序](http://baike.baidu.com/link?url=EII9VU1EoF8UhfM9DqTP_DWOU8K-NCEL5gm5T3oPwpanN7XusNinqFc3aLff6dYSdSMgOD6gqGD1pXkKYxGKnksG0OvFE6puO_mC1nNr1ND_tnpDgo7UqeiSGTST1OQ_jpCEJCdoLlL0dVyyNwAZJq))
2.1
冒泡排序（BubbleSort）
伪代码及特征：

> void BubbleSort(int r[],int n)//0号单元用作交换操作的暂存单元
> {
>   exchange = n;//第一趟冒泡排序的区间是[1，n]
>   while(exchange!=0)//当上一趟排序有记录交换时
>   {
>  bound = exchange;
>  exchange = 0;
>  for(j = 1; j < bound; j++)//一趟冒泡，排序区间是[1，bound]
>      if(r[j] > r[j+1]){
>      r[j]<---->r[j+1];
>      exchange = j;//记录每一次记录交换的位置
>  }
>  
>  }
>  
> }

2.2
快速排序
