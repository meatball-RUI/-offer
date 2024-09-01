# 小行星相撞
## 题目
输入一个表示小行星的数组，数组中每个数字的绝对值表示小行星的大小，数字的正负号表示小行星运动的方向，正号表示向右飞行，负号表示向左飞行。如果两颗小行星相撞，那么体积较小的小行星将会爆炸最终消失，体积较大的小行星不受影响。如果相撞的两颗小行星大小相同，那么它们都会爆炸消失。飞行方向相同的小行星永远不会相撞。求最终剩下的小行星。  
eg  
有6颗小行星【4，5，-6，4，8，-5】,它们相撞之后最终剩下3颗小行星【-6，4，8】  
## 小行星相撞过程
| 步骤 | 小行星| 操作 | 栈 | 注释 ｜
| :--- | :--- | :--- | :-- | :-- |
| 1  | "2" | 入栈 | [2] ||
| 2  | "1" | 入栈 | [2][1] | |
| 3  | "3" | 入栈 | [2][1][3] | |
| 4  | "*" | 乘法运算 | [2,3] | 3,1出栈，结果3入栈
| 5  | "+" | 加法运算 | [5] | 3,2出栈，结果5入栈