#### 二分法模板
```
int start = 0, end = nums.length - 1;
while (start + 1 < end) {
    int mid = start + (end - start) / 2;
    if (...) {
        start = mid;
    } else {
        end = mid;
    }
}
```


#### 动态规划 
| 执行步骤 | 解释 | 
| :----- | :----- | 
| 确定状态 | 找子问题及完成问题的最后一步 | 
| 写出状态转移方程 | ———— | 
| 边界条件 | 也就是找到无法通过转移方程得到的结果 | 
| 确定计算方向 | 这个看状态方程与边界条件 | 
