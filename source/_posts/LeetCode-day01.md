---
title: LeetCode(day01)
date: 2021-06-28 11:54:36
---
## 1. 两数之和

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

**提示：**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **只会存在一个有效答案**

**思路**

使用冒泡法逐一与前一个相加，如果两数相加之和等于`target`则返回，反之继续

**个人解答：**

``` javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
   for (let i = 0; i < nums.length; i ++) {
       for (let k = i + 1; k <= nums.length; k ++) {
           if (nums[i] + nums[k] == target) {
               return [i, k]
           }
       }
   }
};
```



## 2. 整数反转

给你一个 32 位的有符号整数 `x` ，返回将 `x` 中的数字部分反转后的结果。

如果反转后整数超过 32 位的有符号整数的范围 `[−231, 231 − 1]` ，就返回 0。

**假设环境不允许存储 64 位整数（有符号或无符号）。**

**示例 1：**

```
输入：x = 123
输出：321
```

**示例 2：**

```
输入：x = -123
输出：-321
```

**示例 3：**

```
输入：x = 120
输出：21
```

**示例 4：**

```
输入：x = 0
输出：0
```

**提示：**

- `-231 <= x <= 231 - 1`

**思路**

将参数转化为String类型，然后判断该参数是否大于0，如果大于0，则转化为数组再翻转转化为String类型，如果小于0，则现将负号剔除，进行前面操作

**个人解答**

``` javascript
/**
 * @param {number} x
 * @return {number}
 */
var reverse = function(x) {
    x = String(x);
    let target = [];
    if (Number(x) < 0) {
        target[0] = x.split("")[0];
        target.push(x.split("").splice(1).reverse().join(""));
        target = target.join('')
    } else {
        target = x.split("").reverse().join("");
    }
    if (Number(target) <= 2147483648 && Number(target) >= -2147483648) {
        return target;
    } else {
        return 0;
    }
};
```



## 3. 回文数

给你一个整数 `x` ，如果 `x` 是一个回文整数，返回 `true` ；否则，返回 `false` 。

回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。例如，`121` 是回文，而 `123` 不是。

**示例 1：**

```
输入：x = 121
输出：true
```

**示例 2：**

```
输入：x = -121
输出：false
解释：从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

**示例 3：**

```
输入：x = 10
输出：false
解释：从右向左读, 为 01 。因此它不是一个回文数。
```

**示例 4：**

```
输入：x = -101
输出：false
```

**提示：**

- `-231 <= x <= 231 - 1`

**思路**

如果是正数，则将数据化为数组、翻转，如果相等则`true`，反之`false`，如果为负数，则先将负号剔除，进行前面操作

**个人解答**

``` javascript
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function (x) {
    x1 = String(x);
    x2 = String(x).split('').reverse().join("")
    return x1 == x2
};
```



## 4. 最长公共前缀

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

**示例 1：**

```
输入：strs = ["flower","flow","flight"]
输出："fl"
```

**示例 2：**

```
输入：strs = ["dog","racecar","car"]
输出：""
解释：输入不存在公共前缀。
```

**提示：**

- `0 <= strs.length <= 200`
- `0 <= strs[i].length <= 200`
- `strs[i]` 仅由小写英文字母组成

**思路**

使用冒泡法，将除数组第一个元素与数组第一各元素的各个子元素进行对比，如果相等则将子元素记录下来，反之则返回记录的数据

**个人解答**

``` javascript
/**
 * @param {string[]} strs
 * @return {string}
 */
var longestCommonPrefix = function (strs) {
    var re = ''
    if (!strs.length || strs.includes('')) return re;
    if (strs.length == 1) return strs[0]
    for (let i = 0; i < strs[0].length; i++) {
        for (let j = 0; j < strs.length; j++) {
            if (strs[j][i] != strs[0][i]) return re;
        }
        re += strs[0][i]
    }
    return re
};
```



## 5. 有效括号

给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。

 **示例 1：**

```
输入：s = "()"
输出：true
```

**示例 2：**

```
输入：s = "()[]{}"
输出：true
```

**示例 3：**

```
输入：s = "(]"
输出：false
```

**示例 4：**

```
输入：s = "([)]"
输出：false
```

**示例 5：**

```
输入：s = "{[]}"
输出：true
```

**提示：**

- `1 <= s.length <= 104`
- `s` 仅由括号 `'()[]{}'` 组成

**思路**

首先建立一个Map对象，将括号放入其中，如果Map对象中不含有该string对象中的括号，则将该元素放入栈`stk`中，反之则判断Map类型中对应`value`是否为栈中最后一个元素，如果是，则从栈`stk`中取出，如果不是则`return false`，最后如果栈`stk`的长度大于0，则代表括号没有一一对应，返回`false`，反之则返回`true`

**个人解答**

``` javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
  if (s.length == 1) {
    return false;
  }

  let mapList = new Map([
    [')', '('],
    ['}', '{'],
    [']', '[']
  ])

  let stk = []

  for (let ch of s) {
    if (mapList.has(ch)) {
      if (!stk.length || stk[stk.length - 1] !== mapList.get(ch)) {
        return false;
      }
      stk.pop()
    } else {
      stk.push(ch)
    }
  }

  return !stk.length
};
```

