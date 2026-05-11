1.1 Is Unique: Implement an algorithm to determine if a string has all unique characters. What if you
cannot use additional data structures?

```
    public boolean isUnique(String str) {
        // If the string length exceeds the number of unique chars in the charset
        if (str.length() > 128) return false;

        boolean[] char_set = new boolean[128];
        for (int i = 0; i < str.length(); i++) {
            int val = str.charAt(i);
            if (char_set[val]) { // Already found this char in string
                return false;
            }
            char_set[val] = true;
        }
        return true;
    }
```

```
     public static Boolean validparen(String s)
  {
    //String has all unique characters. 
      HashSet<Character> set = new HashSet<>();
      for(int i=0;i<s.length();i++)
      {
          if(!set.add(s.charAt(i)))
              return false;
      }
      return true;
  }
```
Time Complexity: $O(n)$  , Space Complexity: $O(k)$

1.2 Check Permutation: Given two strings, write a method to decide if one is a permutation of the
other.
```
    public class StringPermutation {

    /**
     * Checks if string2 is a permutation of string1.
     * Time Complexity: O(N) where N is the length of the strings.
     * Space Complexity: O(1) as the character set size is fixed (128 for ASCII).
     */
    public static boolean isPermutation(String s, String t) {
        // 1. Permutations must have the same length
        if (s.length() != t.length()) {
            return false;
        }

        // 2. Assuming ASCII character set (128 chars)
        int[] charCounts = new int[128]; 

        // 3. Count characters in the first string
        for (int i = 0; i < s.length(); i++) {
            charCounts[s.charAt(i)]++;
        }

        // 4. Compare with the second string
        for (int i = 0; i < t.length(); i++) {
            charCounts[t.charAt(i)]--;
            
            // If any count goes below zero, they aren't permutations
            if (charCounts[t.charAt(i)] < 0) {
                return false;
            }
        }

        return true;
    }

    public static void main(String[] args) {
        String str1 = "testing";
        String str2 = "ginetts";
        
        System.out.println("Are they permutations? " + isPermutation(str1, str2));
    }
}
```

1.3 _pg 193
URLify: Write a method to replace all spaces in a string with '%20'. You may assume that the string
has sufficient space at the end to hold the additional characters, and that you are given the "true"
length of the string. (Note: If implementing in Java, please use a character array so that you can
perform this operation in place.)
EXAMPLE
Input: "Mr John Smith "
, 13
Output: "Mr%20John%20Smith"
```
    public class URLify {

    /**
     * Replaces spaces in a character array with '%20' in-place.
     * @param str The character array with extra buffer space at the end.
     * @param trueLength The actual length of the string characters.
     */
    public static void replaceSpaces(char[] str, int trueLength) {
        int spaceCount = 0, index, i;

        // 1. Count the number of spaces in the true length
        for (i = 0; i < trueLength; i++) {
            if (str[i] == ' ') {
                spaceCount++;
            }
        }

        // 2. Calculate the new end index
        // Each space adds 2 extra characters (%20 replaces 1 space)
        index = trueLength + spaceCount * 2;

        // 3. Walk backwards and move characters or insert %20
        for (i = trueLength - 1; i >= 0; i--) {
            if (str[i] == ' ') {
                str[index - 1] = '0';
                str[index - 2] = '2';
                str[index - 3] = '%';
                index = index - 3;
            } else {
                str[index - 1] = str[i];
                index--;
            }
        }
    }

    public static void main(String[] args) {
        // Example: "Mr John Smith    " (True length 13, enough space for %20s)
        String input = "Mr John Smith    ";
        char[] charArray = input.toCharArray();
        int trueLength = 13;

        replaceSpaces(charArray, trueLength);

        System.out.println("URLified string: [" + new String(charArray).trim() + "]");
    }
}
```
Palindrome Permutation: Given a string, write a function to check if it is a permutation of a palindrome.
A palindrome is a word or phrase that is the same forwards and backwards. A permutation
is a rearrangement of letters. The palindrome does not need to be limited to just dictionary words.
EXAMPLE
Input: Tact Coa
Output: True (permutations: "taco cat", "atco eta", etc.)

