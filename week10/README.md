# 699. 掉落的方块
```
在无限长的数轴（即 x 轴）上，我们根据给定的顺序放置对应的正方形方块。

第 i 个掉落的方块（positions[i] = (left, side_length)）是正方形，其中 left 表示该方块最左边的点位置(positions[i][0])，side_length 表示该方块的边长(positions[i][1])。

每个方块的底部边缘平行于数轴（即 x 轴），并且从一个比目前所有的落地方块更高的高度掉落而下。在上一个方块结束掉落，并保持静止后，才开始掉落新方块。

方块的底边具有非常大的粘性，并将保持固定在它们所接触的任何长度表面上（无论是数轴还是其他方块）。邻接掉落的边不会过早地粘合在一起，因为只有底边才具有粘性。

 

返回一个堆叠高度列表 ans 。每一个堆叠高度 ans[i] 表示在通过 positions[0], positions[1], ..., positions[i] 表示的方块掉落结束后，目前所有已经落稳的方块堆叠的最高高度。

```

CODE
```
vector<int> fallingSquares(vector<vector<int>>& positions) {
         int n = positions.size();
        vector<int> height(n, 0);

        for (int i = 0; i < n; ++i)
        {
            // 记录当前的窗口，并更新高度
            int l = positions[i][0];
            int h = positions[i][1];
            int r = l + h;
            height[i] += h;

            // 遍历它后面的方块，如果有交叉，可能更新他们的高度
            for (int j = i + 1; j < n; ++j)
            {
                int l2 = positions[j][0];
                int r2 = positions[j][1] + l2;
                // 有交集，取最大高度去更新
                if (l2 < r && r2 > l)
                {
                    height[j] = max(height[j], height[i]);
                    // cout << "update  " << j << " at " << height[j] << endl;
                }   
            }
        }

        for (int i = 1; i < n; ++i)
        {
            // cout << height[i] << endl;
            height[i] = max(height[i-1], height[i]);
        }

        return height;

    }
};
```
