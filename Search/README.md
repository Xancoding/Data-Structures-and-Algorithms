## 基本概念
- 静态查找表：只需进行数据元素的查询操作
    - 顺序查找 `无序`
    - 二分查找 `有序`
    - 分块查找 `块间有序 块内无序`
- 动态查找表：在查找过程中需要对查找表进行修改
    - **二叉搜索树**
        - 左子树所有结点的值均小于根结点的值
        - 右子树所有结点的值均大于根结点的值
        - 对其中序遍历，可得一个按关键字值有序的序列
    - 二叉平衡树(**AVL树**)
        - 满足二叉搜索树性质
        - 左子树和右子树高度之差的绝对值不超过1
    - B-树和B+树
- **哈希表**
- 关键字(Key)：数据元素中某个数据项，用以标识特定的数据元素
    - 主关键字(Primary Key)：唯一标识某个数据元素的关键字
    - 次关键字(Secondary Key)：能标识多个数据元素的关键字
- **ASL**：平均查找长度
- **最小不平衡子树**：在根到插入结点的查找路径中，以距离插入结点最近的、且平衡因子的绝对值大于1的结点为根，沿此查找路径再找两个结点构成这棵最小不平衡子树
- 结点的**平衡因子**：左子树的深度 - 右子树的深度

## 哈希表
[模拟散列表](https://www.acwing.com/problem/content/description/842/ "模拟散列表")
### 开放寻址法
#### 代码实现
```cpp
#include<iostream>
#include<cstring>

using namespace std;

const int N = 200003, INF = 0x3f3f3f3f;

int h[N];

int find(int x) {  // 如果x在哈希表中，返回x的下标；如果x不在哈希表中，返回x应该插入的位置
    int k = (x % N + N) % N;
    while (h[k] != INF && h[k] != x) {
        k++;
        if (k == N) k = 0;
    }
    
    return k;
}

int main() {
    int m;
    cin >> m;
    
    memset (h, 0x3f, sizeof h);
    
    while (m--) {
        string op;
        int x;
        cin >> op >> x;
        if (op[0] == 'I') {
            h[find(x)] = x; 
        } else {
            if (h[find(x)] == INF) cout << "No" << endl;
            else cout << "Yes" << endl;
        }
    }
    
    return 0;
}
```
#### 线性探测法
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/19.jpg" /> </div>
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/12.png" /> </div>

#### 平方探测法
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/20.jpg" /> </div>
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/13.png" /> </div>

### 拉链法
#### 代码实现
```cpp
#include<iostream>
#include<cstring>

using namespace std;

const int N = 100003;

int h[N], ne[N], e[N], idx;

void insert(int x) {  // 向哈希表中插入一个数
    int k = (x % N + N) % N;
    e[idx] = x;
    ne[idx] = h[k];
    h[k] = idx++;
}

int find(int x) {  // 在哈希表中查询某个数是否存在
    int k = (x % N + N) % N;
    for (int i = h[k]; i != -1; i = ne[i]) {
        if (e[i] == x) return true;
    }
    
    return false;
}

int main() {
    int m;
    cin >> m;
    
    memset (h, -1, sizeof h);
    
    while(m--) {
        string op;
        int x;
        cin >> op >> x;
        
        if (op[0] == 'I') {
            insert(x);
        } else {
            if (find(x)) cout << "Yes" << endl;
            else cout << "No" << endl;
        }
    }
    
    return 0;
}
```

## 二叉平衡树
### 插入
- **最小不平衡子树**的根结点为 A ，则调整该子树的规律可归纳为下列**四种情况**：
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/1.png" /> </div>
<hr />
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/2.png" /> </div>
<hr />
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/3.png" /> </div>
<hr />
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/4.png" /> </div>
<hr />

#### 例
<center>
<figure>
<img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/5.png" />
<img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/6.png" />
</figure>
</center>

### 删除
- 查找待删除结点
- 删除结点(同**二叉搜索树**)
- 平衡化处理
    - 删除的结点在结点A的左子树上，最小不平衡子树是RR或RL
    - 删除的结点在结点A的右子树上，最小不平衡子树是LL或LR
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/7.png" /> </div>

#### 例
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/8.png" /> </div>
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/9.png" /> </div>
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/10.png" /> </div>
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/11.png" /> </div>


## 二叉搜索树
### 初始化
```cpp
/* 创建一棵二叉树  */
void tree_create(treelink &T){
   while(1) {
       char data[MAX];
       scanf("%s", data);
       if (strcmp(data, "#") == 0)
           break;
       else
           node_insert(T, data);
   }
}
```
### 查找
- `指针p`指向当前访问结点的双亲，初始值为NULL
- 若**查找成功**，则`指针s`指向该数据元素的结点，函数值返回`true`
- 若**查找失败**，则`指针s`指向查找路径上访问的最后一个结点，函数值返回`false`
```cpp
/* 查找数据 */
bool node_find(treelink T, char  *data,treelink &p, treelink &s) {  //p为父结点 s为子结点
    if(T == NULL) {
        s = p;
        return false;
    } else if (strcmp(T->data, data) == 0) {
        s = T;
        return true;
    } else if (strcmp(T->data, data) > 0) {
        p = T;
        return node_find(T->left, data, p, s);
    } else {
        p = T;
        return node_find(T->right, data, p, s);
    }
}
```
### 插入
```cpp
/* 插入数据 */
void node_insert(treelink &T, char *data) {
    if (T == NULL) {
        T = new treenode;
        strcpy(T->data, data);
        T->left = T->right = NULL;
    } else if (strcmp(T->data, data) > 0) {
        node_insert(T->left, data);
    } else {
        node_insert(T->right, data);
    }
}
```
### 删除
- 若`p结点`既有**左子树**又有**右子树**
    1. 将`待删除结点p`的**右子树**链接到它的`中序前趋结点s`(即左子树下最右下结点)的右孩子指针域上
    2. 将`结点s`的**左子树**链接到其`双亲结点`的左(或右)孩子域上
```cpp
/* 删除数据 */
bool node_delete(treelink &T, char *data) {
    if (T == NULL) return false;
    else {
        if (strcmp(T->data, data) == 0) {
            Delete(T);
            return true;
        }
        else if (strcmp(T->data, data) > 0)
            return node_delete(T->left, data);
        else
            return node_delete(T->right, data);
    }
}

void Delete(treelink &p) {
    if (!p->right) {  //没有右子树
        treelink tmp = p;
        p = p->left;
        delete tmp;
    } else if (!p->left) {  //没有左子树
        treelink tmp = p;
        p = p->right;
        delete tmp;
    } else {  //左右子树都存在
        treelink q = p, s = q->left;  //s指向被删结点的中序前驱，q指向s的双亲
        while(s->right) {
            q = s;
            s = s->right;
        }
        strcpy(p->data, s->data);
        if (q != p)
            q->right = s->left;
        else
            q->left = s->left;
        delete s;
    }
}
```
### 测试数据
```cpp
45 24 53 12 14 90 #
```
### 输出
```cpp
-----------------------+45(D)
---------------------+24(L)
-------------------+12(L)
-----------------+14(R)
---------------------+53(R)
-------------------+90(R)
```

## ASL
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/14.png" /> </div>
<hr />

### 顺序查找
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/15.png" /> </div>
<hr />

### 折半查找
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/16.png" /> </div>
<hr />

### 分块查找
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/17.png" /> </div>
<hr />

### 哈希表
<div align="center"> <img src="https://git.acwing.com/ZagY/learn-data-structures/-/raw/main/Search/images/18.png" /> </div>
<hr />
