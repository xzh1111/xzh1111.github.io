# 寻找两个正序数组中位数


## 简单实现
寻找两个正序数组的中位数可以通过归并排序的思想，先将两个数组合并为一个有序数组，然后根据数组的长度是奇数还是偶数来找出中位数。以下是一个简单的实现：

```go

package main

import "fmt"

func findMedianSortedArrays(nums1 []int, nums2 []int) float64 {
    mergedNums := merge(nums1, nums2)
    length := len(mergedNums)
    if length%2 == 0 {
        return float64(mergedNums[length/2-1]+mergedNums[length/2]) / 2.0
    } else {
        return float64(mergedNums[length/2])
    }
}

func merge(nums1 []int, nums2 []int) []int {
    merged := make([]int, len(nums1)+len(nums2))
    i, j, k := 0, 0, 0
    for i < len(nums1) && j < len(nums2) {
        if nums1[i] < nums2[j] {
            merged[k] = nums1[i]
            i++
        } else {
            merged[k] = nums2[j]
            j++
        }
        k++
    }
    for i < len(nums1) {
        merged[k] = nums1[i]
        i++
        k++
    }
    for j < len(nums2) {
        merged[k] = nums2[j]
        j++
        k++
    }
    return merged
}

func main() {
    nums1 := []int{1, 3}
    nums2 := []int{2}
    fmt.Println(findMedianSortedArrays(nums1, nums2))  // 输出: 2

    nums1 = []int{1, 2}
    nums2 = []int{3, 4}
    fmt.Println(findMedianSortedArrays(nums1, nums2))  // 输出: 2.5
}
```



## 二分查找

```go
package main

import (
	"fmt"
	"math"
)

func findMedianSortedArrays(nums1 []int, nums2 []int) float64 {
	if len(nums1) > len(nums2) { // 首先确保nums1不比nums2长
		nums1, nums2 = nums2, nums1
	}
	m, n := len(nums1), len(nums2)
	imin, imax, halfLen := 0, m, (m+n+1)/2
	for imin <= imax {
		i := (imin + imax) / 2
		j := halfLen - i
		//  二分查找在nums1中找到一个位置i，使得nums1[i-1] <= nums2[j]和nums2[j-1] <= nums1[i]
		if i < m && nums2[j-1] > nums1[i] {
			imin = i + 1
		} else if i > 0 && nums1[i-1] > nums2[j] {
			imax = i - 1
		} else {
			var maxOfLeft float64
			if i == 0 { // nums1都在右边，maxOfLeft在nums2
				maxOfLeft = float64(nums2[j-1])
			} else if j == 0 {
				maxOfLeft = float64(nums1[i-1])
			} else {
				maxOfLeft = math.Max(float64(nums1[i-1]), float64(nums2[j-1]))
			}
			if (m+n)%2 == 1 { // 奇数, 返回中间位置
				return maxOfLeft
			}

			var minOfRight float64
			if i == m { // nums1都在左边，minOfRight在nums2
				minOfRight = float64(nums2[j])
			} else if j == n {
				minOfRight = float64(nums1[i])
			} else {
				minOfRight = math.Min(float64(nums1[i]), float64(nums2[j]))
			}
			return (maxOfLeft + minOfRight) / 2.0
		}
	}
	return 0
}

func main() {
	nums1 := []int{1, 3}
	nums2 := []int{2}
	fmt.Println(findMedianSortedArrays(nums1, nums2))  // 输出: 2

	nums1 = []int{1, 2}
	nums2 = []int{3, 4}
	fmt.Println(findMedianSortedArrays(nums1, nums2))  // 输出: 2.5
}
```

这个问题的关键在于理解我们正在寻找什么。我们需要找到两个数组中的中位数，也就是说，我们需要找到一个位置，使得***左边***的所有元素都小于***右边***的所有元素，并且左边的元素数量等于右边的元素数量（当总数为偶数时）或者多一个（当总数为奇数时）。

为了找到这个位置，我们可以使用二分查找的方法。我们首先在较短的数组中找到一个位置i，然后在较长的数组中找到一个位置j，使得j = (m+n+1)/2 - i。这样，我们就可以保证左边的元素数量等于右边的元素数量（当总数为偶数时）或者多一个（当总数为奇数时）。

然后，我们需要检查这个位置是否满足条件，也就是说，我们需要检查`nums1[i-1]是否<= nums2[j]`，并且`nums2[j-1]是否<= nums1[i]`。如果满足条件，那么我们就找到了正确的位置。如果不满足条件，那么我们就需要调整i的位置，如果nums1[i-1]大于nums2[j]，那么我们需要减小i的值（`imax = i - 1`），如果nums2[j-1]大于nums1[i]，那么我们需要增大i的值(`imin = i + 1`)。

最后，当我们找到正确的位置后，我们就可以计算中位数了。如果总数为奇数，那么中位数就是左边的最大值，也就是max(nums1[i-1], nums2[j-1])。如果总数为偶数，那么中位数就是左边的最大值和右边的最小值的平均值，也就是(max(nums1[i-1], nums2[j-1]) + min(nums1[i], nums2[j])) / 2。

这个方法的时间复杂度为O(log(min(m, n)))，其中m和n分别是两个数组的长度，因为我们每次都在较短的数组中进行二分查找，所以时间复杂度取决于较短的数组的长度。

### 边界情况考虑

- 当i为0时，表示nums1的所有元素都在右侧，此时左侧的最大值应该是nums2[j-1]。

- 当i为m（nums1的长度）时，表示nums1的所有元素都在左侧，此时右侧的最小值应该是nums2[j]。

- 当j为0时，表示nums2的所有元素都在右侧，此时左侧的最大值应该是nums1[i-1]。

- 当j为n（nums2的长度）时，表示nums2的所有元素都在左侧，此时右侧的最小值应该是nums1[i]。
