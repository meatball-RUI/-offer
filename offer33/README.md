变位词组

    题目：给定一组单词（means 一组字符串数组），请将它们按照变位词分组。
    例如，输入一组单词【"eat","tea","tan","ate","nat",""bat】,可分成三组
        ["eat","tea","ate"]
        ["tan","nat"]
        ["bat"]
    假设英文单词中只包含英文小写字母。
    分析：找到一组变位词共同的特性
    

    解法一：将每个英文小写字母映射到一个质数，比较质数乘积
    	```java
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
 	```

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
     		//创建一个HashMap，key为排序好的字符串，value为变位词组
     		Map<String,List<String>> groups=new HashMap<>();
       		//遍历字符串数组中的字符串
       		for(String str:strs){
	 		//将字符转换成字符数组为了后面给字符串中的字母排序用
	 		char[] charArray=str.toCharArray();
    			//对字符串中的字母进行排序
    			Arrays.sort(charArray);
       			//利用String的构造函数将，字符数组转换成字符串
       			String sorted=new String(charArray);
	  		如果sorted不存在的话，创建一个以sorted为key的键值对
	  		groups.putIfAbsent(sorted,new LinkedList<String>)();
     			将符合条件的单词跟sorted映射
     			groups.get(sorted).add(str);
	}
 	//利用LinkedList构造函数构造一个包含指定集合的​​元素的列表，按照集合迭代器返回的顺序排列它们。
 	return new LinkedList<>(groups.values());
}
	
 	不太懂的：
        Arrays.sort(charArray);
	通过Arrays.sort可以将字符串数组升序排序
 	Arrays.sort():
  	static void        sort(char[] a)
   			   Sorts the specified range of the array into ascending order 
	 
	 String sorted=new String(charArray):
  	 用这个构造函数可以把这个字符串数组变成字符串
         String(char[] value)
         Allocates a new String so that it represents the sequence of characters currently contained in the character array argument.

错题集：
package offer33;

import java.util.Arrays;
import java.util.HashMap;
import java.util.LinkedList;
import java.util.List;
import java.util.Map;

public class GroupAnagrams1 {
	public List<List<String>> groupAnagrams1(String[] strs){
		Map<String,List<String>> map=new HashMap<String,List<String>>();
		for(String str:strs) {
			char[] hashword=str.toCharArray();
			Arrays.sort(hashword);
			String word=hashword.toString();
			//System.out.println("the word is:"+word);
			map.put(word, new LinkedList<String>());
			map.get(word).add(str);
		}
		return new LinkedList<>(map.values());
	}
}
结果：
the word is:[C@1c655221
the word is:[C@6996db8
the word is:[C@1963006a
the word is:[C@7fbe847c
the word is:[C@41975e01
the word is:[C@c2e1f26
[[cab], [caa], [a], [aac], [abc], [cba]]
the word is:[C@7dc5e7b4
[[]]
the word is:[C@1ee0005
[[aa]]
错的点：将字符数组转成String不是用的toString的方法，是用的String的构造函数
String word=hashword.toString();
System.out.println("the word is:"+word);

String的构造函数
String(char[] value)
Allocates a new String so that it represents the sequence of characters currently contained in the character array argument.

toString方法
Java toString() Method
If you want to represent any object as a string, to String() method comes into existence.
The toString method returns the String representation of the object.
If you print any object, Java compiler internally invokes the toString() method on the object. So overriding the toString() method, returns the desired output, it can be the state of an object etc. depending on your implementation.
Advantage of Java toString() method
By overriding the toString() method of the Object class, we can return values of the object, so we don't need to write much code.
Understanding problem without toString() method
Let's see the simple code that prints reference.
Student.java
class Student{
	int rollno;
 	String name;
  	String city;
   	Student(int rollno, String name, String city){
    		this.rollno=rollno;
      		this.name=name;
		this.city=city;
	}
 	public static void main(String args[]){
  		Student s1=new Student(101,"Raj","lucknow");
    		Student s2=new Student(102,"Vijay","ghaziabad");
      		System.out.println(s1);//compiler writes here s1.toString()
		System.out.println(s2);//compiler writes here s2.toString()
	}
}
Output:

Student@1fee6fc
Student@1eed786

As you can see in the above example, printing s1 and s2 prints the hashcode values of the objects but I want to print the values of these objects.Since Java compiler internally calls toString()

Example of Java toString() method
Let's see an example of toString() method
Student.java
class Student{
int rollno;
String name;
String city;
Student(int rollno,String name,String city){
	this.rollno=rollno;
 	this.name=name;
  	this.city=city;
}
public String toString(){
	//overriding the toString() method
 	return rollno+" "+name+" "+city;
}
public static void main(String args[]){
	Student s1=new Student(101,"Raj","lucknow");
 	Student s2=new Student(102,"Vijay","ghaziabad");
  	System.out.println(s1); //compiler writes here s1.toString();
   	System.out.println(s2);//compiler writes here s2.toString();
}
}
Output:

101 Raj lucknow
102 Vijay ghaziabad

In the above program, Java compiler internally calls toString() method, overriding this method will return the specified values of s1 and s2 objects of Student class.
   
https://blog.csdn.net/weixin_43244698/article/details/106675167