```
    import java.util.HashSet;

public static boolean isPermutationOfPalindrome(String phrase) {
    HashSet<Character> set = new HashSet<>();
    
    for (char c : phrase.toLowerCase().toCharArray()) {
        if (Character.isLetter(c)) { // Ignore spaces/punctuation
            if (set.contains(c)) {
                set.remove(c); // Found a pair, even count
            } else {
                set.add(c); // Odd count for now
            }
        }
    }
    
    return set.size() <= 1;
}
```

String Compression: Implement a method to perform basic string compression using the counts
of repeated characters. For example, the string aabcccccaaa would become a2blc5a3. If the
"compressed" string would not become smaller than the original string, your method should return
the original string. You can assume the string has only uppercase and lowercase letters (a - z).

```
    public class StringCompressor {

    public static String compress(String str) {
        // 1. Initial check: If string is null or empty
        if (str == null || str.isEmpty()) return str;

        StringBuilder compressed = new StringBuilder();
        int countConsecutive = 0;

        for (int i = 0; i < str.length(); i++) {
            countConsecutive++;

            // 2. If next character is different or we are at the end
            if (i + 1 >= str.length() || str.charAt(i) != str.charAt(i + 1)) {
                compressed.append(str.charAt(i));
                compressed.append(countConsecutive);
                
                // Reset counter for the next character
                countConsecutive = 0;
            }
            
            // 3. Performance Optimization: If compressed is already longer than original
            if (compressed.length() >= str.length()) {
                return str;
            }
        }

        return compressed.toString();
    }

    public static void main(String[] args) {
        String input = "aabcccccaaa";
        System.out.println("Original: " + input);
        System.out.println("Compressed: " + compress(input)); // a2b1c5a3
        
        String input2 = "abc";
        System.out.println("Result for 'abc': " + compress(input2)); // abc (since a1b1c1 is longer)
    }
}
```
Rotate Matrix: Given an image represented by an NxN matrix, where each pixel in the image is 4
bytes, write a method to rotate the image by 90 degrees. Can you do this in place?
```
    class Solution {
        public void rotate(int[][] matrix) {
            //transpose
            //reflect
            transpose(matrix);
            reflect(matrix);
        }
        public void transpose(int[][] matrix)
        {
            for(int i=0;i<matrix[0].length;i++)
            {
                for(int j=i;j<matrix.length;j++)
                {
                    int tmp = matrix[j][i];
                    matrix[j][i] = matrix[i][j];
                    matrix[i][j] = tmp;
                }
            }
        }
        public void reflect(int[][] matrix)
        {
            int n= matrix.length;
            for(int i = 0;i<n;i++)
            {
                for(int j= 0;j<n/2;j++)
                {
                    int tmp = matrix[i][j];
                    matrix[i][j]= matrix[i][n-j-1];
                    matrix[i][n-j-1] = tmp;
                }
            }
        }
    }
```

Zero Matrix: Write an algorithm such that if an element in an MxN matrix is 0, its entire row and
column are set to 0.
```
    class Solution {
    public void setZeroes(int[][] matrix) {
        int R = matrix.length;
        int C = matrix[0].length;
        Set<Integer> rows = new HashSet<Integer>();
        Set<Integer> cols = new HashSet<Integer>();

        // Essentially, we mark the rows and columns that are to be made zero
        for (int i = 0; i < R; i++) {
            for (int j = 0; j < C; j++) {
                if (matrix[i][j] == 0) {
                    rows.add(i);
                    cols.add(j);
                }
            }
        }

        // Iterate over the array once again and using the rows and cols sets, update the elements.
        for (int i = 0; i < R; i++) {
            for (int j = 0; j < C; j++) {
                if (rows.contains(i) || cols.contains(j)) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
}
```

