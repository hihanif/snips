public class Solution {
	public int[] searchRange(int[] A, int target) {
		int start = Solution.firstGreaterEqual(A, target);
		if (start == A.length || A[start] != target) {
			return new int[]{-1, -1};
		}
		return new int[]{start, Solution.firstGreaterEqual(A, target + 1) - 1};
	}

	//find the first number that is greater than or equal to target.
	//could return A.length if target is greater than A[A.length-1].
	//actually this is the same as lower_bound in C++ STL.
	private static int firstGreaterEqual(int[] A, int target) {
		int low = 0, high = A.length;
		while (low < high) {
			int mid = low + ((high - low) >> 1);
			//low <= mid < high
			if (A[mid] < target) {
				low = mid + 1;
			} else {
				//should not be mid-1 when A[mid]==target.
				//could be mid even if A[mid]>target because mid<high.
				high = mid;
			}
		}
		return low;
	}
}
==

    
    // pure bs
    int bs(int[] nums, int target, int left, int right, int shift) {
        if (left > right) return -1;
        int mid = left + (right - left)/2;
        int newmid = (mid + shift) % nums.length;
        
        if (nums[newmid] == target) return newmid;
        if (nums[newmid] < target) return bs(nums, target, mid + 1, right, shift);
        return bs(nums, target, left, mid - 1, shift);
    }

==
class Solution {
    // find lower bound in the array
    public int findMin(int[] nums) {
        if (nums.length == 0) return -1;
        
        int lo = 0, hi = nums.length - 1;
        while (lo < hi) {
            int mid = lo + ((hi - lo) >> 1);
            if (nums[mid] <= nums[hi]) { // go left. 
                hi = mid;            
            } else {
                lo = mid + 1;
            }
        }
        
        return nums[lo];
    }
}

==
  Find K Closest Elements

class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        List<Integer> resultList = new ArrayList<>();
        
        if (arr.length == 0 || k == 0) return resultList;
        
        int left = 0, right = arr.length - k;
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (x - arr[mid] > arr[mid + k] - x) { // greater indicates that there is a chance of better range on the right side
                left = mid + 1;
            } else { // less than or equal.
                right = mid;
            }
        }
        
        
        for (int i = left;  i < left + k; i++) {
            resultList.add(arr[i]);
        }
        
        return resultList;
    }
}

==
  Search in a Sorted Array of Unknown Size

class Solution {
    public int search(ArrayReader reader, int target) {
        if (reader.get(0) == target) return 0;
        int hi = 1, lo = 1;
        while(reader.get(hi) < target) {
            lo = hi;
            hi *= 2;
        }
        
        while (lo <= hi) {
            int mid = lo + ((hi - lo) >>1);
            int midValue = reader.get(mid);
            if (midValue == target) return mid;
            if (midValue > target) {
                hi = mid - 1;
            } else {
                lo = mid + 1;
            }
        }
        
        return -1;
    }
}

