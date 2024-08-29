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
		  //timePoints
		  for(String time: timePoints){
		    String t[]=time.split(":");
		    int min=Integer.parseInt(t[0])*60+Integer.parseInt(t[1]);
		    if(minuteFlags[min]){
		      return 0;
		    }
		    minuteFlags[min]=true;
		    }
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
###不熟的点
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

```java
int min=Integer.parseInt(t[0])*60+Integer.parseInt(t[1]);
```