```
    class Solution {
    public void setZeroes(int[][] matrix) {
        
        boolean firstCol = false;
        boolean firstRow = false;
        
        for(int i=0; i<matrix.length; i++){
            if(matrix[i][0] == 0){
                firstCol = true;
                break;
            }
        }
        
        for(int i=0; i<matrix[0].length; i++){
            if(matrix[0][i] == 0){
                firstRow = true;
                break;
            }
        }
        
        for(int i=1; i<matrix.length; i++){
            for(int j=1; j<matrix[i].length; j++){
                if(matrix[i][j] == 0){
                    matrix[0][j] = 0;
                    matrix[i][0] = 0;
                }
            }
        }
        
        for(int i=1; i<matrix.length; i++){
            for(int j=1; j<matrix[i].length; j++){
                if(matrix[i][0] == 0 || matrix[0][j] == 0){
                    matrix[i][j] = 0;
                }
            }
        }
        
        if(firstCol){
            for(int i=0; i<matrix.length; i++){
                matrix[i][0] = 0;
            }
        }
        
        if(firstRow){
            for(int j=0; j<matrix[0].length; j++){
                matrix[0][j] = 0;
            }
        }
        
    }
}
```

String Rotation:Assumeyou have a method isSubstringwhich checks if one word is a substring
of another. Given two strings, sl and s2, write code to check if s2 is a rotation of sl using only one
call to isSubstring (e.g., "waterbottle" is a rotation of"erbottlewat").

```
public class StringRotation {

    public static boolean isRotation(String s1, String s2) {
        int len = s1.length();
        
        /* 1. Check that s1 and s2 are equal length and not empty */
        if (len == s2.length() && len > 0) {
            /* 2. Concatenate s1 and s1 */
            String s1s1 = s1 + s1;
            
            /* 3. Call isSubstring exactly once */
            return isSubstring(s1s1, s2);
        }
        
        return false;
    }

    /* Provided method from the problem description */
    private static boolean isSubstring(String big, String small) {
        return big.contains(small);
    }

    public static void main(String[] args) {
        String s1 = "waterbottle";
        String s2 = "erbottlewat";
        
        System.out.println("Is rotation? " + isRotation(s1, s2));
    }
}
```

Sorted Merge: You are given two sorted arrays, A and B, where A has a large enough buffer at the
end to hold B. Write a method to merge B into A in sorted order.

```
    class Solution {
        public void merge(int[] nums1, int m, int[] nums2, int n) {
            int p1 = m-1;
            int p2 = n-1;
            int p = m+n-1;
            for(int i=p;i>=0;i--)
            {
                if(p2<0)
                {
                    break;
                }
                if(p1>0 && nums1[p1] > nums2[p2])
                {
                    nums1[p] = nums1[p1];
                    p1--;
                    p--;
                }
                else
                {
                    nums1[p] = nums2[p2];
                    p2--;
                    p--;
                }
            }
            
        }
    }

    //time complexity - O(m+n) space - O(1)
```

Group Anagrams: Write a method to sort an array of strings so that all the anagrams are next to
each other.

```
    class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs.length == 0) return new ArrayList();
        Map<String, List> ans = new HashMap<String, List>();
        for (String s : strs) {
            char[] ca = s.toCharArray();
            Arrays.sort(ca);
            String key = String.valueOf(ca);
            if (!ans.containsKey(key)) ans.put(key, new ArrayList());
            ans.get(key).add(s);
        }
        return new ArrayList(ans.values());
    }
}
Time Complexity: O(NKlogK), where N is the length of strs, and K is the maximum length of a string in strs. The outer loop has complexity O(N) as we iterate through each string. Then, we sort each string in O(KlogK) time.

Space Complexity: O(NK), the total information content stored i
```

