# 向下的路径节点值之和
```java
package offer50;

import java.util.HashMap;
import java.util.Map;

public class PathSumRUI {
	public int pathSum(TreeNode root,int sum) {
		//创建一个hashmap用来映射根节点到当前节点的path和出现的次数
		Map<Integer,Integer> map=new HashMap<>();
		//初始化一个hashmap
		map.put(0, 1);
		//调用dfs方法计算count的值
		return dfs(root,sum,map,0);
	}
	private int dfs(TreeNode root, int sum, Map<Integer, Integer> map, int path) {
		//如果满足当前root节点为null，返回0
		if(root==null) {
			return 0;
		}
		//计算根节点到当前节点的值
		path+=root.val;
		//判断是否存在从某个节点到当前节点的path和为sum的情况,计算从根节点到当前节点符合条件的路程
		int count=map.getOrDefault((path-sum), 0);
		//更新map的映射情况
		map.put(path, map.getOrDefault(path, 0)+1);
		//递归遍历左子节点并且跟之前确定的count数量进行累加
		count+=dfs(root.left,sum,map,path);
		//递归遍历右子节点并且跟之前确定的count数量进行累加
		count+=dfs(root.right,sum,map,path);
		//回溯，当前节点递归完返回到父节点，并将当前节点的所对应的path和从map中清掉，不影响其他路径的count计算
		map.put(path, map.get(path)-1);
		return count;
	}
}
```

