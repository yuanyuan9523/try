#Group Anagram

==Problem:== Given an array of strings, group anagrams together.

==Example:== 
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
- - -
#### Solution 1
For each string, compare whether the number of occurrences of each character is equal. If they are equal, put them in a list as a category.
```
public List<List<String>> groupAnagrams(String[] strs) {
    List<List<String>> ans = new ArrayList<>();
    boolean[] used = new boolean[strs.length];
    for (int i = 0; i < strs.length; i++) {
        List<String> temp = null;
        if (!used[i]) {
            temp = new ArrayList<String>();
            temp.add(strs[i]);
            //Compare each pair to see if the strings match
            for (int j = i + 1; j < strs.length; j++) {
                if (equals(strs[i], strs[j])) {
                    used[j] = true;
                    temp.add(strs[j]);
                }
            }
        }
        if (temp != null) {
            ans.add(temp);
        }
    }
    return ans;
}

private boolean equals(String string1, String string2) {
    Map<Character, Integer> hash = new HashMap<>();
    //Record the number of occurrences of each character in the first string, and accumulate
    for (int i = 0; i < string1.length(); i++) {
        if (hash.containsKey(string1.charAt(i))) {
            hash.put(string1.charAt(i), hash.get(string1.charAt(i)) + 1);
        } else {
            hash.put(string1.charAt(i), 1);
        }
    }
   //Record the number of times each character of the first string appears, and subtract 1 from each previous one
    for (int i = 0; i < string2.length(); i++) {
        if (hash.containsKey(string2.charAt(i))) {
            hash.put(string2.charAt(i), hash.get(string2.charAt(i)) - 1);
        } else {
            return false;
        }
    }
    //Determine whether the number of times for each character is 0 or not, otherwise it returns false directly
    Set<Character> set = hash.keySet();
    for (char c : set) {
        if (hash.get(c) != 0) {
            return false;
        }
    }
    return true;
}

```
#### Solution 2
Sort each string alphabetically, and uniquely map a class of strings to the same position.
```
public List<List<String>> groupAnagrams(String[] strs){
    List<List<String>> res =  new ArrayList<>();
    if (strs == null || strs.length == 0) return res;
    HashMap<String,Integer> map= new HashMap<>();
    for (String str : strs) {
    	char[] c = str.toCharArray();
    	Arrays.sort(c);
    	String s = new String(c);
    	if(map.containsKey(s)) {
    		List<String> list = res.get(map.get(s));
    		list.add(str);
    	} else {
    		List<String> list = new ArrayList<>();
    		list.add(str);
    		map.put(s, res.size());
    		res.add(list);
    	}
    }
    return res;
}
```
#### Solution 3
Record the number of occurrences of each character of the string to complete the mapping.
 ```
HashMap<String, List<String>> map = new HashMap<>();
    for(String str : strs) {
    	int[] count = new int[26];
    	for(Character c : str.toCharArray()) {
    		count[c-'a']++;
    	}
    	String s = "";
    	for (int i = 0; i < count.length; i++) {
			s += String.valueOf(count[i]) + String.valueOf((char) i + 'a');
		}
    	if (map.containsKey(s)) {
    		List<String> list = map.get(s);
    		list.add(str);
    	} else {
    		List<String> list = new ArrayList<>();
    		list.add(str);
    		map.put(s, list);
    	}
    }
    return new ArrayList<>(map.values());
}
```






