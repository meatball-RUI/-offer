# 从根节点到叶节点的的路径数字之和
## 解题小经验
因为路径通常是顺着指向子节点的指针的方向，也就是纵向方向，这更加符合深度优先搜索的特点。
## code
```java
package offer49;

public class TreeNode {
	TreeNode left;
	TreeNode right;
	int val;
	public TreeNode(int val) {
		this.val=val;
	}
}
```
## code
```java
package offer49;

public class SumNumbersRUI{
	public int sumNumbersRUI(TreeNode root) {
		return dfs(root,0);
	}

	private int dfs(TreeNode root, int path) {
		if(root==null) {
			return 0;
		}
		path=path*10+root.val;
		//如果当前节点是叶子节点，返回当前path的值
		if(root.left==null&&root.right==null) {
			return path;
		}
		return dfs(root.left,path)+dfs(root.right,path);
	}
}
```
```java
package offer49;


public class Main {
	public static void main(String[] args) {
		TreeNode node1=new TreeNode(1);
		TreeNode node2=new TreeNode(2);
		TreeNode node3=new TreeNode(3);
		TreeNode node4=new TreeNode(4);
		TreeNode node5=new TreeNode(5);
		node1.left=node2;
		node1.right=node3;
		node2.left=node4;
		node2.right=node5;
		SumNumbersRUI snr=new SumNumbersRUI();
		System.out.print(snr.sumNumbersRUI(node1));
	}
}
```
