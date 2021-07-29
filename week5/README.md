# 第五周作业

## 第一题 1011. 在 D 天内送达包裹的能力

CODE
```
class Solution {
public:
    int shipWithinDays(vector<int>& weights, int days) {
        //二分夹逼 每天运算x的days天运玩。 当运算天数小于规定天数的时候，那么x往左边找，当天数大于规定天时，往右边找。
        int left = *max_element(weights.begin(),weights.end());
        int right = accumulate(weights.begin(),weights.end(),0);
        while(left < right) {
            int mid = (left + right) >> 1;
            int need = 1, curr = 0;
            for(int weight: weights) {
                if (curr + weight > mid) {
                    need++;
                    curr = 0;
                }
                curr += weight;
            }
            if (need <= days) { //当天数小于规定天的时候 x往左边走
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```

## 第二道题  875  爱吃香蕉的珂珂
```
珂珂喜欢吃香蕉。这里有 N 堆香蕉，第 i 堆中有 piles[i] 根香蕉。警卫已经离开了，将在 H 小时后回来。

珂珂可以决定她吃香蕉的速度 K （单位：根/小时）。每个小时，她将会选择一堆香蕉，从中吃掉 K 根。如果这堆香蕉少于 K 根，她将吃掉这堆的所有香蕉，然后这一小时内不会再吃更多的香蕉。  

珂珂喜欢慢慢吃，但仍然想在警卫回来前吃掉所有的香蕉。

返回她可以在 H 小时内吃掉所有香蕉的最小速度 K（K 为整数）。

```
CODE
```
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        int left = 1;
        int right = *max_element(piles.begin(),piles.end()); //最大值为数组里面的最大吃的速度
        while(left < right) {
            int mid = (right + left) >> 1;
            int need = 0;
            for(int h: piles) {
                need +=  (h - 1)/mid + 1;
            }
            if (need <= h) { //当时间小于的时候，往左边走
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};
``` 
