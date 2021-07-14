作业
```
	•	从中序与后序遍历序列构造二叉树（Medium）
	•	课程表 II （Medium）
	•	被围绕的区域（Medium）
```
### 第一题
```
从中序与后序遍历序列构造二叉树（Medium）
```
code
```

class Solution {
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if (inorder.size() == 0 || postorder.size() == 0) return nullptr;
        return dfs(inorder,postorder,0,inorder.size() - 1, 0 ,postorder.size() - 1);
    }

private: 
  TreeNode* dfs(vector<int>& inorder,vector<int>& postorder,int l1,int r1,int l2,int r2) {
      if (l1 > r1) return NULL;
      TreeNode* root = new TreeNode(postorder[r2]);
      int width = l1; //寻找头的节点在中序偏离的下标
      while (width <= r1 && inorder[width] != postorder[r2]) width++;
      int leftSize = width - l1; //左子树的个数
      root->left = dfs(inorder,postorder,l1,width - 1,l2,l2 + leftSize - 1); // 左边子树在 中序遍历的下标是 l1 -> width - 1, 在后序遍历的下标是 l2 - > l2 + leftsize - 1。
      root->right = dfs(inorder,postorder,width + 1 ,r1,l2 + leftSize ,r2 - 1); //右子树在中序的遍历的下标是 l2 ->  width + 1 -> r1 , 在后序遍历的下标是 l2 + leftSize , r2 -1 最后去掉最后一个
      return root;
  }


};

```

### 第二题
```
 课程表 II （Medium）
```
CODE
```
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        this->n  = numCourses;
        indu = vector<int>(n,0);
        ediges = vector<vector<int>>(n,vector<int>());
        for (vector<int> &b: prerequisites) {
            int r1 = b[0];
            int r2 = b[1]; // [0,1] r2 -> r1
            addEdite(r2,r1);
        }
        bfs();
        return ans;
    }

private:
    int n;
    vector<vector<int>> ediges;
    vector<int> indu;
    vector<int> ans;
    void addEdite(int x,int y) {
        ediges[x].push_back(y);
        indu[y]++;
    }
    void bfs() {
        queue<int> q;
        for (int i = 0; i < n; i++) {
            if (indu[i] == 0){
                q.push(i);
            }
        }
        while(!q.empty()) {
            int x = q.front();
            q.pop();
            ans.push_back(x);
            for (auto& b: ediges[x]) {
                indu[b]--;
                if (indu[b] == 0) {
                    q.push(b);
                }
            }
        }
    }
};
```

### 第三题
```
被围绕的区域（Medium）
```


