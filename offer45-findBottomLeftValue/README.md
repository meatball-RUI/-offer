# 二叉树最低层最左边的值
## 错题集
```java
package offer45;

import java.util.LinkedList;
import java.util.Queue;

public class FindBottomLeftValue {
	public int findBottomLeftValue(TreeNode root) {
		Queue<TreeNode> queue1=new LinkedList<TreeNode>();
		Queue<TreeNode> queue2=new LinkedList<TreeNode>();
		int bottomLeft;
		if(root!=null) {
			queue1.offer(root);
			bottomLeft=root.val;
		}
		while(!queue1.isEmpty()) {
			TreeNode node=queue1.poll();
			if(node.left!=null) {
				queue2.offer(node.left);
			}
			if(node.right!=null) {
				queue2.offer(node.right);
			}
			if(queue1.isEmpty()) {
				queue1=queue2;
				queue2=new LinkedList<>();
				bottomLeft=queue1.peek().val;
			}
		}
		return bottomLeft;
	}
}
```
```java
这个题目里面已经假设至少会存在一个节点了，所以不用再单独考虑root！=null的情况
```

## 又错了，出现了空指针异常
```
Exception in thread "main" java.lang.NullPointerException: Cannot read field "val" because the return value of "java.util.Queue.peek()" is null
	at offer45.FindBottomLeftValue.findBottomLeftValue(FindBottomLeftValue.java:21)
	at offer45.Main.main(Main.java:19)
```
``` java
package offer45;

import java.util.LinkedList;
import java.util.Queue;

public class FindBottomLeftValue {
	public int findBottomLeftValue(TreeNode root) {
		Queue<TreeNode> queue1=new LinkedList<TreeNode>();
		Queue<TreeNode> queue2=new LinkedList<TreeNode>();
		queue1.offer(root);
		int bottomLeft=root.val;
		while(!queue1.isEmpty()) {
			TreeNode node=queue1.poll();
			if(node.left!=null) {
				queue2.offer(node.left);
			}
			if(node.right!=null) {
				queue2.offer(node.right);
			}
			if(queue1.isEmpty()) {
				queue1=queue2;
				queue2=new LinkedList<>();
				bottomLeft=queue1.peek().val;
			}
		}
		return bottomLeft;
	}
}
```
## chatgpt的优化
你代码的逻辑整体是正确的，但有以下几点需要注意或改进：  
1.空队列情况下调用 queue1.peek()：  
在你的代码中，queue1.peek().val 在 queue1 可能为空的情况下调用。在某一层处理完之后，queue1 赋值为 queue2。但是如果 queue2 为空（即已经没有下一层的节点），此时调用 queue1.peek() 会导致空指针异常（NullPointerException）。  
2.不需要两个队列：  
你可以简化你的实现，不需要用两个队列。广度优先搜索可以通过遍历当前层的所有节点，并直接将下一层的节点加入同一个队列中。  
代码修正：  
你可以使用一个队列，并且每次只更新 bottomLeft 为每一层的第一个节点的值。  
``` java
package offer45;

import java.util.LinkedList;
import java.util.Queue;

public class FindBottomLeftValue {
    public int findBottomLeftValue(TreeNode root) {
        // 使用一个队列来进行层序遍历
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        // 用于存储最左下角的值
        int bottomLeft = root.val;

        // 开始层序遍历
        while (!queue.isEmpty()) {
            // 在每层开始时，更新 bottomLeft 为该层的第一个节点的值
            bottomLeft = queue.peek().val;

            // 遍历当前层的所有节点
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                
                // 左子节点优先入队
                if (node.left != null) {
                    queue.offer(node.left);
                }
                // 然后右子节点入队
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
        }
        
        return bottomLeft; // 返回最左下角的值
    }
}
```
改进的要点：  
1.一个队列简化代码：使用一个队列即可进行层序遍历，按照从左到右的顺序依次遍历每层节点的左右子节点。  
2.每层第一个节点的值：在每层开始时，将 queue.peek().val 赋值给 bottomLeft，这样可以确保每次都更新当前层的最左节点。  
3.空指针检查避免：通过在每一层的遍历开始时检查 queue.peek() 来避免空队列调用，确保逻辑安全。  
总结：  
使用一个队列可以让代码更加简洁。  
修正了潜在的空队列调用 peek() 引发的 NullPointerException 问题。  

