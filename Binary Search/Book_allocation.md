#  Allocate Books or Book Allocation | Hard Binary Search


**Solution Link:** 

- [Youtube - Allocate books](https://www.youtube.com/watch?v=Z0hwjftStI4&list=PLgUwDviBIf0pMFMWuuvDNMAkoQFi-h0ZF&index=19)

## Similar Problems
- [Youtube - Painters partition](https://www.youtube.com/watch?v=thUd_WJn6wk&list=PLgUwDviBIf0pMFMWuuvDNMAkoQFi-h0ZF&index=20)
- [ Youtube - Split array largest sum](https://www.youtube.com/watch?v=thUd_WJn6wk&list=PLgUwDviBIf0pMFMWuuvDNMAkoQFi-h0ZF&index=20)
-[Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/submissions/1872013014/)


## What is the problem asking?
Given an array pages of n integers, where pages[i] represents the number of pages in the i-th book, and an integer k representing the number of students, allocate all the books to the students so that each student gets at least one book, each book is allocated to only one student, and the allocation is contiguous.

Allocate the books to m students in such a way that the maximum number of pages assigned to a student is minimized. If the allocation of books is not possible, return -1.

## Solution

### Approach
 - Here we first need to undrstand that a minimum amount of pages that can be allocated to a student is the maximum value in the pages array, else some of the books cannot be allocated and the maximum pages that can be allocated to a student is the SUM(pages[])
 - now we have to minimize the number of pages being allocated, since now we have got our range [min(pages[]), SUM(pages[])].
 - now we have to do just a binary search in that range and check if the pages at the mid level get us upto `k` students or not
 - if at a particular point our split is < m then we will move towards left , else towards right because (more towards the right means more pages and less students can be allocated)
 - 

 ### Code 
```java
class Solution {

    public int maxSplits(int arr[], int maxSum){
        int i=0;
        int subArr = 0;
        int tillSum = 0;
        while(i < arr.length){
            tillSum += arr[i];
            if(tillSum > maxSum){
                subArr++;
                tillSum = arr[i];
            }

            i++;
        }

        if(tillSum <= maxSum){
            subArr++;
        }


        return subArr;
    }

    public int findPages(int[] pages, int k) {
       
     int max = Integer.MIN_VALUE, sum=0;
     for(int num: pages){
        sum += num;
        max = Math.max(max, num);
     }

     int start = max, end = sum;

     while(start <= end){
        int mid = (start+end)/2;
        int splits = maxSplits(pages, mid);

        // System.out.println("For "+mid+" pages -> "+splits);

        if(splits == k){
            end = mid-1;
            continue;
        }

        if(splits < k){
            end = mid-1;
        }else{
            start = mid+1;
        }
     }

     return start;


        
    }
}
```