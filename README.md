# 📘 Problem: Count Fixed-Bound Subarrays

## Problem Statement
You are given:
- An integer array `nums`
- Two integers `minK` and `maxK`.

A **fixed-bound subarray** of `nums` is a **contiguous** subarray that satisfies:
- The **minimum value** of the subarray is exactly `minK`.
- The **maximum value** of the subarray is exactly `maxK`.

You need to **return the total number of fixed-bound subarrays**.

---

## Examples

### Example 1
**Input:**  
`nums = [1,3,5,2,7,5]`, `minK = 1`, `maxK = 5`

**Output:**  
`2`

**Explanation:**  
The fixed-bound subarrays are `[1,3,5]` and `[1,3,5,2]`.

---

### Example 2
**Input:**  
`nums = [1,1,1,1]`, `minK = 1`, `maxK = 1`

**Output:**  
`10`

**Explanation:**  
Every subarray of `nums` is a fixed-bound subarray.  
Total number of subarrays = 1+2+3+4 = 10.

---

## Constraints
- `2 <= nums.length <= 10⁵`
- `1 <= nums[i], minK, maxK <= 10⁶`

---

# 💡 Approach

We want to **efficiently count** subarrays that:
- Contain **at least one** `minK` and `maxK`.
- Contain **only** elements between `minK` and `maxK` (inclusive).

### Key Observations
- If an element `nums[i]` is **outside** `[minK, maxK]`, then it **breaks** the subarray.
- Track the **last index** of:
  - An invalid (bad) element (`bad_idx`)
  - A `minK` element (`left_idx`)
  - A `maxK` element (`right_idx`)

At each position `i`, the number of new fixed-bound subarrays ending at `i` is:

```text
max(0, min(left_idx, right_idx) - bad_idx)
```

because:
- `bad_idx` is where last invalid value occurred (subarray must start after it).
- Both `minK` and `maxK` must have appeared between `bad_idx+1` and `i`.

### Steps
1. Initialize:
   - `bad_idx = -1`
   - `left_idx = -1`
   - `right_idx = -1`
   - `res = 0`
2. Loop through each element `nums[i]`:
   - If `nums[i]` is outside `[minK, maxK]`, update `bad_idx = i`.
   - If `nums[i] == minK`, update `left_idx = i`.
   - If `nums[i] == maxK`, update `right_idx = i`.
   - Add `max(0, min(left_idx, right_idx) - bad_idx)` to the result `res`.
3. Return `res`.

This works in **O(N)** time and **O(1)** extra space.

---

# ✅ Code

```cpp
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    long long countSubarrays(vector<int>& nums, int minK, int maxK) {
        long long res = 0;
        int bad_idx = -1, left_idx = -1, right_idx = -1;

        for (int i = 0; i < nums.size(); ++i) {
            if (!(minK <= nums[i] && nums[i] <= maxK)) {
                bad_idx = i;
            }

            if (nums[i] == minK) {
                left_idx = i;
            }

            if (nums[i] == maxK) {
                right_idx = i;
            }

            res += max(0, min(left_idx, right_idx) - bad_idx);
        }

        return res;
    }
};

Would you also like a **visual dry-run** example on `[1,3,5,2,7,5]` to make it even clearer? 🚀 (I can draw the tracking variables step-by-step if you want!)
