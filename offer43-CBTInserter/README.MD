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

### 有注释code
```java
package offer43;

import java.util.LinkedList;
import java.util.Queue;

public class CBTInserter {
	//使用队列来维护树节点，便于插入时找到下一个合适的父节点
	private Queue<TreeNode> queue;
	private TreeNode root;
	
	//构造函数，传入完全二叉树的根节点
	public CBTInserter(TreeNode root) {
		this.root=root;
		queue=new LinkedList<>();
		//根节点加入队列
		queue.offer(root);
		//初始化队列，将完全二叉树中左右子节点都满的节点移出队列
		//直到找到一个缺少左右子节点的节点为止
		while(queue.peek().left!=null&&queue.peek().right!=null) {
			//移出左右子节点都满的节点
			TreeNode node=queue.poll();
			//左右子节点加入队列
			queue.offer(node.left);
			queue.offer(node.right);
		}
	}
	//插入一个新节点，返回新节点的父节点的值
	public int insert(int v) {
		//获取队列中下一个可以作为父节点的节点
		TreeNode parent=queue.peek();
		//创建新节点
		TreeNode node=new TreeNode(v);
		//如果当前父节点的左子节点为空，则将新节点插入左子节点
		if(parent.left==null) {
			parent.left=node;
		}else {
			//否则插入右子节点，并将父节点移出队列
			parent.right=node;
			//父节点已满，移出队列
			queue.poll();
			//新插入的左子节点进入队列
			queue.offer(parent.left);
			//新插入的右子节点进入队列
			queue.offer(parent.right);
		}
		//返回父节点的值
		return parent.val;
	}
	//获取根节点
	public TreeNode get_Root() {
		return this.root;
	}
}
```
