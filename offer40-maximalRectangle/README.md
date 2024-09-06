# 矩阵中的最大矩形
请在一个由0、1组成的矩阵中找出最大的只包含1的矩形并输出它的面积。例如，在图6.6的矩阵中，最大的只包含1的矩阵如阴影部分所示，它的面积是6.  
<img src="https://github.com/meatball-RUI/DataStructure-and-Algorithm-Offer/blob/main/offer40-maximalRectangle/Screenshot%202024-09-05%20at%2019.07.41.png" width="350px">  
## 思路
以矩阵中的每行为基线，将矩阵转换成多个直方图  
<img src="https://github.com/meatball-RUI/DataStructure-and-Algorithm-Offer/blob/main/offer40-maximalRectangle/Screenshot%202024-09-05%20at%2019.30.42.png" width="350px">

### Code
```java
package offer40;

public class Solution {
	public int maximalRectangle(char[][] matrix) {
		if(matrix.length==0||matrix[0].length==0) {
			return 0;
		}
		int maxArea=0;
		int[] heights=new int[matrix[0].length];
		for(char[] row:matrix) {
			for(int i=0;i<row.length;i++) {
				if(row[i]=='0') {
					heights[i]=0;
				}else {
					heights[i]++;
				}
			}
			maxArea=Math.max(maxArea, LargestRectangle.largestRectangle(heights));
		}
		return maxArea;
	}
}
```
### 错题总结
```java
if (row[i] == '0') {
	heights[i] = 0;  // 遇到 '0'，当前柱子的高度重置为 0
}
```
二维数组中存放的char型的数据，不是int型的，所以比较的时候是row[i]=='0'不是row[i]==0;

### 时间空间
