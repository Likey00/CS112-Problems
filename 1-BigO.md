# Reverse Arrays
Write a method that reverses a given character array, and analyze the space and runtime complexity of your algorithm. Try to find a solution which does not create a new array, but instead modifies the given array in place. 

**Example input:** `['H','a','n','n','a','h']`

**Example output:** `['h','a','n','n','a','H']`

<details>
    <summary>Click to reveal solution</summary>
<br>

## Naive Approach

A valid solution would be to create a new array, then iterate through the given array and put each element into the new array in reverse order. Finally, copy the new array over to the given array.

```java
public static void reverse(char arr[]) {
    int n = arr.length; //We will use arr.length a lot

    char[] reversed = new char[n];

    for (int i = 0; i < n; i++) {
        //The index of the last element is n-1, so we can access the ith element from the end with n-1-i
        reversed[n-1-i] = arr[i];
    }

    //Copy everything back over from reversed to arr
    for (int i = 0; i < n; i++) arr[i] = reversed[i];
}
```
If n is the size of the input array, the time complexity is O(n) since we traverse it once, and the space complexity is O(n) since we created a new array of size n.
<br>

## Efficient Solution

An efficient solution would be to go through half the array, and swap each element at index i with the element i away from the end of the array. Note that an additional variable is required to swap values, but no new array is necessary.

```java
public static void reverse(char arr[]) {
    int n = arr.length; //We will use arr.length a lot

    //Iterate through only the first half
    for (int i = 0; i < n/2; i++) {
        //Use a temp variable so the value of arr[i] is not lost while swapping
        char temp = arr[i];
        arr[i] = arr[n-1-i];
        arr[n-1-i] = temp;
    }
}
```
If n is the size of the input array, the time complexity is O(n) since we have n/2 * 4 = 2n array accesses in total. The space complexity is O(1) since we only used an extra variable, not a new array of size n.
</details>
<br>

# Merge Sorted Arrays
Write a method that merges two already sorted int arrays nums1 and nums2 into one large sorted array nums1. Assume that nums1 has enough extra space to fit the solution, and you are provided the size of nums1 without extra space as n and the size of nums2 as m. Try to come up with a method that does not use any extra space (very tricky!)

**Example input:** `[1,2,3,0,0,0]` and `[2,5,6]`, `n = 3` and `m = 3`

**Example output:** `[1,2,2,3,5,6]`

<details>
<summary>Click to reveal solution</summary>
<br>

## Naive Approach
A valid solution would be to fill up the empty spaces of nums1 with the elements of nums2, and then sort the whole array. 

```java
public static void merge(int nums1[], int nums2[], int n, int m) {
    //Iterate through the indices of nums2
    for (int i = 0; i < m; i++) {
        //By shifting over the indices by n, we can fit all the elements of nums2 in the empty spaces of nums1
        nums1[n+i] = nums2[i];
    }

    Arrays.sort(nums1);
}
```
Because sorting is O(nlogn) where n is the size of the array, and the size of the array here is n+m, this solution would have a time complexity of O((n+m)log(n+m)). No new arrays were created, so the space complexity would be O(1). 
<br>

## More Efficient Approach
A more efficient solution would be to maintain variables corresponding to the current indices of nums1 and nums2, and create a new array of size n+m. Since nums1 and nums2 are already sorted, it is possible to compare the values at these indices, add the smaller one to the array, and increment that index since the value was used up. Continuing this process, we will always be taking the smallest option between nums1 and nums2 to add to the array, and end up with a sorted array. 

```java
public static void merge(int nums1[], int nums2[], int n, int m) {
    int merged[] = new int[n+m];
    int ptr1 = 0, ptr2 = 0; 

    //As long as there are still elements to consider in both arrays
    while (ptr1 < n && ptr2 < m) {
        if (nums1[ptr1] <= nums2[ptr2]) {
            //If the element in nums1 is less, set the next element of merged to it
            merged[ptr1+ptr2] = nums1[ptr1];
            ptr1++; //Need to increment ptr1 since this element is used up
        }
        else {
            //Otherwise, do the same for nums2 and ptr2
            merged[ptr1+ptr2] = nums2[ptr2];
            ptr2++;
        }
    }

    //If there are leftover elements in nums1, add them all to the end 
    while (ptr1 < n) {
        merged[ptr1+ptr2] = nums1[ptr1];
        ptr1++;
    }
    
    //Same for nums2
    while (ptr2 < m) {
        merged[ptr1+ptr2] = nums2[ptr2];
        ptr2++;
    }
    
    //Copy merged into nums1
    for (int i = 0; i < n+m; i++) nums1[i] = merged[i];
}
```
Because in the worst case you will visit each element in nums1 and each element in nums2, the time complexity is O(n+m). Because an additional array of size n+m was needed, the space complexity is O(n+m).
<br>

