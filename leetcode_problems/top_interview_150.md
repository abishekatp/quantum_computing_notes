# Array / String

## 88. Merge Sorted Array
You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

```go
func merge(nums1 []int, m int, nums2 []int, n int)  {
    i := 0
    j := 0
    final := []int{}
    for i<m && j < n {
        if nums1[i] < nums2[j]{
            final = append(final, nums1[i])
            i++
        }else {
            final = append(final, nums2[j])
            j++
        }
    }
    for i < m {
        final = append(final, nums1[i])
        i++
    }
    for j < n {
        final = append(final, nums2[j])
        j++
    }
    for k:=0; k < m+n; k++{
        nums1[k] = final[k]
    }
}
```

##  27. Remove Element

Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.

Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the elements which are not equal to val. The remaining elements of nums are not important as well as the size of nums.
Return k.

```go
func removeElement(nums []int, val int) int {
    k:=0
    for i:=0; i< len(nums);{
        if nums[i] != val{
            nums[k] = nums[i]
            i++
            k++
        }else{
            i++
        }
    }
    return k
}
```

## 26. Remove Duplicates from Sorted Array

Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Then return the number of unique elements in nums.

Consider the number of unique elements of nums to be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the unique elements in the order they were present in nums initially. The remaining elements of nums are not important as well as the size of nums.
Return k.


```go
func removeDuplicates(nums []int) int {
	uniqueMap := make(map[int]bool)
	k := 0
	for i := 0; i < len(nums); {
		if _, ok := uniqueMap[nums[i]]; !ok {
			uniqueMap[nums[i]] = true
			nums[k] = nums[i]
			k++
			i++
		} else {
			i++
		}
	}
	return k
}
```

## 80. Remove Duplicates from Sorted Array II

Given an integer array nums sorted in non-decreasing order, remove some duplicates in-place such that each unique element appears at most twice. The relative order of the elements should be kept the same.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the first part of the array nums. More formally, if there are k elements after removing the duplicates, then the first k elements of nums should hold the final result. It does not matter what you leave beyond the first k elements.

Return k after placing the final result in the first k slots of nums.

Do not allocate extra space for another array. You must do this by modifying the input array in-place with O(1) extra memory.


```go
func removeDuplicates(nums []int) int {
    // elements before the index k are the valid elements
	k := 0
	for i := 0; i < len(nums); {
		count := 1
        // check for at most two duplicated before the index k
		for j := 0; j < k; j++ {
			if nums[j] == nums[i] {
				count++
			}
		}
		if !(count > 2) {
			nums[k] = nums[i]
			k++
			i++
		} else {
			i++
		}
	}
    return k
}
```

# Two Pointers

## 125. Valid Palindrome
A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string s, return true if it is a palindrome, or false otherwise.

```go
import "unicode"
func isPalindrome(s string) bool {
	i := 0
    f := []rune(strings.ToLower(s))
	j := len(s) - 1
	for j > i {
        r1:= f[i]
        r2:= f[j]
		first := unicode.IsLetter(r1) || unicode.IsDigit(r1)
		second := unicode.IsLetter(r2) || unicode.IsDigit(r2)
		if !(first) {
			i++
		}
		if !(second) {
			j--
		}
		if first && second && r1 == r2 {
			i++
			j--
		} else if first && second {
			return false
		}

	}
	return true
}
```

## 167. Two Sum II - Input Array Is Sorted

Given a 1-indexed array of integers numbers that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number. Let these two numbers be numbers[index1] and numbers[index2] where 1 <= index1 < index2 <= numbers.length.

Return the indices of the two numbers, index1 and index2, added by one as an integer array [index1, index2] of length 2.

The tests are generated such that there is exactly one solution. You may not use the same element twice.

Your solution must use only constant extra space.

Example 1:

Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].

```go
func twoSum(numbers []int, target int) []int {
    for i:=0; i< len(numbers); i++{
        for j:=0; j<i; j++{
            if numbers[i] + numbers[j] == target{
                return []int{j+1, i+1}
            }
        }
    }
    return []int{-1, -1}
}
```

## 209. Minimum Size Subarray Sum

Given an array of positive integers nums and a positive integer target, return the minimal length of a 
subarray
 whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.


Example 1:

Input: target = 7, nums = [2,3,1,2,4,3]
Output: 2
Explanation: The subarray [4,3] has the minimal length under the problem constraint.

```go
func minSubArrayLen(target int, nums []int) int {
	// compute running total
	for i := 1; i < len(nums); i++ {
		nums[i] = nums[i] + nums[i-1]
	}
    
	// compute sub array total using the running total
	return binaryTree(int(math.Round(float64(len(nums)/2))), nums, target)
}

func binaryTree(sub int, nums []int, target int) int {
    if sub >= len(nums){
        return 0
    }

    isTrue := false
    for i := len(nums) - 1; i >= sub; i-- {
        if nums[i]-nums[i-sub] >= target {
            isTrue  = true
        }
    }

    if nums[sub-1] >= target{
        isTrue = true
    }
    if (sub == 1) && isTrue{
        return sub
    }else if sub ==1{
        return 0
    }

    if isTrue{
        lessSub := binaryTree(int(math.Round(float64(sub/2))), nums, target)
        if lessSub!=0{
            return lessSub
        }else{
            return sub
        }
    }
    return binaryTree(sub + int(math.Round(float64(sub/2))), nums, target)
}
```