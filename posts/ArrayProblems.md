# Subarray SUM

[TOC]



### Problem : Count of subarray with sum S

Given an array of integers and an integer **k**, you need to find the total number of continuous subarrays whose sum equals to **k**. 

 https://leetcode.com/problems/subarray-sum-equals-k/ 

#### Algorithm

​        Brute force = O(n^2), just generate all continuous subarrays and store their sum. 
​        Can you do better? Yes, can we use array prefixes or suffixes?
​         Consider array ------------ 1 2 3 4 5 6
​        Generate prefix sum -------- 1 3 6 10 15 21
​        For each prefix sum, generate difference from target number 9 i.e.
​         ------------------------   -8 -6 -3 1 6 12
​         Keep all prefix sum and the index in hashmap. For each difference, see
​         if we saw that prefix sum earlier?
​         Diff of 1 was seen at index 0 i.e. if prefix-sum at 3 is subtracted from prefix sum at 0, then arget sum is obtained which gives us array [1,3]
​        Diff of 6 was seen at index 2 i.e. if prefix-sum at 4 is subtracted from prefix sum at 2, then arget sum is obtained which gives us array [3,4]
​        Time complexity O(N)
​        

### Problem : Count of subsequence with sum S, same element multiple times allowed

Ordering does not matter: https://leetcode.com/problems/combination-sum/ 

Ordering matters:  https://leetcode.com/problems/combination-sum-iv/ 

Both of these can be solved by recursive backtracking where you pick an element and reduce element value from target sum S and recursively try to find solution. If target sum is zero, you add the current path to valid subsequence. If ordering does not matter, then sort the values and insert in hashset, else keep each different permutation as new possible subsequence.

### Problem : Count of subsequence with sum S, each element can be used once

 https://leetcode.com/problems/combination-sum-ii/ 

Similarly recursive backtracking/dfs can be used to find the solution.

### Problem : All subsequence with sum S and length L of each subsequence

 https://leetcode.com/problems/combination-sum-iii/ 

 https://leetcode.com/problems/3sum/ (Three numbers summing upto S)

 https://leetcode.com/problems/3sum-with-multiplicity/  (duplicate numbers)

3 sum can be broken into 1 loop + 2 sum problem. 2 sum problem can be solved using either dictionary or using binary search over sorted array. For binary search, we check if sum of two pointers - lo and hi - is equal to S. If sum is less, then we increase lo else we reduce hi. 

Here's a very concise implementation of the idea taken from top voted answer at leetcode:

```java
public List<List<Integer>> threeSum(int[] num) {
    Arrays.sort(num);
    List<List<Integer>> res = new LinkedList<>(); 
    for (int i = 0; i < num.length-2; i++) {
        if (i == 0 || (i > 0 && num[i] != num[i-1])) {
            int lo = i+1, hi = num.length-1, sum = 0 - num[i];
            while (lo < hi) {
                if (num[lo] + num[hi] == sum) {
                    res.add(Arrays.asList(num[i], num[lo], num[hi]));
                    while (lo < hi && num[lo] == num[lo+1]) lo++;
                    while (lo < hi && num[hi] == num[hi-1]) hi--;
                    lo++; hi--;
                } else if (num[lo] + num[hi] < sum) lo++;
                else hi--;
           }
        }
    }
    return res;
}
```

### Problem : All subsequence with sum "closest to S" and length L of each subsequence

 https://leetcode.com/problems/3sum-closest/ 

Similar to 3 Sum problem, use 3 pointers to point current element, next element and the last element. If the sum is less than target, it means we have to add a larger element so next element move to the next. If the sum is greater, it means we have to add a smaller element so last element move to the second last element. Keep doing this until the end. Each time compare the difference between sum and target, if it is less than minimum difference so far, then replace result with it, otherwise keep iterating.

```java
public class Solution {
    public int threeSumClosest(int[] num, int target) {
        int result = num[0] + num[1] + num[num.length - 1];
        Arrays.sort(num);
        for (int i = 0; i < num.length - 2; i++) {
            int start = i + 1, end = num.length - 1;
            while (start < end) {
                int sum = num[i] + num[start] + num[end];
                if (sum > target) {
                    end--;
                } else {
                    start++;
                }
                if (Math.abs(sum - target) < Math.abs(result - target)) {
                    result = sum;
                }
            }
        }
        return result;
    }
}
```

Ref. : https://leetcode.com/problems/3sum-closest/discuss/7872/Java-solution-with-O(n2)-for-reference

### Problem : Longest consecutive subsequence

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

Your algorithm should run in O(*n*) complexity.

 https://leetcode.com/problems/longest-consecutive-sequence/ 

