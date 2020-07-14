#### 二分法
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