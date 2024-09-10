# 二叉树中每层的最大值
## 一个队列实现二叉树的广度优先搜索
``` java
package offer44;

import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class LargestValuesRUI2 {
	public List<Integer> largestValuesRUI2(TreeNode root){
		int current=0;
		int next=0;
		int max=Integer.MIN_VALUE;
		Queue<TreeNode> queue=new LinkedList<>();
		List<Integer> result=new LinkedList<>();
		if(root!=null) {
			queue.offer(root);
			current++;
		}
		while(!queue.isEmpty()) {
			TreeNode node=queue.poll();
			max=Math.max(max, node.val);
			current--;
			if(node.left!=null) {
				queue.offer(node.left);
				next++;
			}
			if(node.right!=null) {
				queue.offer(node.right);
				next++;
			}
			if(current==0) {
				result.add(max);
				current=next;
				next=0;
				max=Integer.MIN_VALUE;
			}
		}
		return result;
	}
}
```
## 二个队列实现二叉树的广度优先搜索
``` java
package offer44;

import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class TwoQueueRUI {
	public List<Integer> largestValues(TreeNode root){
		Queue<TreeNode> queue1=new LinkedList<TreeNode>();
		Queue<TreeNode> queue2=new LinkedList<TreeNode>();
		int max=Integer.MIN_VALUE;
		List<Integer> result=new LinkedList<Integer>();
		if(root!=null) {
			queue1.offer(root);
		}
		while(!queue1.isEmpty()) {
			TreeNode node=queue1.poll();
			max=Math.max(max, node.val);
			if(node.left!=null) {
				queue2.offer(node.left);
			}
			if(node.right!=null) {
				queue2.offer(node.right);
			}
			if(queue1.isEmpty()) {
				result.add(max);
				queue1=queue2;
				queue2=new LinkedList<>();
				max=Integer.MIN_VALUE;
			}
		}
		return result;
	}
}
```
