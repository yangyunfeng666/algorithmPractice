第四周题目
```
本周作业
以下题目选 2 道提交即可
	•	LRU 缓存机制（Medium）
	•	子域名访问计数（Easy）
	•	数组的度（Easy）
	•	元素和为目标值的子矩阵数量（Hard）
	•	合并K 个升序链表（Hard） (要求：用分治实现，不要用堆)

```

#### 第一题LRU 缓存机制（Medium）
```
class LRUCache {
public:

    LRUCache(int capacity) {
        this->capacity = capacity;
        tail = new Node();
        head = new Node();
        head->next = tail;
        tail->pre = head;
    }
    
    int get(int key) {
        if (map.find(key) == map.end()) return -1; //如果不存在，直接返回
        int value = map[key]->val; 
        deleteFromList(map[key]); //删除队列数据
        insertIntoHead(key,value); //插入到对头
        return value;
    }
    
    void put(int key, int value) { 
        if (map.find(key) != map.end()) { //当存在的时候，
            deleteFromList(map[key]); //在链表中删除
            insertIntoHead(key,value); //插入到队头
        } else {
            insertIntoHead(key,value); //直接插入到对头
        }
        if (map.size() > capacity) { //大于容量的时候，去除tail的前一个值
            map.erase(tail->pre->key);
            deleteFromList(tail->pre);
        }
    }

    

private:
    struct Node { //定义数据结构
        int val;
        int key;
        Node* pre;
        Node* next;
    };

    unordered_map<int,Node*> map; //定义map，来快速查找节点
    int capacity;
    Node* head; // 保护节点头
    Node* tail; // 保护节点尾

    void deleteFromList(Node* node) {
        node->pre->next = node->next;
        node->next->pre = node->pre;
        delete node;
    };

    void insertIntoHead(int key,int val) {
        Node* node = new Node();
        node->val = val;
        node->key = key;
			// 处理node和Head后节点的关系
        node->next = head->next;
        head->next->pre = node;
			//处理head和node关系
        node->pre = head;
        head->next = node;
        map[key] = node;
    };
};

```

#### 合并K 个升序链表（Hard） (要求：用分治实现，不要用堆)

Code
```

class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
      if (lists.size() == 0) return  NULL;
      if (lists.size() == 1) return  lists[0];
      if (lists.size() == 2) return  merge(lists[0],lists[1]);
       int mid = lists.size() >> 1;
        vector<ListNode*> l1;
        for (int i = 0; i < mid; i++) {
            l1.push_back(lists[i]);
        }
         vector<ListNode*> l2;
        for (int i = mid; i < lists.size(); i++) {
            l2.push_back(lists[i]);
        }
       return merge(mergeKLists(l1),mergeKLists(l2));
    }


    ListNode* merge(ListNode* l1,ListNode* l2) {
        if(l1 == NULL) return l2;
        if (l2 == NULL) return l1; 
        ListNode* list =  new ListNode(0); 
        ListNode* pre = list;
        while (l2 != NULL && l1 != NULL) {
            if (l2->val > l1->val) {
                pre->next = l1;
                l1 = l1->next;
            } else {
                pre->next = l2;
                l2 = l2->next;
            }
            pre = pre->next;
        }
        pre->next = l1 == NULL ? l2 : l1;
        return list->next;
    }

};
```

