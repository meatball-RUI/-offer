# 序列化和反序列化二叉树
## 思路
前序遍历二叉树
## 难点
只把节点的值序列化到字符串中是不够的。首先，要用一个分隔符（如逗号）把不同的节点分隔开。其次，还要考虑如何才能在反序列化的时候构建不同结构的二叉树。例如，图左和图右中的二叉树都有5个节点，并且每个节点的值都是6.如果只把节点的值序列化到字符串，那么序列化这两棵二叉树的结果是相同的，都是一串数字6.如果这样，反序列化的时候就不能构建不同结构的二叉树。  
<img src="https://github.com/meatball-RUI/DataStructure-and-Algorithm-Offer/blob/main/offer48-deserialize/Screenshot%202024-09-15%20at%2017.07.29.png" width=400px />

### 难点解决
说明:(a)序列化成字符串“6，6，6，#，#，6，#，#，6，#，#”的二叉树；（b）序列化成字符串“6，6，#，#，6，6，#，#，6，#，#”的二叉树  
用前序遍历解决序列化，返序列化也是前序递归

## Code
### TreeNode
```java
package offer48;

public class TreeNode {
	TreeNode left;
	TreeNode right;
	int val;
	public TreeNode(int val) {
		this.val=val;
	}
}
```
### Serialize
```java
package offer48;

public class Serialize {
	public String serialize(TreeNode root) {
		if(root==null) {
			return "#";
		}
		String leftStr=serialize(root.left);
		String rightStr=serialize(root.right);
		return String.valueOf(root.val)+","+leftStr+","+rightStr;
	}
}
```
### DeSerialize
```java
package offer48;

public class DeSerialize {
	public TreeNode deSerialize(String str) {
		String[] strs=str.split(",");
		int[] i= {0};
		return dfs(strs,i);
	}

	private TreeNode dfs(String[] strs, int[] i) {
		String str=strs[i[0]];
		i[0]++;
		if(str.equals("#")) {
			return null;
		}
		TreeNode node=new TreeNode(Integer.valueOf(str));
		node.left=dfs(strs,i);
		node.right=dfs(strs,i);
		return node;
	}
}
```
### main
序列化成字符串后，反序列化构建二叉树，然后再序列成字符串
```java
package offer48;

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
		Serialize s=new Serialize();
		String str=s.serialize(node1);
		System.out.print(str);
		System.out.println();
		DeSerialize des=new DeSerialize();
		TreeNode node=des.deSerialize(str);
		Serialize s1=new Serialize();
		String str1=s1.serialize(node);
		System.out.print(str1);
	}
}
```
### 执行结果
``` java
1,2,4,#,#,5,#,#,3,#,#
1,2,4,#,#,5,#,#,3,#,#
```
