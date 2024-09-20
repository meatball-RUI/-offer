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
## 为什么要在这道题中用hashmap
在这道题中使用HashMap是为了高效地存储和查找路径和，以便快速计算出从任意节点到当前节点的路径是否满足给定的和sum。具体原因如下：
1.路径和的快速查找
这道题要求找到从任意节点到任意节点的路径，且路径和等于目标值sum。为了快速找到这些路径，HashMap被用来存储前缀路径和（从根节点到当前节点的路径和），并在递归时进行如下操作：  
** 存储路径和：在遍历树时，计算从根节点到当前节点的路径和，并将其存储在map中。  
** 快速查找前缀和：对于当前节点path，我们需要找出是否存在某个祖先节点，其路径和等于path-sum，如果存在，那么从这个祖先节点到当前节点的路径和就等于sum。利用HashMap可以快速查找这个值。  
例子说明：  
假设从根节点到某个节点X的路径和是pathX，从根节点到另一个节点Y的路径和是pathY，并且pathY-pathX=sum，那么从节点X到节点Y的路径和就是sum
通过HashMap，我们可以在常数时间内检查path-sum是否已经存在，也就是找到是否有一条路径和正好等于sum的路径。  
2.避免重复计算  
如果不使用HashMap，我们可能需要遍历每一个节点，然后再从每个节点出发计算其到所有子节点的路径和，这样的做法会导致重复计算，从而使时间复杂度变高（可能达到O（n^2)）。  
使用HashMap可以记录路径和的出现次数，避免重复计算。例如，当你递归遍历到某个节点时，已经记录的路径和可以帮助你快速判断从根节点到当前节点的所有路径是否有满足条件的。  
3.实现回溯  
在递归遍历时，HashMap还起到回溯的作用。每次递归进入一个节点时，我们会把当前路径和加入到HashMap中，当递归离开这个节点时（即回溯），要将这个路径从HashMap中移除，以便不影响其他路径的计算。这一步使得每个分支都能独立地进行路径和的计算。  
详细流程（结合HashMap）
1.初始化：我们使用一个HashMap<Integer,Integer>，其中键是从根节点到当前节点的路径和，值是该路径和出现的次数。
* 初始化时，map.put(0,1)。这个是为了处理从根节点到当前节点路径和正好等于sum的情况  
2.递归遍历：当我们递归到每个节点时：  
* 计算从根节点到当前节点的路径和path  
* 查找path-sum是否存在于HashMap中。如果存在，说明有一条从之前某个节点到当前节点的路径和等于sum
* 将当前路径和path记录到HashMap中。
3.递归处理左右子树：依次处理左右子树的递归  
4.回溯：递归返回父节点时，要将当前节点的路径从HashMap中移除，以免影响其他分支的计算。  
总结： 
HashMap在这道题中的作用是为了高效地存储和查找路径和，它使得：
* 查找某个路径和的时间复杂度从O（n）降到O（1），提高了算法效率。  
* 能够通过存储前缀路径和，快速找到符合条件的路径，避免重复计算。  
* 结合递归和回溯，确保我们不会重复计算同一条路径。  




