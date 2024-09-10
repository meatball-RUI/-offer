# 错题集
```java
package offer45;

import java.util.LinkedList;
import java.util.Queue;

public class FindBottomLeftValue {
	public int findBottomLeftValue(TreeNode root) {
		Queue<TreeNode> queue1=new LinkedList<TreeNode>();
		Queue<TreeNode> queue2=new LinkedList<TreeNode>();
		int bottomLeft;
		if(root!=null) {
			queue1.offer(root);
			bottomLeft=root.val;
		}
		while(!queue1.isEmpty()) {
			TreeNode node=queue1.poll();
			if(node.left!=null) {
				queue2.offer(node.left);
			}
			if(node.right!=null) {
				queue2.offer(node.right);
			}
			if(queue1.isEmpty()) {
				queue1=queue2;
				queue2=new LinkedList<>();
				bottomLeft=queue1.peek().val;
			}
		}
		return bottomLeft;
	}
}
```
```java
这个题目里面已经假设至少会存在一个节点了，所以不用再单独考虑root！=null的情况
```
