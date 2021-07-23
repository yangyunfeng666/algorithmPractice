# 寻找旋转排序数组中的最小值 II
```
已知一个长度为 n 的数组，预先按照升序排列，经由 1 到 n 次 旋转 后，得到输入数组。例如，原数组 nums = [0,1,4,4,5,6,7] 在变化后可能得到：
若旋转 4 次，则可以得到 [4,5,6,7,0,1,4]
若旋转 7 次，则可以得到 [0,1,4,4,5,6,7]
注意，数组 [a[0], a[1], a[2], ..., a[n-1]] 旋转一次 的结果为数组 [a[n-1], a[0], a[1], a[2], ..., a[n-2]] 。

给你一个可能存在 重复 元素值的数组 nums ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 最小元素 。

```
CODE
```
class Solution {
public:
    int findMin(vector<int>& nums) {
        //寻找第一个小于最后一个值的下标
        int l = 0, r = nums.size() - 1;
        while(l < r) {
            int mid = l + (r - l) >> 1;
            if (nums[mid] < nums[r]) { // 当中值小于mid的时候，把最右边的值赋值为mid
                r = mid;
            } else if (nums[mid] > nums[r]) { //否则是左边
                l = mid - 1;    
            } else { //相等的时候 --
                r--;
            }
        }
        return nums[r];
    }
};
```

# 合并K个升序链表
```
给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

```

CODE
```
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```
CODE
```

class BinerSerarch {
    private:
       vector<Node> data;
    public:
       bool empty() {
           return data.size() == 0;
       }
        void push(const Node& node) {
            data.push_back(node);
            int p = data.size() - 1;
            while (p > 0) {
                int fa =  (p - 1) >> 1;
                if (data[fa].key > data[p].key) {
                    swap(data[fa],data[p]);
                    p = fa;
                } else {
                    break;
                }
            }
        }

        Node pop() {
            Node node = data[0];
            data[0] = data[data.size() - 1];
            data.pop_back();
            int p = 0;
            int child = 2 * p + 1;
            while (child < data.size()) {
                int rightChild = 2 * p + 2;
                if (data[child].key > data[rightChild].key) {
                        child = rightChild;
                }
                if (data[p].key > data[child].key) {
                    swap(data[p],data[child]);
                    p = child;
                    child = 2 * p + 1;
                } else {
                    break;
                }
            }
            return node;
        }
};


class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        BinerSerarch q;
        for (ListNode* & b: lists) {
            if(b != nullptr)
            q.push(Node(b->val,b));
        }
        ListNode head;
        ListNode* tail = &head;
        while(!q.empty()) {
            Node node = q.pop();
            tail->next = node.listNode;
            tail = tail->next;
            ListNode* next = node.listNode->next;
            if (next != nullptr) {
                q.push(Node(next->val,next));
            }
        }
        return head.next;
    }	
};

```
# 滑动窗口最大值
```
给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

```
CODE
```
class Solution {
public:
     vector<int> maxSlidingWindow(vector<int>& nums, int k) {
         vector<int> ans;
         deque<int> q;
         for (int i = 0; i < nums.size(); i++) {
             while(!q.empty() && q.front() <= i - k) q.pop_front();
             while(!q.empty() && nums[q.back()] < nums[i]) q.pop_back();
             q.push_back(i);
             if (i > k - 1) {
                ans.push_back(nums[q.front()]);
             }
         }
         return ans;
     }
};
```

# 二叉搜索树中的插入操作
```
给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 输入数据 保证 ，新值和原始二叉搜索树中的任意节点值都不同。

注意，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回 任意有效的结果 。
```

CODE
```
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (root == NULL) {
            TreeNode* root = new TreeNode(val);
            return root;
        }
        if (root->val < val) {
            root->right = insertIntoBST(root->right,val);
        } else {
            root->left = insertIntoBST(root->left,val);
        }
        return root;
    }
};
```
# 在排序数组中查找元素的第一个和最后一个位置
```
给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回 [-1, -1]。

进阶：

你可以设计并实现时间复杂度为 O(log n) 的算法解决此问题吗？
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]

输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]

```
CODE
```
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> ans;
        // x >= target
        int left = 0, right = nums.size();
        while (left < right) {
            int mid = (left + right) >> 1;
            if (nums[mid] >= target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        ans.push_back(right);
        // x <= target
        left = -1, right = nums.size() - 1;
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            if (nums[mid] <= target) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        ans.push_back(right);
        if (ans[0]> ans[1]) return {-1,-1};
        return ans;
    }
};
```

