# 二叉树剪枝 
## 错题集1,中序递归遍历二叉树
``` java
package offer47;

import java.util.ArrayList;
import java.util.List;

public class InOrderTraversal {
	public List<Integer> inOrderTraversal(TreeNode root){
		List<Integer> result=new ArrayList<>();
		if(root!=null) {
			inOrderTraversal(root.left);
			result.add(root.val);
			inOrderTraversal(root.right);
		}
		return result;
	}
}
```
### 错误点
在递归调用时没有将结果正确地传递和汇总到返回的列表中。在递归调用时忽略了将递归调用返回的结果与当前的result列表合并
### 问题分析
进行inOrderTraversal(root.left)还有inOrderTraversal(root.right);递归调用时，虽然遍历了子树，但没有保存这些递归调用的结果，导致最终的结果列表只包含当前节点的值（即result.add(root.val)）,不会包含左右子树的结果。
#### 解决1:用addAll（）方法
```java
package offer47;

import java.util.ArrayList;
import java.util.List;

public class InOrderChatGPT {
	public List<Integer> inOrderTraversal(TreeNode root){
		List<Integer> result=new ArrayList<>();
		if(root!=null) {
			result.addAll(inOrderTraversal(root.left));
			result.add(root.val);
			result.addAll(inOrderTraversal(root.right));
		}
		return result;
	}
}
```
#### 解决2:给inOrderTraversal方法加上result的参数进行传递
```java
package offer47;

import java.util.LinkedList;
import java.util.List;

public class InOrderTraversalRUI {
	public List<Integer> inOrderTraversal(TreeNode root){
		List<Integer> nodes=new LinkedList<>();
		dfs(root,nodes);
		return nodes;
	}

	private void dfs(TreeNode root, List<Integer> nodes) {
		if(root!=null) {
			dfs(root.left,nodes);
			nodes.add(root.val);
			dfs(root.right,nodes);
		}
		
	}
	
}
```
#### 解决3:将List<Integer> result=new ArrayList<>(); 声明成类成员变量
``` java
import java.util.LinkedList;
import java.util.List;

public class InOrderTraversalRUI {
    private List<Integer> nodes;  // 将 nodes 声明为类成员变量

    public List<Integer> inOrderTraversal(TreeNode root) {
        nodes = new LinkedList<>();
        dfs(root);
        return nodes;
    }

    private void dfs(TreeNode root) {
        if (root != null) {
            dfs(root.left);
            nodes.add(root.val);
            dfs(root.right);
        }
    }
}
```
## 错题集2 java.lang.NullPointerException
报的错
``` java
Exception in thread "main" java.lang.NullPointerException: Cannot read field "val" because "root" is null
	at offer47.PreOrderTraversal.bfs(PreOrderTraversal.java:15)
	at offer47.PreOrderTraversal.bfs(PreOrderTraversal.java:16)
	at offer47.PreOrderTraversal.bfs(PreOrderTraversal.java:16)
	at offer47.PreOrderTraversal.bfs(PreOrderTraversal.java:16)
	at offer47.PreOrderTraversal.preOrderTraversal(PreOrderTraversal.java:10)
	at offer47.Main.main(Main.java:25)
```
code
``` java
package offer47;

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class PreOrderTraversal {
	public List<Integer> preOrderTraversal(TreeNode root){
		List<Integer> result=new LinkedList<>();
		bfs(root,result);
		return result;
	}

	private void bfs(TreeNode root, List<Integer> result) {
		result.add(root.val);
		bfs(root.left,result);
		bfs(root.right,result);
	}
}
```
啊，吃屎了，基条件都没加
### 解决：加上基条件if(root!=null)
``` java
package offer47;

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class PreOrderTraversal {
	public List<Integer> preOrderTraversal(TreeNode root){
		List<Integer> result=new LinkedList<>();
		bfs(root,result);
		return result;
	}

	private void bfs(TreeNode root, List<Integer> result) {
		if(root!=null) {
			result.add(root.val);
			bfs(root.left,result);
			bfs(root.right,result);
		}
	}
}
```
## 中序遍历递归+栈实现
### 中序遍历递归1
```java
package offer47;

import java.util.ArrayList;
import java.util.List;

public class InOrderTraversal {
	List<Integer> result=new ArrayList<>();
	public List<Integer> inOrderTraversal(TreeNode root){
		if(root!=null) {
			inOrderTraversal(root.left);
			result.add(root.val);
			inOrderTraversal(root.right);
		}
		return result;
	}
}
```
### 中序遍历递归2
```java
package offer47;

import java.util.LinkedList;
import java.util.List;

public class InOrderTraversalRUI {
	public List<Integer> inOrderTraversal(TreeNode root){
		List<Integer> nodes=new LinkedList<>();
		dfs(root,nodes);
		return nodes;
	}

	private void dfs(TreeNode root, List<Integer> nodes) {
		if(root!=null) {
			dfs(root.left,nodes);
			nodes.add(root.val);
			dfs(root.right,nodes);
		}
		
	}
	
}
```
### 中序遍历递归3
```java
package offer47;

import java.util.ArrayList;
import java.util.List;

public class InOrderChatGPT {
	public List<Integer> inOrderTraversal(TreeNode root){
		List<Integer> result=new ArrayList<>();
		if(root!=null) {
			result.addAll(inOrderTraversal(root.left));
			result.add(root.val);
			result.addAll(inOrderTraversal(root.right));
		}
		return result;
	}
}
```
### 中序遍历Stack实现
package offer47;

