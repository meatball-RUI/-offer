# 二叉搜索树的下一个节点
## TreeNode
```java
package offer53;

public class TreeNode {
	TreeNode left;
	TreeNode right;
	int val;
	public TreeNode(int val) {
		this.val=val;
	}
}
```
## 时间复杂度O（n），空间复杂度O（h）的解法  
思路：先找到目标节点p，然后确定按中序排序的后继节点
### InorderSuccessorStack 解法
```java
package offer53;

import java.util.Stack;

public class InorderSuccessorStackOffer {
	public TreeNode inorderSuccessorStackOffer(TreeNode root,TreeNode p) {
		Stack<TreeNode> stack=new Stack<>();
		boolean found=false;
		TreeNode cur=root;
		while(cur!=null||!stack.isEmpty()) {
			while(cur!=null) {
				stack.push(cur);
				cur=cur.left;
			}
		}
		TreeNode node=stack.pop();
		if(found) {
			return cur;
		}
		if(cur==p) {
			found=true;
		}
		cur=cur.right;
		return null;
	}
}

```
### InorderSuccessorRecursion解法
```java
package offer53;

public class InorderSuccessorRecursion {
	public TreeNode inorderSuccessorRecursion(TreeNode root, TreeNode p) {
		boolean[] found= {false};
		return inorder(root,p,found);
	}

	private TreeNode inorder(TreeNode root, TreeNode p,boolean[] found) {
		if(root==null) {
			return null;
		}
		TreeNode leftResult=inorder(root.left,p,found);
		if(leftResult!=null) {
			return leftResult;
		}
		if(found[0]) {
			return root;
		}
		if(root==p) {
			found[0]=true;
		}
		TreeNode rightResult=inorder(root.right,p,found);
		if(rightResult!=null) {
			return rightResult;
		}
		return null;
	}
}
```
### Main
```java
package offer53;

public class Main {
	public static void main(String[] args) {
		TreeNode node1=new TreeNode(8);
		TreeNode node2=new TreeNode(6);
		TreeNode node3=new TreeNode(10);
		TreeNode node4=new TreeNode(5);
		TreeNode node5=new TreeNode(7);
		TreeNode node6=new TreeNode(9);
		TreeNode node7=new TreeNode(11);
		node1.left=node2;
		node1.right=node3;
		node2.left=node4;
		node2.right=node5;
		node3.left=node6;
		node3.right=node7;
//		InorderSuccessorStack iss=new InorderSuccessorStack();
//		System.out.print(iss.inorderSuccessorStack(node1,node3).val);
//		InorderSuccessorStackOffer isso=new InorderSuccessorStackOffer();
//		System.out.print(isso.inorderSuccessorStackOffer(node1,node3).val);
		InorderSuccessorRecursion isr=new InorderSuccessorRecursion();
		System.out.print(isr.inorderSuccessorRecursion(node1, node3).val);
	}
}
```
## 时间复杂度O（h), 空间复杂度O（1）的解法
思路：将遍历的节点与目标节点相比较，后继节点比目标节点大（如果遍历的节点小，往右子树走），又是比目标节点大的节点中的最小的节点（找到比目标节点大的节点之后，再往左子节点走，直到找到比目标节点大的节点中的最小的节点）
