# 展平二叉树
## TreeNode
```java
package offer52;

public class TreeNode {
	TreeNode left;
	TreeNode right;
	int val;
	public TreeNode(int val) {
		this.val=val;
	}
}
```
## BFS
```java
package offer52;

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

public class BFS {
	public List<Integer> bfs(TreeNode root){
		Queue<TreeNode> queue=new LinkedList<>();
		List<Integer> result=new ArrayList<>();
		queue.offer(root);
		while(!queue.isEmpty()) {
			TreeNode node=queue.poll();
			result.add(node.val);
			if(node.left!=null) {
				queue.offer(node.left);
			}
			if(node.right!=null) {
				queue.offer(node.right);
			}
		}
		return result;
	}
}
```
## IncreasingBSTRecursion 解法
```java
package offer52;

public class IncreasingBSTRecursion {
	private TreeNode prev=null;
	public TreeNode increasingBTSRecursion(TreeNode root) {
		TreeNode dummy=new TreeNode(-1);
		prev=dummy;
		inorder(root);
		return dummy.right;
	}
	private void inorder(TreeNode root) {
		if(root==null) {
			return;
		}
		inorder(root.left);
		prev.right=root;
		root.left=null;
		prev=root;
		inorder(root.right);
	}
}
```
## IncreasingBSTStack 解法
```java
package offer52;

import java.util.Stack;

public class IncreasingBSTStack {
	public TreeNode increasingBSTStack(TreeNode root) {
		Stack<TreeNode> stack=new Stack<>();
		TreeNode prev=null;
		TreeNode cur=root;
		TreeNode first=null;
		if(cur==null) {
			return null;
		}
		while(cur!=null||!stack.isEmpty()) {
			while(cur!=null) {
				stack.push(cur);
				cur=cur.left;
			}
			cur=stack.pop();
			if(prev!=null) {
				prev.right=cur;
			}else {
				first=cur;
			}
			cur.left=null;
			prev=cur;
			cur=cur.right;
		}
		return first;
	}
}
```
## Main
```java
package offer52;

import java.util.List;

public class Main {

	public static void main(String[] args) {
		TreeNode node1=new TreeNode(4);
		TreeNode node2=new TreeNode(2);
		TreeNode node3=new TreeNode(5);
		TreeNode node4=new TreeNode(1);
		TreeNode node5=new TreeNode(3);
		TreeNode node6=new TreeNode(6);
		node1.left=node2;
		node1.right=node3;
		node2.left=node4;
		node2.right=node5;
		node3.right=node6;
		IncreasingBSTStack ibs=new IncreasingBSTStack();
		TreeNode node=ibs.increasingBSTStack(node1);
		BFS bfs=new BFS();
		List<Integer> result=bfs.bfs(node);
		System.out.println(result.toString());
		
		
//		IncreasingBSTRecursion ibr=new IncreasingBSTRecursion();
//		TreeNode nodeIBR=ibr.increasingBTSRecursion(node1);
//		BFS bfs1=new BFS();
//		List<Integer> result1=bfs1.bfs(nodeIBR);
//		System.out.println(result1.toString());
		
//		IncreasingBTSRecursionRUI ibrr=new IncreasingBTSRecursionRUI();
//		TreeNode nodeIBRR=ibrr.increasingBTSRecursion(node1);
//		BFS bfs2=new BFS();
//		List<Integer> result2=bfs2.bfs(nodeIBRR);
//		System.out.println(result2.toString());
	}
}
```
