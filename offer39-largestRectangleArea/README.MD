# 直方图最大矩形面积
## 题目
直方图是由排列在同一基线上的相邻柿子组成的图形。输入一个由非负数组成的数组，数组中的数字是直方图中柱子的高。求直方图中最大矩形面积。假设直言方图中柱子的宽都为1.例如，输入数组[2,1,5,6,2,3]，其对应的直方图如图所示，该直方图中最大矩形面积为12，如阴影部分所示  
<img src="https://github.com/meatball-RUI/DataStructure-and-Algorithm-Offer/blob/main/offer39-largestRectangleArea/Screenshot%202024-09-03%20at%2011.15.47.png" width="350px">
<img src="https://github.com/meatball-RUI/DataStructure-and-Algorithm-Offer/blob/main/offer39-largestRectangleArea/histogram_area.png" width="350px">
## 解决方法
### 蛮力法
如果能逐一找出直方图中所有的矩形并比较它们的面积，就能得到最大的矩形面积。下面使用嵌套的二重循环遍历所有矩形，并比较它们的面积，参考代码如下所示
```java
package offer39;
public class Solution1 {
	// Function to calculate the largest rectangle area in a histogram
	// 计算直方图中最大矩形面积的函数
	public int largestRectangleArea(int[] heights) {
		//Variable to store the maximum area found
		//用于存储找到的最大面积的变量
		int maxArea=0;
		//Outer loop to consider each bar as the left boundary of the rectangle
		//外层循环，将每个柱子作为矩形的左边界
		for(int i=0;i<heights.length;i++) {
			
			//Initialize the minimum height to the current bar
			//将最小高度初始化为当前柱子的高度
			int min=heights[i];
			
			//Inner loop to consider each bar from 'i' as the right boundary
			//内层循环，将从‘i’开始的每个柱子作为右边界
			for(int j=i;j<heights.length;j++) {
				//Update the minimum height between the bars from 'i' to 'j'
				//更新从‘i’到‘j’之间柱子的最小高度
				min=Math.min(min, heights[j]);
				//Calculate the area with 'i' as the left boundary and 'j' as the right boundary
				//计算以‘i’为左边界，‘j’为右边界的矩形面积
				int area=min*(j-i+1);
				//Update the maximum area if the current area is larger
				//如果当前面积最大，则更新最大面积
				maxArea=Math.max(maxArea, area);
			}
		}
		//Return the maximum area found
		//返回找到的最大面积
		return maxArea;
	}
}
```
#### 时间空间复杂度
时间复杂度O（n^2）,空间复杂度O（n）

### 分治法
使用分治法(Divide and Conquer)来解决最大矩形面积的问题是一种更高效的算法。分治法的核心思想是将问题递归地分成更小的子问题进行解决，然后将子问题的解组合成原问题的解。   
分治法解决最大矩形面积的步骤：   
1. 找出最小高度的柱子   
* 在给定的范围内找到最矮的柱子（高度最小的柱子）。这个柱子决定了以它为高度的矩形的最大宽度。  
* 因为这个柱子将直方图分成了两部分，左边部分和右边部分  
2. 递归地计算左半部分和右半部分的最大矩形面积  
* 左半部分的最大矩形面积可以通过递归调用相同的函数来计算    
* 右半部分的最大矩形面积也可以通过递归调用相同的函数来计算   
3. 结合结果：  
* 最后的最大矩形面积是三者中的最大值  
1.通过最矮柱子计算得到矩形的面积  
2.左半部分的最大矩形面积    
3.右半部分的最大矩形面积   
代码实现：  
```java
package offer39;

public class ConquerAndDivideRUI1{
	//用来计算矩形最大值的函数
	public int largestRectangle(int[] heights) {
		//调用辅助方法用来计算所有范围内的最大矩形面积值
		return helper(heights,0,heights.length);
	}
	//递归辅助函数，用来计算子数组中的最大矩形面积值
	private int helper(int[] heights, int start, int end) {
		//遍历数组，找到最小高度的柱子的索引
		int minIndex=start;
		for(int i=0;i<heights.length;i++) {
			if(heights[i]<heights[minIndex]) {
				minIndex=i;
			}
		}
		//递归计算左半部分最大矩形值
		int areaMinIndex=minIndex*(start-end+1);
		//递归计算左半部分最大矩形值
		int areaLeft=helper(heights,start,minIndex-1);
		//递归计算右半部分最大矩形值
		int areaRight=helper(heights,minIndex+1,end);
		return Math.max(areaMinIndex, Math.max(areaLeft, areaRight));
	}
}

```
#### 时间空间复杂度
##### 时间复杂度  
* 假设输入数组的长度为n。如果每次都能将n根柱子分成两根柱子数量为n/2的直方图，那么递归调用的深度为O（logn），整个分治法的时间复杂度是O（nlogn）  
* 但如果直方图中柱子的高度是排序的（递增排序或递减排序），那么每次最矮的柱子都位于直方图的一侧，递归调用的深度就是O（n），此时分治法的时间复杂度也变成O（n^2)  
###### 空间复杂度  
1.递归调用栈的深度：   
* 在最坏情况下，递归调用的深度会达到 O(n)，这就是空间复杂度的主要来源。  
* 每一层递归调用会占用 O(1) 的空间，但由于调用栈的深度可能达到 O(n)，所以总的空间复杂度为 O(n)。    
2.额外的空间使用：  
* 除了递归调用栈外，算法在每次递归中只使用了常量级别的额外空间（例如变量minIndex、area、left、right），所以不影响总体空间复杂度。   

