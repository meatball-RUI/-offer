# Code
``` java
package offer42;

import java.util.LinkedList;
import java.util.Queue;

public class RecentCounterRUI {
	//用来存储时间戳的队列
	private Queue<Integer> times;
	//用来存储窗口大小
	private int windowSize;
	
	//初始时间队列跟窗口大小
	public RecentCounterRUI() {
		times = new LinkedList<>();
		windowSize = 3000;
	}
	//接收一个新的时间戳，并返回在过去3000ms内的请求数
	public int ping(int t) {
		//将新的时间戳加入到队列中
		times.offer(t);
		//判断队列中是不是有时间戳，不在当前的时间窗口内
		while (times.peek() + windowSize < t) {
			//弹出不在当前窗口内的时间戳
			times.poll();
		}
		//返回队列中时间戳的数量，即过去3000ms内的请求数
		return times.size();
	}
}
```
## 时间复杂度
假设计数器时间窗口的大小是w毫秒，其中记录的时间是递增的，那么时间窗口中记录的时间的数目是O（w），因此空间复杂度是O（w）。每当收到一个新的请求ping时，由于可能需要删除O（w）个已经演出时间窗口的请求，因此时间复杂度也是O（w）。但由于这个题目中时间窗口的大小都为3000ms，w是一个常数，因此也可以认为时间复杂度和空间复杂度都是O（1）