Solution: It's pretty easy to do in O(N log N) by just sorting the original array and then doing linear traversal.

Approach 2: We an build a dictionary. Inserting into dictionary takes O(1), so overall a hash table can be built in O(N).  For each number, you will see if it's increment/decrement number exists. Single lookup happens in O(1) and in worst-case, total N lookups, so overall O(N) lookup cost for each numbers. Now, if we do it for every number, then it will O(N^2) but the idea is to do it only for numbers which are not already part of larger sequence.

If we ensure to not touch any number if a number minus one exist in the list, then we will ensure we start from the bottom and go in one direction only i.e. no need to decrement from this number. 

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> num_set = new HashSet<Integer>();
        for (int num : nums) {
            num_set.add(num);
        }
       int longestStreak = 0;

        for (int num : num_set) {
            if (!num_set.contains(num-1)) {
                int currentNum = num;
                int currentStreak = 1;

                while (num_set.contains(currentNum+1)) {
                    currentNum += 1;
                    currentStreak += 1;
                }

                longestStreak = Math.max(longestStreak, currentStreak);
            }
        }

        return longestStreak;
    }
}
```
### Problem : 0-1 Knapsack Problem

Given a weight-array W and value-array V, you need to pick subset from W such that sum is <= S and value of elements in subset is maximum. Usually, stated as problem where a thief has to pack stuff in knapsack which can hold maximum weight S by choosing some items each with weight Wi and Value Vi. Thief obviously wants to maximize total value of all items while keeping weight least.

#### Solution

Consider W = [2 2 3 5], V = [10 20 200 50] and S = 4. Clearly we should just pick one item of value 200.

If we change V to [10 20 25 50] and S = 4. Clearly we should just pick first two items with combined value of 30.

We can be greedy and always pick the highest value item from remaining items but it's pretty simple to figure out it's not the right choice. So, we need deeper thought.

There are 2^n subsets. Any of these subset could be the final optimal subset of items to choose. It contains some unknown m items out of total N items and total value of these items is maximum V' and weight of these items is W'. We need to figure exact value of those m items. What can we say about this optimal subset:

Can we express this subset as a bottom-up solution:

For each item 'i', we can select it if it increases the value accumulated till now for weight S

dp[i,w] = maximum value that can be obtained using items (0,i) and maximum weight S

dp[i,w] = w < wi ? dp[i-1, w] : max( dp[i-1, w], dp[i-1, w-wi] + Vi)	

```c#
	for(int w = 0; w <= W; ++w)
    {
        B[0, w] = 0;
    }

	for(int i=0; i<N; ++i)
	{
		B[i,0] = 0;
	}
	
	for(int i=0; i<N; ++i)
	{
		for(int w=0; w<= W; ++w)
		{
			if(Weights[i] <= w)
			{
				B[i,w] = max(B[i-1, w], B[i-1, w-Weights[i]] + Values[i]);
			}
			else
			{
				B[i,w] = B[i-1,w];
			}
		}
	}
