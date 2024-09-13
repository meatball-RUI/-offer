# 二叉树剪枝 
## 错题集
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

