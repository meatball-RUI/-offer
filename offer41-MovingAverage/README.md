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

import java.util.LinkedList;
import java.util.Queue;

public class MovingAverageRUI {
	private Queue<Integer> window;
	private int capacity;
	private int sum;
	public MovingAverageRUI(int capacity) {
		window=new LinkedList<>();
		this.capacity=capacity;
		sum=0;
	}
	public double next(int val) {
		window.offer(val);
		sum+=val;
		if(window.size()>capacity) {
			sum-=window.poll();
		}
		return (double)sum/window.size();
	}
}
```



