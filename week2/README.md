


### 本周作业
以下题目选 2 道提交即可
```
	•	加一（Easy）
	•	合并两个有序链表（Easy）
	•	设计循环双端队列（Medium）
	•	和为 K 的子数组（Medium）
```

#### 第一题  加一（Easy）

code 
```
if (digits.size() == 0) return digits;
        int len = digits.size() - 1; 
        while(len >= 0) { 
            if ((digits[len] + 1) % 10 != 0) {
                digits[len]++;
                return digits;
            } else {
                digits[len] = 0;
            }
            len --;
        }
        vector<int> a = vector(digits.size()+1,0);
        a[0] = 1;
        return a;
```
题解使用指针，最后一个下标
```
if (digits.size() == 0) return digits;
        int len = digits.size() - 1;  //最后一个下标
        while(len >= 0) { //从数组末端往前走
            if ((digits[len] + 1) % 10 != 0) { //判断最后个值是否需要进位，如果不进，那么当前位++，然后返回。
                digits[len]++;
                return digits;
            } else { //否则，当前位的值为0，
                digits[len] = 0;
            }
            len --; //进入下一个位置
        }
        // 如果前面的所有位都是0，说明，需要再扩充进1位，赋值低位为1.
        vector<int> a = vector(digits.size()+1,0);
        a[0] = 1;
        return a;

```

#### 第二题 合并两个有序链表
code 
```
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
      if (l1 == nullptr) return l2; //判断当ll1 == null 的时候返回l2
      if (l2 == nullptr) return l1; // 判断当前l2== null的时候返回l1
      ListNode* head = new ListNode(-1); //定义一个新的空头链表
      ListNode* pre = head;
      while(l1 != nullptr && l2 != nullptr) { //当两个链表都有不能为空的时候，比较值，把值小的一个赋值到新的链表上，往后移动
          if (l1->val < l2->val) {
              pre->next = l1;
              l1 = l1->next;
          } else {
              pre->next = l2;
              l2 = l2->next;
          }
          pre = pre->next;
      }
      //最后当最后的谁还有值，就把他赋值给下一个节点上
      pre->next = l1 == nullptr ? l2 : l1;
      //返回空头节点的下一个节点的连接
      return head->next;
    }
};
```


#### 第四题 和为 K 的子数组（Medium）

<!"https://leetcode-cn.com/problems/subarray-sum-equals-k/submissions/">
code 
```

```



