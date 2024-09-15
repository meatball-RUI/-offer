# 二叉树剪枝
``` java
package offer47;

public class PruneTree {
	//定义一个方法pruneTree来修剪树，参数为树的根节点root
	public TreeNode pruneTree(TreeNode root) {
		//如果当前节点为null，则返回null
		if(root==null) {
			return null;
		}
		//递归修剪左子树，将修剪后的结果赋值给当前节点的左子树
		root.left=pruneTree(root.left);
		//递归修剪右子树，将修剪后的结果赋值给当前节点的右子树
	    root.right=pruneTree(root.right);
	    //如果当前节点的左子树和右子树都为空，且当前节点的值为0，则删除该节点（返回null）
		if(root.left==null&&root.right==null&&root.val==0) {
			return null;
		}
		//否则返回当前节点（即不删除该节点）
		return root;
	}
}
```