## Most Efficient Solution (Clever)
The most efficient solution is very similar to the previous one, but you can avoid creating a new array by noticing the extra space in nums1 is at the end. Thus, by filling in the array from the end to the beginning, it is possible to use nums1 like the merged array without worrying about overwriting numbers which are in use.

```java
public static void merge(int nums1[], int nums2[], int n, int m) {
    //Note that there is no merged array now, we will use nums1
    int ptr1 = n-1, ptr2 = m-1; //This time they start at the end of their arrays

    //Now we go until ptr1 or ptr2 is less than 0
    while (ptr1 >= 0 && ptr2 >= 0) {
        //Now we add the greater element since we're going backwards
        if (nums1[ptr1] >= nums2[ptr2]) {
            //I need to add 1 to the pointer sum now, since n-1 + m-1 = n+m-2, and I need n-m-1 for the end
            nums1[ptr1+ptr2+1] = nums1[ptr1];
            ptr1--; //Decrease this time since we're going in reverse
        }
        else {
            nums1[ptr1+ptr2+1] = nums2[ptr2];
            ptr2--;
        }
    }

    //If there are leftover elements in nums1, add them all
    while (ptr1 >= 0) {
        nums1[ptr1+ptr2+1] = nums1[ptr1];
        ptr1--;
    }
    
    //Same for nums2
    while (ptr2 >= 0) {
        nums1[ptr1+ptr2+1] = nums2[ptr2];
        ptr2--;
    }
}
```
The time complexity is still O(n+m) since we did the same process in a different order, but the space complexity is now O(1) since we did not need a new array.
</details>
<br>

# Two Sum
Write a method that takes in a **sorted** array of integers and an integer target value, and returns two elements of the array which add up to this integer target. Assume that there will always be two elements which add to the target.

**Example Input:** `[2, 7, 11, 15]`, and `target = 9`

**Example Output:** `[2, 7]`

<details>
    <summary>Click to reveal solution</summary>
<br>

## Naive Approach
A valid solution would be to iterate through every combination of 2 elements, and check if any of them are equal to the target. If you find two elements which sum to target, return them.

```java
public static int[] twoSum(int[] arr, int target) {
    int n = arr.length;

    //Note how j starts from i+1. This avoids combinations like (1, 2) and (2, 1) both being checked
    for (int i = 0; i < n; i++) {
        for (int j = i+1; j < n; j++) {
            //If a pair matches the target, I can just return it
            if (arr[i] + arr[j] == target) return new int[] {arr[i], arr[j]};
        }
    }
    
    //Need to return something arbitrary for program to compile, this code won't be reached
    return new int[] {0, 0};
}
```
The time complexity here is O(n^2), since we are doing 2 array accesses per inner loop, and the inner loop is run 1+2+3...n-1 times, which evaluates to n(n-1)/2 * 2 operations. The space complexity is O(1) since no new arrays were created.
<br>

## Efficient Solution
The most efficient solution is something commonly known as a two pointer approach. Start with 2 variables which store the index of the beginning and end of the array. Test their sum, and if the sum is greater than the target, you can leverage the sortedness of the list and reduce the end index by 1. If the sum is less than the target, increase the beginning index by 1. Reducing the end index will decrease the sum, and increasing the begin index will increase the sum. Using this method, the sum hones into the target sum.

```java
public static int[] twoSum(int[] arr, int target) {
    int n = arr.length;

    int lo = 0, hi = n-1;

    //Keep going until lo exceeds hi, which means nothing was found
    while (lo <= hi) {
        //Increment lo if we need to increase sum, decrement hi if we need to decrease
        if (arr[lo] + arr[hi] < target) lo++;
        else if (arr[lo] + arr[hi] > target) hi--;

        //If we've found the target, we can just return
        else return new int[] {arr[lo], arr[hi]};
    }

    return new int[] {0, 0};
}
```
The time complexity here is O(n), since in the worst case lo and hi meet in the middle, which means that the while loop runs n/2 times and there are 2 array accesses per loop. The space complexity is O(1) since no new arrays were created. 
</details>
<br>

