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
	 		//创建一个对应26个英文字母的质数数组
        		int[] hash= {2,3,5,7,11,13,17,19,23,29,31,37,41,43,47,53,59,61,67,71,73,79,83,89,97,101};
	  		//创建一个key为质数乘积，value为变位词组的list的HashMap
        		HashMap<Long,List<String>> groups=new HashMap<>();
	  		//遍历strs数组
        		for(String str:strs) {
        			long wordHash=1;
	   			//遍历每个单词求其质数乘积
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

	public class Main {
	
		public static void main(String[] args) {
			// TODO Auto-generated method stub
			GroupAnagrams ga=new GroupAnagrams();
			String[] input={"eat", "tea", "tan", "ate", "nat", "bat"};
			System.out.print(ga.groupAnagrams1(input));
			//为什么只能是ga.groupAnagrams1(input)调用;
			//不能是ga.groupAnagrams1({"eat", "tea", "tan", "ate", "nat", "bat"})调用
			//1.因为如果实参是基本类型（包括包装类型）或者String，则实参不会变（传的是值）
			//2.如果实参是对象集合或者数组，则实参会改变（传的是引用）	
		}
	
	}

        此时会报错：
        Exception in thread "main" java.lang.ClassCastException: class java.util.HashMap$Values cannot be cast to class                        java.util.List (java.util.HashMap$Values and java.util.List are in module java.base of loader 'bootstrap')
	      at offer33.GroupAnagrams.groupAnagrams1(GroupAnagrams.java:25)
	      at offer33.Main.main(Main.java:9)
       	来自stackOverflow上的解释：
	Explanation
 	Because HashMap#values() returns a java.util.Collection<V> and you can't cast a Collection into an ArrayList, thus you get 	ClassCastException.
  	因为HashMap的values方法返回的是一个Collection，没办法直接将一个Collection转成ArrayList，所以同理这边我们也没办法直接将一个Collection转成List

	I'd suggest using ArrayList(Collection<? extends V>) constructor. This constructor accepts an object which implements 		Collection<? extends V> as an argument. You won't get ClassCastException when you pass the result of HashMap.values() like 	this:
	List<V> al = new ArrayList<V>(hashMapVar.values());
 	使用可以接收Collection的ArrayList的构造函数可以解决这个问题，同理我们可以new一个List的子类来解决这个问题.
  	那么List的子类是用LinkedList还是ArrayList？
   		看一下LinkedList跟ArrayList的Constructor
     			LinkedList：
				Constructors Constructor and Description
				LinkedList()
				Constructs an empty list.
				LinkedList(Collection<? extends E> c)
				Constructs a list containing the elements of the specified collection, in the order they are 					returned by the collection's iterator.
                                构造一个包含指定集合的​​元素的列表，按照集合迭代器返回的顺序排列它们。
    			ArrayList：
       				Constructors Constructor and Description
				ArrayList()
				Constructs an empty list with an initial capacity of ten.
				ArrayList(Collection<? extends E> c)
				Constructs a list containing the elements of the specified collection, in the order they are 					returned by the collection's iterator.
				ArrayList(int initialCapacity)
				Constructs an empty list with the specified initial capacity.
    				构造一个包含指定集合的​​元素的列表，按照集合迭代器返回的顺序排列它们。
    				会发现这两个构造函数LinkedList(Collection<? extends E> c) ArrayList(Collection<? extends E> c)的作用
	                        是一样的。
       				return  new ArrayList<>(groups.values());的执行
					import java.util.ArrayList;
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
							return  new ArrayList<>(groups.values());
						}
					}
     				结果：[[eat, tea, ate], [tan, nat], [bat]]

	  			return new LinkedList<>(groups.values());的执行
				import java.util.ArrayList;
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
						return new LinkedList<>(groups.values());
				//		return  new ArrayList<>(groups.values());
					}
				}
    				结果：[[eat, tea, ate], [tan, nat], [bat]]
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

 	解法二：把一组变位词映射到同一个单词
  	eg.把“eat”、“tea”、“ate”的字母执照字母表排序都得到字符串“aet”
   	哈希表的键是把单词字母排序得到的字符串，而值为一组变位词
    	public List<List<String>> groupAnagrams(String[] strs){
     		Map<String,List<String>> groups=new HashMap<>();
       		
	}

https://blog.csdn.net/weixin_43244698/article/details/106675167


