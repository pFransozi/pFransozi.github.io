---
layout: post
title:  array - theory and algorithms
date:   2023-04-22 21:00:00
description: array - theory and algorithms
tags: ["programming", "data structure", "array"]
language: en-us
---
## Introduction

No doubt that arrays are the most fundamental data structure in CS, that because it is a fixed-size, contiguous block of memory of the same data type. Those aspects give to an array O(1) time to store and access an element. Arrays work on an index system starting from 0 to (n-1), where n is the size of the array.

Like any data structure, an array also has its limitations. When the index is used to look up an element, time required is constant, which means O(1). Otherwise, when the algorithm looks up an element by value, it requires an entire traversal of an array, unless it is sorted in some way. An entire traversal of an array means O(n).

There are three additional limitations to consider when working with arrays: 
1. deleting an element from an array requires shifting all the subsequent elements one position to the left, which takes O(n) time. To avoid this, it's better to overwrite the value of the element you want to delete. 
2. inserting an element at the beginning of an array requires shifting all the other elements one position to the right, which is also an O(n) operation. Therefore, this should be done sparingly, and only when necessary. 
3. arrays have a fixed size, which means that their capacity is limited and cannot be changed once the array is created. In some cases, you may need to add more elements to the array than it can currently hold, which requires allocating a new, larger array. This operation takes O(n) time, where n is the size of the old array, as you need to copy all the existing elements from the old array to the new one. To avoid excessive copying, it is common to allocate a new array that is twice the size of the old one, which reduces the number of copy operations required. Nonetheless, this approach can still be time-consuming and inefficient for large arrays.