```
Time complexity = O(N.W)

B[n,W] is the maximal value that can be placed in knapsack. 

How to find actual items?

Let i=n and k=W 

if B[i,k] ≠ B[i−1,k] then 

​	mark the i th item as in the knapsack 

​	i = i−1, k = k-wi 

else 

​	i = i−1 // Assume the i th item is not in the knapsack 

### Problem : Equal Sum 2-Partition (Partition into two equal weight subsequence)

Partition an array of integers into two equal weighted partitions. Would it work if there are negative integers?

```
Input: [1, 5, 11, 5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].
```

 https://leetcode.com/problems/partition-equal-subset-sum/ 

This is exactly same as finding all subsequences with sum = TotalSum/2 and each element can be used only once. Recursive backtracking/dfs can be used.

However, this can also be solved using 0-1 knapsack problem where we try to pick some items to make sum = TotalSum/2. If sum can be made using all items, then definitely remaining items will have equal sum.

See more details here: https://leetcode.com/problems/partition-equal-subset-sum/discuss/90592/01-knapsack-detailed-explanation

> This problem is essentially let us to find whether there are several numbers in a set which are able to sum to a specific value (in this problem, the value is sum/2).
>
> Actually, this is a 0/1 knapsack problem, for each number, we can pick it or not. Let us assume dp[i][j] means whether the specific sum j can be gotten from the first i numbers. If we can pick such a series of numbers from 0-i whose sum is j, dp[i][j] is true, otherwise it is false.
>
> Base case: dp[0][0] is true; (zero number consists of sum 0 is true)
>
> Transition function: For each number, if we don't pick it, dp[i][j] = dp[i-1][j], which means if the first i-1 elements has made it to j, dp[i][j] would also make it to j (we can just ignore nums[i]). If we pick nums[i]. dp[i][j] = dp[i-1][j-nums[i]], which represents that j is composed of the current value nums[i] and the remaining composed of other previous numbers. Thus, the transition function is dp[i][j] = dp[i-1][j] || dp[i-1][j-nums[i]]



### Problem : Equal sum K-Partition (Partition into K-subsequence/subset of equal weight)

Partition an array into K-equal weighted partitions

 https://leetcode.com/problems/partition-to-k-equal-sum-subsets/ 

This is exactly same as find all subsequences with sum = TotalSum / K. Each element can be used only once.

### Problem : Pairs divisible by K

 https://www.hackerrank.com/challenges/divisible-sum-pairs/problem 

Find all (x, y) pairs in an array where (x + y) % k == 0

#### Solution:

If x % k = a, then find y where y % k = (k-a). If we get each number % k in a dictionary, then we can find pairs easily.



### Problem : Subarrays divisible by K

 https://leetcode.com/problems/subarray-sums-divisible-by-k/ 

#### Solution

Remember prefix sum? If Sum of (0, i) = 7 and Sum of (0, j) = 10 where i < j and we were looking for target 3, then (i, j) sums to 3. Basically, we calculate prefix sum and put it in dictionary along with its index. At every step, we calculate how far we are from target and check if that distance exists in dictionary. 

In this case, instead of prefix-sum, we will add {(prefix-sum) % target} to dictionary. 

### Problem : Subsequence divisible by K

#### Solution

Recursive. For each number, two recursive calls - with and without maintaining current_sum. Whenever current_sum % k == 0, add to list.

## Sliding Window problems

One template to solve all below questions: https://leetcode.com/problems/find-all-anagrams-in-a-string/discuss/92007/Sliding-Window-algorithm-template-to-solve-all-the-Leetcode-substring-search-problem. 

### Problem : Maximum in a sliding window of length K

Give an array **arr[]** of **N** integers and another integer **k ≤ N**. The task is to find the maximum element of every sub-array of size **k**.

 https://leetcode.com/problems/sliding-window-maximum/ 

#### Solution

Maintain a max-heap of size K. Each time sliding window moves we remove one element and add one element. At each step we query the max element.
    Building K size max-heap = K log K if we consider log K time in insertion of each element and K such elements, although correct time complexty is O(K) because K is from 1..K when we are building the heap of height K.
    Each addition/removal takes O(log K)
    Query take O(1)
    So, total time = O(K) + n * O(log K) = N log K
    

Can we do it in O(N)?

Yes, using DEQUE data structure. Pick each element and add to rear of deque. While adding to deque, eject all elements from front which are smaller than current element or those elements which occured k-indexes before and are out of sliding window. 

9 8 7 6 5 5 6 7 8 9 , k = 3

Deque:

9 -> 9 8 -> 9 8 7 -> 8 7 6 -> 7 6 5 -> 6 5 5 -> 6 -> 7 -> 8 -> 9 

Answer: Skip (k-1) elements and then pick front of queue at each step [9 8 7 6 6 7 8 9]

Excellent video to help visualization: https://www.youtube.com/watch?v=5VDQxLAlfu0



### Problem : Longest subarray with at most K-unique numbers

 https://www.geeksforgeeks.org/find-the-longest-substring-with-k-unique-characters-in-a-given-string/ 

https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/

https://leetfree.com/problems/longest-substring-with-at-most-k-distinct-characters.html

How do you change if it's exactly K or at least K?

#### Solution

All subarrays O(N^2)

This can be done in O(N) by keeping a left pointer and maintaining count of unique characters. Every time we have more than K unique characters, we increment left pointer until the k-unique character constraint is satisfied. 



```c#
	public static int lengthOfLongestSubstringKDistinct(String s, int k)
    {
        int result = 0;
        int i = 0;
        var map = new Dictionary<char, int>();
    	for (int j = 0; j < s.Length; j++)
        {
            char c = s[j];
            if (map.ContainsKey(c))
            {
                map.Add(c, map[c] + 1);
            }
            else
            {
                map.Add(c, 1);
            }

            if (map.Count <= k)
            {
                result = Math.Max(result, j - i + 1);
            }
            else
            {
                while (map.Count > k)
                {
                    char l = s[i];
                    int count = map[l];
                    if (count == 1)
                    {
                        map.Remove(l);
                    }
                    else
                    {
                        map.Add(l, map[l] - 1);
                    }
                    i++;
                }
            }

        }

        return result;
    }
