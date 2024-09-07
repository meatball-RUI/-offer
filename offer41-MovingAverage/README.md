# 滑动窗口的平均值
    初始化滑动窗口的大小。   
    向窗口中添加元素，并动态返回当前窗口中的平均值。  
    当窗口中的元素数量超过指定大小时，移除最早进入窗口的元素。  
## 问题描述：
实现一个能够计算滑动窗口平均值的类。这个类的核心功能是动态接收输入流中的数据，并在窗口大小固定的情况下计算当前窗口内的平均值。
```java
class MovingAverage {
    public MovingAverage(int size) { ... } // 构造函数
    public double next(int val) { ... }    // 返回滑动窗口的平均值
}
```
## Code
```java
package offer41; 

//导入LinkedList类
import java.util.LinkedList;
//导入Queue接口
import java.util.Queue;

public class MovingAverage{
	//用于存储滑动窗口中的元素
	private Queue<Integer> nums;
	//窗口的大小
	private int capacity;
	//窗口的总和值
	private int sum;
	
	//用构造函数初始化元素值
	public MovingAverage(int size) {
		nums=new LinkedList<>();
		capacity=size;
		sum=0;
	}
	//向滑动窗口中加入一个值计算平均值
	public double next(int val) {
		//向滑动窗口中安全地加入一个值
		nums.offer(val);
		//sum加上新的值
		sum += val;
		//如果滑动窗口超过规定大小
		if(nums.size()>capacity) {
			//移除队列中队首元素
			sum-=nums.poll();
		}
		//返回平均值
		return (double) sum/nums.size();
	}
}
```
## 时间复杂度
O（1）
## 空间复杂度
O（n），n是capacity的大小



