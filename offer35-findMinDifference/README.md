# 最小时间差
给定一组范围在00:00至23:59的时间，求任意两个时间之间的最小时间差。  
eg.输入时间组["23:50","23:59","00:00"]，输出为1.
最小时间差为23:59到00:00之间的时间差
## 思路
### 思路1  
这个题目最直观的解法是求出任意两个时间的间隔，然后比较得出最小的时间差。如果输入n个时间，那么需要计算工每个时间一另外n-1个时间的间隔。这种蛮力法需要O(n^2)的时间
### 思路2
对思路1进行优化  
把n个时间排序。排序之后只需要计算两相邻的时间之间的间隔，这样就只需要计算O(n)个时间差。由于对n个时间进行排序通常需要O(nlogn)的时间，因此这种优化算法的总体时间复杂度是O（nlogn）。  
特殊情况：【"23:50","23:59","00:00"】排序，得到【"00:00"，"23:50","23:59"】。  
时间00:00和23:50之间的间隔是1430分钟，而23:50和23:59之间的间隔是9分钟。由于排序之后的第1个时间00:00也可能是第2天的00:00，它和前一天的23:59之间的间隔只有1分钟。也就是说，在计算最小时间差时，需要把排序之后的第1个时间当作第2天的时间（即加上24小时）与最后一个时间之间的间隔也考虑进去。
### 思路3
对思路2的进一步优化   
用数组模拟一个键为时间、值为true或false的哈希表。  
可以用数组模拟哈希表的原因是一天的分钟数是已知的，而且数组的长度为1440，也不算太长。有了这个数组，就可以和用哈希表一样，在O（1）的时间知道每个时间是否出现在输入的时间数组中。
```java
package offer35;

import java.util.List;

public class Solution {
	//创建一个方法用来找最少差别
	public int findMinDifference(List<String> timePoints){
		//如果timePoints.size()大于1440，说明这里面至少有两个时间点是一样的，所以返回0
		  if(timePoints.size()>1440){
		    return 0;
		  }
		  //创建一个模拟hash表的数组，键为时间，值为true或false；
		  boolean minuteFlags[] = new boolean[1440];
		  //遍历timePoints中的每个时间
		  for(String time: timePoints){
		//将每个时间用split方法分成包含小时跟分钟的数组
		    String t[]=time.split(":");
		//将时间计算成分钟来表示
		    int min=Integer.parseInt(t[0])*60+Integer.parseInt(t[1]);
		//如果minuteFlags[min]里之前已经有值了，就返回0，因为说明有一样的值，这时候最少差别肯定是0
		    if(minuteFlags[min]){
		      return 0;
		    }
		//将这个时间对应的value标成true
		    minuteFlags[min]=true;
		    }
		//如果最小差值不是0的话，用helper方法来计算最小差值
		    return helper(minuteFlags);
		  }
	private int helper(boolean minuteFlags[]){
		int minDiff=minuteFlags.length-1;
		int prev=-1;
		int first=minuteFlags.length-1;
		int last=-1;
		for(int i=0;i<minuteFlags.length;++i) {
			if(minuteFlags[i]) {
				if(prev>=0) {
					minDiff=Math.min(i-prev, minDiff);
				}
				prev=i;
				first=Math.min(i, first);
				last=Math.max(i, last);
			}
		}
		minDiff = Math.min(first+minuteFlags.length-last,minDiff);
		return minDiff;
	}
}
```
### 不熟的点1
```java
String t[]=time.split(":");
```
split method in String:  
String[] 	split(String regex)
Splits this string around matches of the given regular expression.
String[] 	split(String regex, int limit)
Splits this string around matches of the given regular expression.
split() 方法根据匹配给定的正则表达式来拆分字符串。
*返回的是String类型的数组*
注意： . 、 $、 | 和 * 等转义字符，必须得加 \\。
注意：多个分隔符，可以用 | 作为连字符。  
eg:
```java
public class Test {
    public static void main(String args[]) {
        String str = new String("Welcome-to-Runoob");
        System.out.println("- 分隔符返回值 :" );
        for (String retval: str.split("-")){
            System.out.println(retval);
        }
 
        System.out.println("");
        System.out.println("- 分隔符设置分割份数返回值 :" );
        for (String retval: str.split("-", 2)){
            System.out.println(retval);
        }
 
        System.out.println("");
        String str2 = new String("www.runoob.com");
        System.out.println("转义字符返回值 :" );
        for (String retval: str2.split("\\.", 3)){
            System.out.println(retval);
        }
 
        System.out.println("");
        String str3 = new String("acount=? and uu =? or n=?");
        System.out.println("多个分隔符返回值 :" );
        for (String retval: str3.split("and|or")){
            System.out.println(retval);
        }
    }
}
```
result:  
- 分隔符返回值 :  
Welcome  
to  
Runoob  

- 分隔符设置分割份数返回值 :  
Welcome  
to-Runoob  

转义字符返回值 :  
www  
runoob  
com  

多个分隔符返回值 :  
acount=?   
 uu =?   
 n=?  
 <br/>
 所以我们的这个case中是将当前时间返回为小时跟分钟的数组  
### 不熟的点2
```java
int min=Integer.parseInt(t[0])*60+Integer.parseInt(t[1]);
```
static int 	parseInt(String s)
Parses the string argument as a signed decimal integer.
将字符串参数解析为有符号十进制整数。
static int 	parseInt(String s, int radix)
Parses the string argument as a signed integer in the radix specified by the second argument.
将字符串参数解析为第二个参数指定的基数中的有符号整数
所以在这边的这个case中是将小时和分钟转换成字符串整数

##参考一下ChatGPT
在写注解的时候，不能够自己理解辅助函数
```java
package offer35;

import java.util.List;

public class ChatGPT {
	//主函数：接收一个时间点列表，返回最小时间差
	public int findMinDifference(List<String> timePoints) {
		//如果时间点数量超过1440（一天内总分钟数），说明必然有重复时间点，直接返回0
		if(timePoints.size()>1440) {
			return 0;
		}
		//创建一个布尔数组，用来记录每一分钟是否存在时间点
		boolean[] minuteFlags=new boolean[1440];
		//遍历每一个时间点，将其转换为分钟数并存储在布尔数值中
		for(String time:timePoints) {
			//将时间点从“HH：MM”格式拆分为小时和分钟
			String[] t=time.split(":");
			//将小时和分钟转换为总分钟数
			int minutes=Integer.parseInt(t[0])*60+Integer.parseInt(t[1]);
		    //如果该分钟数已经存在，说明有重复时间点，返回0
			if(minuteFlags[minutes]){
			      return 0;
			    }
			//在布尔数组中标记该分钟数对应的位置为true
			    minuteFlags[minutes]=true;
			    }
			//调用辅助函数计算最小时间差
			    return helper(minuteFlags);
			  }
		//辅助函数：计算布尔数组中相邻时间点的最小差值
		private int helper(boolean minuteFlags[]){
			//初始化最小时间差为最大可能值（1440分钟）
			int minDiff=minuteFlags.length;
			//变量prev用来记录前一个时间点的分钟数
			int prev=-1;
			//记录第一个时间点的位置
			int first=minuteFlags.length;
			//记录最后一个时间点的位置
			int last=-1;
			for(int i=0;i<minuteFlags.length;++i) {
				if(minuteFlags[i]) {
					if(prev>=0) {
						minDiff=Math.min(i-prev, minDiff);
					}
					prev=i;
					first=Math.min(i, first);
					last=Math.max(i, last);
				}
			}
			minDiff = Math.min(first+minuteFlags.length-last,minDiff);
			return minDiff;
		}
	}		

```