import java.util.LinkedList;
import java.util.List;
import java.util.Stack;

public class InOrderStack {
	public List<Integer> inorderTraversal(TreeNode root) {
		List<Integer> result=new LinkedList<>();
		Stack<TreeNode> stack=new Stack<>();
		TreeNode cur=root;
		while(cur!=null||!stack.isEmpty()) {
			//先沿着左子树一直迭代一直到叶子节点
			while(cur!=null) {
				stack.push(cur);
				cur=cur.left;
			}
//			将叶子节点弹出
			TreeNode node=stack.pop();
//          将当前根节点加入到list中
			result.add(node.val);
//			访问当前根节点的右子节点
			cur=node.right;
		}
		return result;
	}
}
## 后序遍历3种递归实现及stack
### 后序遍历递归1
```java
package offer47;

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class PostOrderTraversal {
	public List<Integer> postOrderTraversal(TreeNode root){
		List<Integer> result=new LinkedList<>();
		bfs(root,result);
		return result;
	}

	private void bfs(TreeNode root,List<Integer> result) {
		if(root!=null) {
			bfs(root.left,result);
			bfs(root.right,result);
			result.add(root.val);
		}
	}
}
```
### 后序遍历递归2
```java
package offer47;

import java.util.LinkedList;
import java.util.List;

public class PostOrderChatGPT {
	List<Integer> postOrderChatGPT(TreeNode root){
		List<Integer> result=new LinkedList<>();
		if(root!=null) {
			result.addAll(postOrderChatGPT(root.left));
			result.addAll(postOrderChatGPT(root.right));
			result.add(root.val);
		}
		return result;
	}
}
```
### 后序遍历递归3
```java
package offer47;

import java.util.LinkedList;
import java.util.List;

public class PostOrderRUI {
	List<Integer> result=new LinkedList<>();
	public List<Integer> postOrderRUI(TreeNode root){
		if(root!=null) {
			postOrderRUI(root.left);
			postOrderRUI(root.right);
			result.add(root.val);
		}
		return result;
	}
}
```
### 后序遍历Stack
```java
package offer47;

import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class PostOrderStack {
	public List<Integer> postOrderStack(TreeNode root){
		List<Integer> result=new ArrayList<>();
		Stack<TreeNode> stack1=new Stack<>();
		Stack<TreeNode> stack2=new Stack<>();
		if(root==null) {
			return result;
		}
		stack1.push(root);
		while(!stack1.isEmpty()) {
			TreeNode node=stack1.pop();
			stack2.push(node);
			if(node.left!=null) {
				stack1.push(node.left);
			}
			if(node.right!=null) {
				stack1.push(node.right);
			}
		}
		while(!stack2.isEmpty()) {
			result.add(stack2.pop().val);
		}
		return result;
	}
}
```

