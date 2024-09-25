# 节点值之和的最大的路径
## TreeNode 
``` java
package offer51;

public class TreeNode {
	TreeNode left;
	TreeNode right;
	int val;
	public TreeNode(int val) {
		this.val=val;
	}
}
```
## MaxPathSum
```java
package offer51;

public class MaxPathSum {
	public int maxPathSum(TreeNode root) {
		int[] maxSum= {Integer.MIN_VALUE};
		dfs(root,maxSum);
		return maxSum[0];
	}

	private int dfs(TreeNode root, int[] maxSum) {
		if(root==null) {
			return 0;
		}
		int leftMax=Math.max(0,dfs(root.left,maxSum));
		int rightMax=Math.max(0,dfs(root.right,maxSum));
		maxSum[0]=Math.max(maxSum[0],root.val+leftMax+rightMax);
		return root.val+Math.max(leftMax, rightMax);
	}
}
```
## Main
```java
package offer51;


public class Main {

	public static void main(String[] args) {
		TreeNode node1=new TreeNode(-5);
		TreeNode node2=new TreeNode(2);
		TreeNode node3=new TreeNode(-4);
		TreeNode node4=new TreeNode(-1);
		node1.left=node2;
		node1.right=node3;
		node2.left=node4;
		System.out.print(new MaxPathSum().maxPathSum(node1));
	}

}
```