```
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs.length == 0) return new ArrayList();
        Map<String, List> ans = new HashMap<String, List>();
        int[] count = new int[26];
        for (String s : strs) {
            Arrays.fill(count, 0);
            for (char c : s.toCharArray()) count[c - 'a']++;

            StringBuilder sb = new StringBuilder("");
            for (int i = 0; i < 26; i++) {
                sb.append('#');
                sb.append(count[i]);
            }
            String key = sb.toString();
            if (!ans.containsKey(key)) ans.put(key, new ArrayList());
            ans.get(key).add(s);
        }
        return new ArrayList(ans.values());
    }
}
```

Search in sorted rotated array
```
 class Solution 
{
    public int search(int[] nums, int target) 
    {
      int start = 0;
      int end = nums.length-1;
      while(start<=end) //start and end same ho jayenge tab mid ho jayega and thats a base case
      {
          int mid = start + (end - start)/2;
          if(nums[mid]==target)
            return mid;
          else if(nums[mid] >= nums[start])//increasing order in first mid
          {
            if(target<nums[mid] && target >=nums[start])
              end = mid-1;
            else
              start= mid+1;
          }
          else//decreasing order in first mid
          {
            if(target>nums[mid] && target<=nums[end])
              start = mid+1;
            else
              end= mid-1;
          }   
      }
      return -1;
        
    }
}
//Time complexity: O(logn)

//space : O(1)
```

Sorted Matrix Search: Given an M x N matrix in which each row and each column is sorted in
ascending order, write a method to find an element

```
    public class SortedMatrixSearch {
    public boolean findElement(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return false;
        }

        int row = 0;
        int col = matrix[0].length - 1; // Start at top-right

        while (row < matrix.length && col >= 0) {
            if (matrix[row][col] == target) {
                System.out.println("Found at: (" + row + ", " + col + ")");
                return true;
            } else if (matrix[row][col] > target) {
                col--; // Move left
            } else {
                row++; // Move down
            }
        }

        return false;
    }

    public static void main(String[] args) {
        int[][] matrix = {
            {15, 20, 40, 85},
            {20, 35, 80, 95},
            {30, 55, 95, 105},
            {40, 80, 100, 120}
        };
        SortedMatrixSearch searcher = new SortedMatrixSearch();
        searcher.findElement(matrix, 55); // Output: Found at: (2, 1)
    }
}
```

# TESTING QUESTION
11.4 No Test Tools: How would you load test a webpage without using any test tools?
Hints: #313, #345

Measurement: We wrap the RestAssured.get() call with System.currentTimeMillis() to capture the Latency (time for a round-trip).
Throughput: This is the most important load-testing metric. It's calculated by dividing the total successful requests by the total wall-clock time in seconds.

11.5 Test a Pen: How would you test a pen?
Hints: #140, #164, #220
Functional - posiive - ink is flowing , cap is closing , able to write through pen
The Mechanism: If it’s a click pen, does it retract and extend every time? If it has a cap, does it click into place securely?
Surface Compatibility: Does it work on different types of paper (glossy, matte, recycled)? Does it work on cardboard or wood?
Drying Time: How long does it take for the ink to dry? If you rub your finger over it after 2 seconds, does it smudge?

Negative - not able to clear out whats written, 
boundary case - Extreme Pressure: Does the tip break if you press down very hard while writing?, Temperature: Does the pen leak if left in a hot car ($40^{\circ}\text{C}$)? Does the ink freeze or become too thick to write in sub-zero temperatures?
edge - after writing for some time, the flow of ink does not break, 

Non functional - Load - when written through same pen for a day, the pen still works smoothly
stress - trying to find the break point till pen works and see how many hours it can write , 
Storage/Life Span: If the pen is left uncapped for 12 hours, will it still write immediately, or does the nib dry out?
scalability - new refill is able to get filled in the body 
Durability (Drop Test): If the pen falls from a table (approx. 3 feet) onto a hard floor, does the casing crack? Does the ink cartridge leak?

usability - easier to handle - good grip, looks good
compatibility - able to get in box of standard size it is not too large or short - standard size
security - 

11.6 Test an ATM: How would you test an ATM in a distributed banking system?
Hints: #210, #225, #268, #349, #393

Functional - Positive 