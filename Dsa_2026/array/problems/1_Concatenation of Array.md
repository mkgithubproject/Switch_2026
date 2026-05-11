# Problem Statement

You are given an integer array `nums` containing `n` elements.

Create a new array `ans` of size `2n` such that:

* The first `n` elements of `ans` are the same as `nums`
* The next `n` elements are again the same as `nums`

In simple words, duplicate the array by appending the original array to itself.

Return the newly created array.

---

# Example 1

Input:

```js id="9q8n2d"
nums = [1,2,1]
```

Process:

```js id="j7u4mk"
First copy  -> [1,2,1]
Second copy -> [1,2,1]
```

Output:

```js id="d4m1zs"
[1,2,1,1,2,1]
```

---

# Example 2

Input:

```js id="v6a2op"
nums = [1,3,2,1]
```

Output:

```js id="k8t5rb"
[1,3,2,1,1,3,2,1]
```

---

# What the Problem Is Asking

If:

```js id="n1f3xc"
nums = [a,b,c]
```

Then return:

```js id="t5g9yu"
[a,b,c,a,b,c]
```

So the final array is:

```js id="w3e8ql"
nums + nums
```

(concatenation of the array with itself)

```
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var getConcatenation = function(nums) {
     let arr = new Array(2*nums.length);
     let index = 0;
     let numIndex =0;
     for(let i =0; i< arr.length;i++){
        if(numIndex == nums.length){
            numIndex =0;
        }
        arr[index++] = nums[numIndex++];
     }
     return arr;
};
```
