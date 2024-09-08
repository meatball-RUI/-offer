## 错题集
``` java
package offer43;

import java.util.LinkedList;
import java.util.Queue;

public class CBTInserterRUI {
	private Queue<TreeNode> queue;
	TreeNode root;
	public CBTInserterRUI(TreeNode root) {
		queue=new LinkedList<>();
		this.root=root;
		queue.offer(root);
		while(queue.peek().left!=null&&queue.peek().right!=null) {
			TreeNode node=queue.poll();
			queue.offer(node.left);
			queue.offer(node.right);
		}
	}
	public TreeNode insert(int val) {
		TreeNode parent=queue.peek();
		TreeNode newNode=new TreeNode(val);
		if(parent.left==null) {
			parent.left=newNode;
		}else {
			parent.right=newNode;
		}
		queue.poll();
		queue.offer(newNode);
		return parent;
	}
	public TreeNode getRoot() {
		return this.root;
	}
}
```
### 我的错误点
``` java
if(parent.left==null) {
	parent.left=newNode;
}else {
	parent.right=newNode;
}
	queue.poll();
	queue.offer(newNode);
	return parent;
```
当parent.left=newNode的时候还不能从queue中移出队列，因为它的父节点还符合能添加子节点的条件
因为它的父节点还符合能添加子节点的条件，所以它的子节点也能加入到queue中，子节点还不符合能添加子节点的条件
