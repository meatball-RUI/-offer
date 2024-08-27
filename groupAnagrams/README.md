变位词组

    题目：给定一组单词（means 一组字符串数组），请将它们按照变位词分组。
    例如，输入一组单词【"eat","tea","tan","ate","nat",""bat】,可分成三组
        ["eat","tea","ate"]
        ["tan","nat"]
        ["bat"]
    假设英文单词中只包含英文小写字母。
    分析：找到一组变位词共同的特性
    

    解法一：将每个英文小写字母映射到一个质数，比较质数乘积
    
        import java.util.HashMap;
        import java.util.LinkedList;
        import java.util.List;
        
        public class GroupAnagrams {
        	//如果输入n个单词，平均每个单词由m个字母，那么第一种思路的时间复杂度是O(mn)
        	public  List<List<String>> groupAnagrams1(String[] strs){
        		int[] hash= {2,3,5,7,11,13,17,19,23,29,31,37,41,43,47,53,59,61,67,71,73,79,83,89,97,101};
        		HashMap<Long,List<String>> groups=new HashMap<>();
        		for(String str:strs) {
        			long wordHash=1;
        			for(int i=0;i<str.length();i++) {
        				wordHash *=hash[str.charAt(i)-'a'];
        			}
        			//put与putIfAbsent区别：
        			//put在放入数据时，如果放入数据的key已经存在于Map中，最后放入的数据会覆盖之前存在的数据，
        			//而putIfAbsent在放入数据时，如果存在重复的key,那么putIfAbsent不会放入值
        			groups.putIfAbsent(wordHash, new LinkedList<String>());
        			groups.get(wordHash).add(str);
        		}
        //		return new LinkedList<>(groups.values());
        		return  (List<List<String>>) groups.values();
        	}
        }
        此时会报错：
        Exception in thread "main" java.lang.ClassCastException: class java.util.HashMap$Values cannot be cast to class                        java.util.List (java.util.HashMap$Values and java.util.List are in module java.base of loader 'bootstrap')
	      at offer33.GroupAnagrams.groupAnagrams1(GroupAnagrams.java:25)
	      at offer33.Main.main(Main.java:9)
       这边对HashMap.values不是很了解
       Constructor in HashMap
       Constructor and Description
        HashMap()
        Constructs an empty HashMap with the default initial capacity (16) and the default load factor (0.75).
        HashMap(int initialCapacity)
        Constructs an empty HashMap with the specified initial capacity and the default load factor (0.75).
        HashMap(int initialCapacity, float loadFactor)
        Constructs an empty HashMap with the specified initial capacity and load factor.
        HashMap(Map<? extends K,? extends V> m)
        Constructs a new HashMap with the same mappings as the specified Map.

       Collection<V> 	values()
      Returns a Collection view of the values contained in this map.

https://blog.csdn.net/weixin_43244698/article/details/106675167