# Task Scheduler (Hard)
You are given a character array tasks, representing the tasks a CPU needs to do, where each letter represents a different task. Tasks could be done in any order. Each task is done in one unit of time. For each unit of time, the CPU could complete either one task or be idle. However, there is a non-negative integer n that represents the cooldown period between two same tasks (the same letter in the array). That is, there must be at least n units of time between any two same tasks. Return the least number of units of time that the CPU will take to finish all the given tasks. You can write/test your solution at the following leetcode link: https://leetcode.com/problems/task-scheduler/

**Example Input:** `['A','A','A','B','B','B']`, `n = 2`

**Example Output:** 8

**Explanation:** The following task order is optimal: `A, B, Idle, A, B, Idle, A, B`. This is 8 units of time.

<details>
<summary>Click to reveal solution</summary>

## Solution
A good way to solve this problem involves thinking about wasted time. The time where the CPU is idle is considered wasted time, since no task is being executed. Since the same task requires n units of time before it can be executed again, we can begin by considering the most frequent letter alone, which will result in (max frequency - 1) * n idle units of time. You can think of groups of n units of time sandwiched between this letter. Now we can slot the rest of the tasks in, and reduce the wasted time in the process. If the element has the same frequency as the max frequency, it will remove (max frequency - 1) units of idle time, since the last instance will go at the end, which is not considered wasted time. Otherwise, the element will remove (its frequency) units of idle time, since each instance of the element will use up one unit of wasted time and be evenly spaced. At the end, we can just return the number of elements + wasted time.

```java
public static int taskScheduler(char[] tasks, int n) {
    //Store the frequencies of each character, and sort them
    int[] frequencies = new int[26];
    for (char c : tasks) frequencies[c-'A']++;
    Arrays.sort(frequencies);
    
    //Initially, only consider max frequency
    int maxFreq = frequencies[25];
    int wastedTime = (maxFreq - 1) * n;
    
    //Go through all the other frequencies
    for (int i = 0; i < 25; i++) {
        if (frequencies[i] == maxFreq) wastedTime -= (maxFreq - 1);
        else wastedTime -= frequencies[i];
    }
    
    //Use Math.max with 0 since negative wasted time is just 0 wasted time
    return tasks.length + Math.max(0, wastedTime);
}
```
If n is the size of the input array, this algorithm will have a runtime of O(n), since we loop through the whole input array once to calculate the frequencies. Even though we created a new array, our space complexity is O(1) since the new array will always be of size 26 regardless of our input size.
</details>
<br>

# Largest Rectangle in Histogram (Insane)
Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram. Refer to the following leetcode link for a visual and to write/test your code: https://leetcode.com/problems/largest-rectangle-in-histogram/

NOTE: I was not personally able to come up with an easily explainable solution faster than O(n^2). This is not fast enough to pass the leetcode test cases, so don't stress if you are getting TLE.

<details>
<summary>Click to reveal solution</summary>

## My Solution
This is not the most efficient solution, but it is one I feel I can explain clearly. I am essentially trying every combination of 2 indices, and returning the max area rectangle of all of them. In order to compute the rectangle between two indices, I need to know the min in that range. Finding the min from scratch every time would be inefficient, but luckily since I am stepping through the array, I can maintain the current minimum in the range I am working, and not waste runtime. 

```java
public int largestRectangleArea(int[] heights) {
    int n = heights.length;
    int largest = 0; //Stores current largest area

    //i represents the starting index of my range 
    for (int i = 0; i < n; i++) {
        //I keep a running min, starting at index i
        int currentMin = heights[i];
        
        //j represents the ending index, and starts at i for 1 cell rects
        for (int j = i; j < n; j++) {
            //Update the currentMin based on heights[j]
            currentMin = Math.min(currentMin, heights[j]);
            
            //The rectangle from i to j is (j-i+1) * currentMin
            largest = Math.max(largest, (j-i+1)*currentMin);
        }
    }
    
    //Return the largest rectangle area seen
    return largest;
}
```
If n is the length of the heights array, the runtime of this solution is O(n^2) since the inner loop runs 1 + 2 + 3... + n = n(n+2)/2 times, and inside this loop we are only doing constant time operations. The space complexity is O(1) since we only create 2 new variables.

## Efficient Solution
Refer to the following youtube video for a full explanation of the efficient solution: https://www.youtube.com/watch?v=ZmnqCZp9bBs
</details>