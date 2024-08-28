外星语言是否排序

题目：有一门外星语言，它的字母表刚好包含所有的英文小写字母，只是字母表的顺序不同。给定一组单词和字母表顺序，请判断这些单词是否按照字母表的顺序排序。
eg.一组单词["offer","is","coming"]，字母表顺序"zyxwvutsrqponmlkjihgfedcba"
由于字母‘o’在字母表中位于‘i’的前面，因此单词"offer"排在“is”前面；同样，由于字母“i”在字母表中在c前面，因此单词is排在coming的前面。
因此输出为True

思路：
  创建一个哈希表，哈希表的键为字母表的每个字母，而值为字母在字母表中的顺序。这个可以方便查找每个字母在字母表中的顺序。
  用一个长度为26的数组来模拟哈希表，可以精简代码
<div align="left">

| key  | value|
| ---------- | -----------|
| z   | 0   |
| y   | 1   |
| ...   | ...  |
</div>

```java
public boolean is AlienSorted(String[] words,String order){
}
```
