# 最小时间差
## 题目
输入一个数组，它的每个数字是某天的温度。请计算需要等几天才会出现更高的温度。例如，如果输入数组【35，31，33，36，34】，那么输出为【3，1，1，0，0】。由于第1天的温度是35°C，要等3天才会出现更高的温度36°C，后面没有更高的温度，它对应的输出是0。其他的以此类推。
## 思路
解决这个问题的思路总结起来就是用一个栈保存每天的温度在数组中的下标。每次从数组中读取一个温度，然后将其与栈中保存的温度（根据下标可以得到温度）进行比较。如果当前温度比位于栈顶的温度高，那么就能知道位于栈顶那一天需要等待几天才会出现更高的温度。然后出栈1次，将当前温度与一个位于栈顶的温度进行比较。如果栈中已经没有比当前温度低的温度，则将当前温度在数组中的下标入栈。
## code
```java
package offer38;

import java.util.Stack;

public class Offer {
	//定义一个方法dailyTemperatures，接受一个整型数组temperatures作为参数，返回一个整型数组
	public int[] dailyTemperatures(int[] temperatures) {
		//创建一个输入数组temperatures长度相同的结果数组result，默认初始化为0
		int[] result = new int[temperatures.length];
		//创建一个栈，用于存储温度数组中的索引
		Stack<Integer> stack=new Stack<>();
		//遍历温度数组中的每一个温度
		for(int i=0;i<temperatures.length;i++) {
			//当栈不为空并且当前温度大于栈顶索引对应的温度时，表示找到了栈顶索引对应的那一天的下一个更暖和的日子
			while(!stack.empty()&&temperatures[i]>temperatures[stack.peek()]) {
				//弹出栈顶元素，得到一个需要计算等待天数的索引
				int prev=stack.pop();
				//计算当前温度与前一个温度之间的天数差，并将其存入结果数组中
				result[prev]=i-prev;
			}
			//将当前温度索引压入栈中
			stack.push(i);
		}
		//返回结果数组
		return result;
	}
}
```