综上所述：
* 时间复杂度：O（^2)
* 空间复杂度：O（n）
该算法的时间复杂度较高，特别是在最坏情况下，效率会比较低。这也是为什么通常会使用更高效的算法（例如基于栈的线性时间算法）来解决这个问题。


### 单调栈法
#### 解题思路
1. 定义问题：  
* 给定一个柱状图，每个柱子的高度由数组heights表示，求在这个柱状图中能形成的最大矩形的面积  
2. 使用单调栈：  
* 单调栈的概念：使用栈来维护柱子的索引，使用栈中的柱子的高度是单调递增的。这样可以方便地计算每个柱子作为最小高度时的最大矩形面积。  
3. 处理柱子：  
* 遍历柱状图的每个柱子时，将柱子的索引压入栈中。  
* 在压入新柱子之前，比较栈顶柱子的高度与当前柱子的高度。如果当前柱子的高度较小，弹出栈顶元素，并计算以栈顶柱子的高度为基准的矩形的面积。更新最大矩形面积。  
4. 计算矩形面积  
* 当弹出栈顶柱子时，计算该柱子作为最小高度的矩形的面积。矩形的宽度是从栈中下一个柱子的索引到当前柱子的索引之间的距离。  
5. 处理剩余柱子  
* 遍历结束后，栈中可能还有未处理的柱子。继续弹出栈顶的柱子，计算其矩形面积。这些柱子的宽度会是从当前柱子的索引到柱状图的末尾。

#### 详细步骤
1. 初始化：  
* 创建一个栈用于存储柱子的索引
* 在栈中先推入一个特殊值-1，作为栈的虚拟左边办。
2. 遍历柱状图
* 对于每个柱子，进行如下操作：  
	*  如果栈不为空且当前柱子的高度小于或等于栈顶柱子的高度，则弹出栈顶柱子的索引，并计算以该柱子为最小高度的矩形面积  
	*  将当前柱子的索引推入栈中。  
3. 处理栈中剩余的柱子：
* 在遍历结束后，栈中仍有柱子未处理。弹出这些柱子的索引，计算其对应的矩形面积  
4. 计算面积：  
* 计算每个弹出的柱子的矩形面积时，宽度是当前柱子的索引减去栈顶柱子的索引减一  

#### 示例  
假设柱状图的高度数组为【2，1，5，6，2，3】：  
步骤 1: 初始化  
* 初始化一个栈并将 -1 推入栈中，作为虚拟的左边界。  
* stack = [-1]  
* maxArea = 0  
步骤 2: 遍历柱状图  
1. 柱子高度为2（索引0）
* 将索引0推入栈中。  
* stack = [-1,0]  
2. 柱子高度为1（索引1）  
* 当前柱子的高度1小于栈顶柱子的高度2.
* 弹出栈顶元素0，计算以高度2为基准的矩形面积
* 宽度=1【右边界，当前遍历到的元素的索引】-（-1）【栈顶元素的前一个元素索引】-1=1
* 面积=2*1=1
3. 处理柱子高度为5（索引2）：
* 将索引2推入栈中，栈为【-1，1，2】。
4. 处理柱子高度为6（索引3）：
* 将索引3推入栈中，栈为【-1，1，2，3】。
5.处理柱子高度为2（索引4）：  
* 当前柱子的高度2小于栈顶柱子的高度6，弹出栈顶元素3，并计算以高度6为基准的矩形面积。
* 高度：栈顶元素对应的高度是6.
* 宽度：宽度计算为4-2-1=1，此时stack.peek()是2.
* 面积：6*1=6  
接着，继续弹出栈顶元素2，并计算心高度5为基准的矩形面积：
* 高度：栈顶元素对应的高度是5.  
* 宽度：宽度计算4-1-1=2，此时stack.peek()是1.
* 面积： 5 * 2=10





