#### 无重叠区间
![eraseOverlapIntervals](/images/Arithmetic/eraseOverlapIntervals.PNG)


* 解法


```java
class Solution {

    private int intervalSchedule(int[][] intvs) {
        if (intvs.length == 0) {
            return 0;
        }
        // 按 end 升序排序
        Arrays.sort(intvs, Comparator.comparingInt(a -> a[1]));
        // 至少有一个区间不相交
        int count = 1;
        // 排序后，第一个区间就是 x
        int xEnd = intvs[0][1];
        for (int[] interval : intvs) {
            int start = interval[0];
            if (start >= xEnd) {
                // 找到下一个选择的区间了
                count++;
                xEnd = interval[1];
            }
        }
        return count;
    }

    public int eraseOverlapIntervals(int[][] intervals) {
        return intervals.length - intervalSchedule(intervals);
    }
}
```


#### 用最少数量的箭引爆气球
![findMinArrowShots](/images/Arithmetic/findMinArrowShots.PNG)


* 解法


```java
class Solution {
    private int intervalSchedule(int[][] intvs) {
        if (intvs.length == 0) {
            return 0;
        }
        // 按 end 升序排序
        Arrays.sort(intvs, Comparator.comparingInt(a -> a[1]));
        // 至少有一个区间不相交
        int count = 1;
        // 排序后，第一个区间就是 x
        int xEnd = intvs[0][1];
        for (int[] interval : intvs) {
            int start = interval[0];
            if (start > xEnd) {
                // 找到下一个选择的区间了
                count++;
                xEnd = interval[1];
            }
        }
        return count;
    }

    public int findMinArrowShots(int[][] points) {
        return intervalSchedule(points);
    }
}
```


#### 跳跃游戏
![canJump](/images/Arithmetic/canJump.PNG)


* 解法


```java
class Solution {
    public boolean canJump(int[] nums) {
        int n = nums.length;
        int farthest = 0;
        for (int i = 0; i < n - 1; i++) {
            farthest = Math.max(farthest, i + nums[i]);
            if (farthest <= i) {
                return false;
            }
        }
        return farthest >= n - 1;
    }
}
```



