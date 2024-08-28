# 外星语言是否排序

## 题目
有一门外星语言，它的字母表刚好包含所有的英文小写字母，只是字母表的顺序不同。给定一组单词和字母表顺序，请判断这些单词是否按照字母表的顺序排序。
eg.一组单词["offer","is","coming"]，字母表顺序"zyxwvutsrqponmlkjihgfedcba"
由于字母‘o’在字母表中位于‘i’的前面，因此单词"offer"排在“is”前面；同样，由于字母“i”在字母表中在c前面，因此单词is排在coming的前面。
因此输出为True

## 思路
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
//创建一个方法用来判断一个单词是否按外星人字母表排序
public boolean isAlienSorted(String[] words,String order){
  //创建一个长度为26的数组用来模拟HashMap，key为外星人字母对应的数字表示，value为在外星人字母表中所处的位置
  int[] orderArray=new int[order.length()];
  //用一个循环依次将哈希表中的字母跟对应所处位置完成一一映射
  for(int i=0;i<order.length();++i){
      orderArray[order.charAt(i)-'a']=i;
  }
  //用一个循环遍历字符串数组直到到处第二个元素，两两判断是否符合外星语言顺序
  for(int i=0;i<words.length-1;++1){
    if(!isSorted(words[i],words[i+1],orderArray)){
        return false;
    }
  }
  return true;
}
//创建一个方法用来判断两个单词是否符合外星人字母表排序
private boolean isSorted(String word1,String word2,int[] order){
  //?现在不清楚为什么i=0不在for循环里面--后续，现在知道为什么i=0在for循环里了，因为这个i的变量后面还是要被用到
  int i=0;
  //遍历两个单词中的比较短的单词的长度
  for(;i<word1.length()&&i<word2.length();++i){
    //获取第一个单词中的第i个字符
    char ch1=word1.charAt(i);
    //获取第二个单词中的第i个字符
    char ch2=word2.charAt(i);
    //在没有比完的情况下，word1中只要出现了比word2小的就是符合条件的
    if(order[ch1-'a']<order[ch2-'a']){
      return true;
    }
    //在没有比完的情况下，word1中只要出现了比word2小的就不符合条件
    if(order[ch1-'a']>order[ch2-'a']){
      return false;
    }
  }
    //如果前面比较的所有的单词一样，那么word1比较短的话就会符合条件
    return i==word1.length();
}
```
##时间空间复杂度
时间复杂度：O（n），n为字符串数组的个数
空间复杂度：O(n),n为外星文字母表长度
