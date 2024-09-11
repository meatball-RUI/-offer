# 二叉树的右侧视图
## 一个队列的解法
``` java
package offer46;

import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class OneQueue {
	public List<Integer> rightSightView(TreeNode root){
		List<Integer> view=new LinkedList<>();
		if(root==null) {
			return view;
		}
		Queue<TreeNode> queue=new LinkedList<>();
		queue.offer(root);
		while(!queue.isEmpty()) {
			int size=queue.size();
			TreeNode rightMost=null;
			for(int i=0;i<size;i++) {
				TreeNode node=queue.poll();
				rightMost=node;
				if(node.left!=null) {
					queue.offer(node.left);
				}
				if(node.right!=null) {
					queue.offer(node.right);
				}
			}
			view.add(rightMost.val);
		}
		return view;
	}
}
```
## 两个队列的解法
```java
package offer46;

import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class TwoQueues {
	public List<Integer> rightSideView(TreeNode root){
		Queue<TreeNode> queue1=new LinkedList<>();
		Queue<TreeNode> queue2=new LinkedList<>();
		List<Integer> view=new LinkedList<>();
		if(root==null) {
			return view;
		}
		queue1.offer(root);
		while(!queue1.isEmpty()) {
			TreeNode node=queue1.poll();
			if(node.left!=null) {
				queue2.offer(node.left);
			}
			if(node.right!=null) {
				queue2.offer(node.right);
			}
			if(queue1.isEmpty()) {
				view.add(node.val);
				queue1=queue2;
				queue2=new LinkedList<>();
			}
		}
		return view;
	}
}
```
