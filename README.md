# golang_non_decreasing_array

Given an array `nums` with `n` integers, your task is to check if it could become non-decreasing by modifying **at most one element**.

We define an array is non-decreasing if `nums[i] <= nums[i + 1]` holds for every `i` (**0-based**) such that (`0 <= i <= n - 2`).

## Examples

**Example 1:**

```
Input: nums = [4,2,3]
Output: true
Explanation: You could modify the first4 to1 to get a non-decreasing array.

```

**Example 2:**

```
Input: nums = [4,2,1]
Output: false
Explanation: You can't get a non-decreasing array by modify at most one element.

```

**Constraints:**

- `n == nums.length`
- `1 <= n <= 104`
- `105 <= nums[i] <= 105`

## 解析

給定一個正整數陣列 nums, 

定義陣列 Array 如果是非遞減陣列代表每個 num[i] < nums[i+1] for i = 0 ~ len(nums) -2 

寫一個演算法來判斷給定的陣列 nums,  在最多只能修改陣列的一個值限制之下，能不能夠變成非遞減陣列

初步思考

透過一個 count 來紀錄 nums[i] > nums[i+1] 的情況

然後判斷這個 count 是否 ≥ 1 

但是這樣會有一個問題 只考慮到 連續的兩個數

但如果 nums[i-2] > nums[i], nums[i-2] < nums[i-1], nums[i-1] > nums[i] 的情況下

nums[i-2] , nums[i-1], nums[i], nums[i+1]

比如說是 3, 4, 2, 3

這樣技術 會發現當 i= 2 時, 會發現 count = 1

但是 nums[0] > nums[2] 所以只考慮到了 nums[i-1] > nums[i] 的關係是不夠的

讓我們思考一下

當遇到 nums[i-1] > nums[i] 時, 該如何做修改

有兩種作法 因為要變成 non-decreasing 所以可以把 其中一個值改成另一個值

1. 把 nums[i-1] 改成 nums[i]   如果 nums[i-2] ≤ nums[i] , 這樣後面的值只要考慮跟 nums[i] 的關係 也可以是不需要修改 nums[i-1]
2. 把 nums[i] 改成 nums[i-1]   如果 nums[i-2] > nums[i], 因為會關聯到 nums[i-1] 所以必須要修改

 
![](https://i.imgur.com/2HII3k6.png)

## 程式碼
```go
package sol

func checkPossibility(nums []int) bool {
	nLen := len(nums)
	count := 0
	for pos := 1; pos < nLen; pos++ {
		if nums[pos-1] > nums[pos] {
			if pos >= 2 && nums[pos-2] > nums[pos] {
				nums[pos] = nums[pos-1]
			}
			count++
		}
		if count >= 2 {
			return false
		}
	}
	return true
}
```
## 困難點

1. 要思考到 nums[i-2] > nums[i] 的狀況

## Solve Point

- [x]  建立一個 count 來紀錄做更改的次數
- [x]  每次檢查 nums[i-1] > nums[i]  ,  如果是 count+= 1 當 i - 2 ≥ 0 時 更新 nums[i]= num[i-1]
- [x]  如果 count ≥ 2 則回傳 false, 當整個 loop 都檢查完 count ≤ 1 則回傳 true