* **Index**
    * [Introduction](#introduction)
    * [Get product of all other elements](#get-product-of-all-other-elements)
    * [Locate smallest window to be sorted](#locate-smallest-window-to-be-sorted)
    * [~~Calculate maximum subarray sum~~](#calculate-maximum-subarray-sum)
    * [~~Find number of smaller elements to the right~~](#find-number-of-smaller-elements-to-the-right)
    * [References](#references)

## Get product of all other elements

This problem requires an algorithm that aims change each value of an array by the product of all other elements. For example, if an array as `a = [3, 2, 1]`, the result must be a new array as `arr = [2, 3, 6]`.

Based on that, let's start our development creating a test file to drive the coding. First test testes an empty array, calling a method that will be responsible for main task of get product of all other elements. But in this stage, the result must be an empty array as well.

Next I'll show the steps to create it, but the entire code can be access [here](https://github.com/pFransozi/algorithms/tree/main/py-array-get-product-of-all-other-elements){:target="_blank"}.

First test method.

~~~ python
import unittest

class TestsGetProductOfAllOtherElements(unittest.TestCase):

def test_an_empty_array(self):

    productAllOtherelements = main.GetProductOfAllOtherElements
    result = main.GetProductOfAllOtherElements.compute_product([])
    self.assertEqual([], result, "Not equal.")
~~~

As expected, test has broken. For solve that, it was created a python file, called main.py, with a class and a method responsible for computing the final array. Then, I create a class `GetProductOfAllOtherElements`, and a main method, `compute_product(self, nums)`.

~~~ python
class GetProductOfAllOtherElements:
    def compute_product(self, nums):
~~~

With that, the error has changed. Now `unittest` indicates an error in the method `GetProductOfAllOtherElements` because it does nothing. And it is ok because errors drive the development. To solve that, let's change `GetProductOfAllOtherElements` method to return the input argument `nums`.

~~~ python
def compute_product(self, nums):
    return nums
~~~

And then, `test_an_empty_array` test has passed. Next step is to create a test for one-element array. In this test case, the expected output should match the input array.

~~~ python
def test_an_array_with_one_element(self):
    productAllOtherelements = main.GetProductOfAllOtherElements
    result = main.GetProductOfAllOtherElements.compute_product([7])

    self.assertEqual([7], result, "Not equal.")
~~~

Let's include another test case to test a two-element array.

~~~ python
def test_an_array_with_one_element(self):
    productAllOtherelements = main.GetProductOfAllOtherElements
    result = main.GetProductOfAllOtherElements.compute_product([7,5])

    self.assertEqual([5, 7], result, "Not equal.")
~~~

Now the algorithm faces a more complex test case error, which means that the algorithm must be improved considering the logic described above: change each value of an array by the product of all other elements. But first, let's improve the test file, including a setUp stage, for initialize the class GetProductOfAllOtherElements.

~~~ python
class TestsGetProductOfAllOtherElements(unittest.TestCase):
    
    def setUp(self):
        self.GetProductOfAllOtherElements = main.GetProductOfAllOtherElements()

    def test_an_empty_array(self):

        result = self.GetProductOfAllOtherElements.compute_product([])

        self.assertEqual([], result, "Not equal.")

    def test_an_array_with_one_element(self):
        result = self.GetProductOfAllOtherElements.compute_product([7])

        self.assertEqual([7], result, "Not equal.")
    
    def test_an_array_with_two_elements(self):
        result = self.GetProductOfAllOtherElements.compute_product([7, 5])

        self.assertEqual([5, 7], result, "Not equal.")
~~~

The GetProductOfAllOtherElements class is now the focus, and the proposed solution leverages the precomputing technique to solve subarray problems. This involves solving each subarray individually and then combining the results to build the final solution.

First of all, let's include a test to verify if `nums` argument is empty or is an one-element array, if so, then return `nums` argument with no change. 

~~~ python
def compute_product(self, nums):
    
    if len(nums) < 2:
        return nums
~~~

Then, let's precompute the numbers before a given index `i`, prefix_precompute, and, the numbers after a given index `i`, posfix_precompute. At end let's build the result based on those two products.

~~~ python
def compute_product(self, nums):
    
    if len(nums) < 2:
        return nums

    prefix_precompute = []
    #
    for num in nums:
        if prefix_precompute:
            prefix_precompute.append(prefix_precompute[-1] * num)
        else:
            prefix_precompute.append(num)

    posfix_precompute = []
    
    for num in reversed(nums):
        if posfix_precompute:
            posfix_precompute.append(posfix_precompute[-1] * num)
        else:
            posfix_precompute.append(num)
    
    posfix_precompute = list(reversed(posfix_precompute))

    compute_result = []
    for i in range(len(nums)):
        if i == 0:
            compute_result.append(posfix_precompute[i + 1])
        elif i == len(nums) - 1:
            compute_result.append(prefix_precompute[i - 1])
        else:
            compute_result.append(prefix_precompute[i - 1] * posfix_precompute[i+1])

    return result
~~~

With that algorithm, `test_an_array_with_two_elements` test has passed. Then, let's include another two tests.

~~~ python
class TestsGetProductOfAllOtherElements(unittest.TestCase):
    
    def setUp(self):
        self.GetProductOfAllOtherElements = main.GetProductOfAllOtherElements()

    def test_an_empty_array(self):

        result = self.GetProductOfAllOtherElements.compute_product([])

        self.assertEqual([], result, "Not equal.")

    def test_an_array_with_one_element(self):
        result = self.GetProductOfAllOtherElements.compute_product([7])

        self.assertEqual([7], result, "Not equal.")
    
    def test_an_array_with_two_elements(self):
        result = self.GetProductOfAllOtherElements.compute_product([7, 5])

        self.assertEqual([5, 7], result, "Not equal.")

    def test_an_defined_array_a(self):
        result = self.GetProductOfAllOtherElements.compute_product([3, 2, 1])
        self.assertEqual([2, 3, 6], result, "Not Equal.")

    def test_an_defined_array_b(self):
        result = self.GetProductOfAllOtherElements.compute_product([1, 2, 3, 4, 5])
        self.assertEqual([120, 60, 40, 30, 24], result, "Not equal.")
~~~

When all tests have passed, it is possible to refactor the main code more safely.

~~~ python
class GetProductOfAllOtherElements:

    def is_less_than_two_elements(self, nums):
        if len(nums) < 2:
            return True

        return False
    
    def compute_element_and_insert(self, products, num):
        if products:
            return products[-1] * num
        else:
            return num

    def compute_prefix(self, nums):
        prefix = []
        for num in nums:
            prefix.append(self.compute_element_and_insert(prefix, num))

        return prefix
    
    def compute_posfix(self, nums):
        
        posfix = []
        for num in reversed(nums):
            posfix.append(self.compute_element_and_insert(posfix, num))
        
        posfix = list(reversed(posfix))
        return posfix
    
    def get_element(self, i, prefix, posfix, nums):
        
        if i == 0:
            return posfix[i + 1]
        elif i == len(nums) - 1:
            return prefix[i - 1]
        else:
            return prefix[i-1] * posfix[i + 1]
        
    
    def is_first_element(self, i):
        return i == 0
    
    def is_last_element(self, i, nums):
        return i == len(nums) - 1

    def compute_product(self, nums):
        if self.is_less_than_two_elements(nums):
            return nums

        prefix = self.compute_prefix(nums)
        posfix = self.compute_posfix(nums)
        
        result = []
        for i in range(len(nums)):
            if self.is_first_element(i):
                result.append(self.get_element(i, prefix, posfix, nums))
            elif self.is_last_element(i, nums):
                result.append(self.get_element(i, prefix, posfix, nums))
            else:
                result.append(self.get_element(i, prefix, posfix, nums))

        return result
~~~

[⇡](#introduction)

## Locate smallest window to be sorted

The algorithm aims to find the smallest subarray that is unsorted, returning an index from left side and other from right side. Considering the next array `a = [3, 7, 5, 6, 9]`, the result must be `left_index = 1` and `right_index = 3`, which means that the subarray from 1 to 3 is the smallest window that is unsorted.

Follow, the first solution has a O(nlogn) time complexity and O(n) space complexity. That complexity is related to the python function `sorted()`. As that function creates a new array, the space complexity is n, which means the size of the old array. And nlogn is related to the algorithm adopted by python function, which is [Timsort](https://en.wikipedia.org/wiki/Timsort) with a average time complexity as O(nlogn).

~~~ python

def window_nlogn(self, array):
        left, right = None, None
        s = sorted(array)
        
        for i in range(len(array)):
            if not self.is_eq(array[i], s[i]) and self.is_none(left):
                left = i
            elif not self.is_eq(array[i], s[i]):
                right = i

        return left, right
~~~

Frequently, when working with arrays, it's possible to create a faster algorithm by iterating through the elements and calculating minimum, maximum, or count. 

Next approach aims to improve space and time complexity. The algorithm has O(1) as space complexity because no new array is created. All operations are over the same array that is passed to the function. And the time complexity is linear, O(n), because the algorithm just traverses the array. 

~~~ python
    def window_n(self, array):
        left, right= None, None
        n = len(array)
        max_seen, min_seen = -float("inf"), float("inf")
        
        for i in range(n):
            max_seen = max(max_seen, array[i])
            if array[i] < max_seen:
                right = i
        
        for i in range(n - 1, -1, -1):
            min_seen = min(min_seen, array[i])
            if array[i] > min_seen:
                left = i
        
        return left, right
~~~

The full code can be accessed [here](https://github.com/pFransozi/algorithms/tree/main/py-locate-smallest-window-to-be-sorted).

[⇡](#introduction)

## Calculate maximum subarray sum

## Find number of smaller elements to the right

[⇡](#introduction)

## References

Cormen, T. H., Leiserson, C. E., Rivest, R. L., & Stein, C. (2022). Introduction to Algorithms, fourth edition. MIT Press.

Miller, A., & Wu, L. (2019). Daily Coding Problem: Get Exceptionally Good at Coding Interviews by Solving One Problem Every Day.

[en.wikipedia.org: Array_(data_structure)](https://en.wikipedia.org/wiki/Array_(data_structure)){:target="_blank"}.

[⇡](#introduction)