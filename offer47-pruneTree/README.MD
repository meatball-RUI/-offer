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