```
### Problem : Longest subarray without repeating numbers

 https://leetcode.com/problems/longest-substring-without-repeating-characters/ 

## Problem : Longest substring with at most two distinct characters

 https://www.programcreek.com/2013/02/longest-substring-which-contains-2-unique-characters/ 

Problems : Same as Longest substring with at most k-distinct characters.

### Problem : Replace number with another number at most K-times to get length of longest repeating subarray

 https://leetcode.com/problems/longest-repeating-character-replacement/

Extremely clever sliding window problem

```c#
public static int characterReplacement(String s, int k)
{
    s = s.ToUpper();
    int len = s.Length;
    int[] count = new int[26];
    int start = 0, maxCount = 0, maxLength = 0;
    for (int end = 0; end < len; end++)
    {
        //max count of current element in current window
        maxCount = Math.Max(maxCount, ++count[s[end] - 'A']);
        //total element that can be changed in this window  = window size i.e. end-start+1 MINUS 'the element with maximum occurence' 
        while (end - start + 1 - maxCount > k)
        {
            //slide the window by moving left pointer 'start'
            count[s[start] - 'A']--;
            start++;
        }
        maxLength = Math.Max(maxLength, end - start + 1);
    }
    return maxLength;
}
```



### Problem : Two arrays - large and small, Find smallest window in large which contain all elements in small

 https://leetcode.com/problems/minimum-window-substring/ 

#### Solution

Leetcode solution explains it really well:

The question asks us to return the minimum window from the string S*S* which has all the characters of the string T*T*. Let us call a window `desirable` if it has all the characters from T*T*.

We can use a simple sliding window approach to solve this problem.

In any sliding window based problem we have two pointers. One ***right*** pointer whose job is to expand the current window and then we have the ***left*** pointer whose job is to contract a given window. At any point in time only one of these pointers move and the other one remains fixed.

The solution is pretty intuitive. We keep expanding the window by moving the right pointer. When the window has all the desired characters, we contract (if possible) and save the smallest window till now.

The answer is the smallest desirable window.

For eg. `S = "ABAACBAB" T = "ABC"`. Then our answer window is `"ACB"` and shown below is one of the possible desirable windows.

![img](https://leetcode.com/problems/minimum-window-substring/Figures/76/76_Minimum_Window_Substring_1.png)

**Algorithm**

1. We start with two pointers, left*l**e**f**t* and right*r**i**g**h**t* initially pointing to the first element of the string S*S*.

2. We use the right*r**i**g**h**t* pointer to expand the window until we get a desirable window i.e. a window that contains all of the characters of T*T*.

3. Once we have a window with all the characters, we can move the left pointer ahead one by one. If the window is still a desirable one we keep on updating the minimum window size.

4. If the window is not desirable any more, we repeat step \; 2*s**t**e**p*2 onwards.

   

### Problem : First string permutation is contained in second string

 https://leetcode.com/problems/permutation-in-string/description/ 

## String problems

### Problem : Generate all permutations / combinations of an array of strings / integers / characters

Permutations are not distinct i.e. [1,1,2] and [1,2,1] are counted as two

 https://leetcode.com/problems/permutations/ 



## Treeset Based

### Problem: K-Empty slots

You are given an n-bit vector with all bits as zero. Each bit turns 1 one-by-one every 1 second in random order and you're given the order. You have to tell how many second you need to wait before finding the situation where two bits turned 1 are at K distance and all bits within these two bits are 0. 

https://leetfree.com/problems/k-empty-slots.html

#### Solution:

Process bits in the order in which they turn 1. Every time a bit turns 1, check the bit at k-th distance on both sides. If it's 1, then check all bits inside those two are zero. This takes O(N^2) as O(N) traversal over input array and O(N/2) average traversal in each iteration. 

We can change this O(N/2) to O(log N) by using a sorted tree data structure. In Java, there's a datastructure called TreeSet that if given a number, it can tell you the next largest and next smallest number in O(log N). Internally, TreeSet uses a balanced Red-Black tree to maintain the sorted ordering of set. Being a set, it does not allow duplicates. We can leverage this to check if any bit smaller than current number is set. It will be the 1 at k-th distance if no other bit in-between is set.

#### Solution 2: 

The problem can be solved by thinking that for every window of length K, we need to find minimum in that window and the minimum should be greater than the left and right neighbors of window. This means that left and right neighbors existed before any of the members of window, so we had K empty slots at that point. This reduces to the problem of sliding window minimum which can be solved using deque.

We can also think of it as problem of finding a sliding window of length K+2 where first and last elements of the window are smaller than rest of the elements.



https://www.programcreek.com/2013/08/leetcode-problem-classification/