### 树
树的最基本性质
1.树的结点数 $n$ = 所有节点的度数之和 + 1
2.度为 $m$ 的树中，第 $i$ 层上最多有 $m^{i-1}$ 个结点
3.高度为 $h$ 的 $m$ 叉树至多有 $(m^h-1)/(m-1)$ 个结点
4.度为 $m$ 、具有 $n$ 个结点的树的最小高度为 $\lceil \log_2 (n(m-1) +1) \rceil$
5.度为 $m$ 、具有 $n$ 个结点的树的最大高度为 $n-m+1$

---
### 二叉树

二叉树的类型：
1.满二叉树 2.完全二叉树 3.二叉排序树 4.平衡二叉树 5.正则二叉树（度只有0和2）

二叉树的基本性质
1.非空二叉树的叶结点数 = 度为 2 的结点数 + 1，即 $n_0 = n_2 +1$
2.非空二叉树的第 $k$ 层最多有 $2^{k-1}$ 个结点 （$k>=1$）
3.高度为 %h% 的二叉树至多有 $2^h-1$ 个结点
4.具有 $n$ 个结点的完全二叉树的高度为 $\lceil log_2(n+1) \rceil or \lfloor log_2n \rfloor +1$
5.在完全二叉树中：若 $i<= \lfloor n/2 \rfloor $ ，则 $i$ 为分支节点，否则为叶节点；叶节点只能在最后两层出现；最多只有一个度为1的结点，即最后一个分支结点 $\lfloor n/2 \rfloor$；若结点 $i$ 为叶节点或只有左孩子，后面的结点均为叶节点；若 $n$ 为奇数，所有分支结点都有左右孩子，若  $n$ 为偶数，编号最大的分支结点 $\lfloor n/2 \rfloor$ 只有左孩子，其他都有左右孩子；当 $i>1$ 时，结点 $i$ 的双亲结点为 $\lfloor i/2 \rfloor$；若结点 $i$ 的左孩子为 $2i$ ，右孩子为 $2i + 1$；结点 $i$ 所在的深度为 $\lfloor log_2i \rfloor 1$

二叉树的链式存储结构
```cpp
typedef struct BiTNode{
    ElemType data;
    struct BiTNode *lchild, *rchild;
}BiTNode, *BiTree;
```
创建二叉树
```cpp
void CreateBiTree (BiTree &T){
    char Ch; cin>>Ch;
    if(ch == '#'){
        T = NULL;
    }
    else{
        T = (BiNode*) malloc (sizeof(BiTree));
        T->data = ch;
        CreateBiTree(T->lchild);
        CreateBiTree(T->rchild);
    }
}
```

先序遍历
```cpp
void PreOrder(BiTree T){
    if(T!=NULL){
        cout<<T->data<<" ";
        PreOrder(T->lchild);
        PreOrder(T->rchild);
    }
}
```

中序遍历
```cpp
void InOrder(BiTree T){
    if(T!=NULL){
        InOrder(T->lchild);
        cout<<T->data<<" ";
        InOrder(T->rchild);
    }
}
```

后序遍历
```cpp
void PostOrder(BiTree T){
    if(T!=NULL){
        PostOrder(T->lchild);
        PostOrder(T->rchild);
        cout<<T->data<<" ";
    }
}
```

非递归中序遍历（考研高频，常用栈模拟递归）
```cpp
void InOrder(BiTree T){
    stack<BiTNode*> S;
    BiTree *p = T;
    while(p!=NULL && S.empty()){
        if(P!=NULL){
            S.push(p);
            p = p->lchild;
        }
        else{
            p = S.top();
            S.pop();
            cout<<p->data<<" ";
            p = p->rchild;
        }
    }
}
```
层次遍历（利用队列，逐层输出）
```cpp
void LevelOrder(BiTree T){
    if(T==NULL) return;
    queue<BiTree*> Q;
    Q.push(T);
    while(!Q.empty()){
        BiTNode *p = Q.front();
        Q.pop();
        cout<< p->data<<" ";
        if(p->lchild != NULL) Q.push(p->lchild);
        if(p->rchild != NULL) Q.push(p->rchild);
    }
}
```

求树的深度
```cpp
int Depth(BiTree T){
    if(T==NULL) return 0;
    int leftDepth = Depth(T->lchild);
    int rightDepth = Depth(T->rchild);
    return (leftDepth > rightDepth ? leftDepth : rightDepth) + 1;
}
```

求结点总数
```cpp
int NodeCount(BiTree T){
    if(T==NULL) return 0;
    return NodeCount(T->lchild) + NodeCount(T->rchild) + 1;
}
```

求叶节点个数
```cpp
int leafCount(BiTree T){
    if(T == NULL) return 0;
    if(T->lchild==NULL && T->rchild==NULL){
        return 1;
    }
    return leafCount(T->lchild) + leafCount(T->rchild);
}
```

求第 $k$ 层结点个数
```cpp
int levelNodeCount(BiTree T; int k){
    if(T==NULL || k<1) return 0;
    if(k==1) return 1;
    return levelNodeCount(T->lchild,k-1) + levelNodeCount(T->rchild,k-1);
}
```

在树中查找值为x的结点(先序查找)
```cpp
BiTNode* FindNode(BiTree T,ElemType x){
    if(T == NULL) return NULL;
    if(T->data == x) return;
    BiTNode *res = NULL;
    res = FindNode(T->lchild,x);
    if(res!=NULL) return res;
    return FindNode(T->rchild,x);
}
```

获取某个结点的双亲（父结点）
```cpp
BiNode* GetParent(BiTree T,BiTree *p){
    if(T==NULL || T==p || p==NULL) return NULL;
    if(T->lchild==p || T->rchild==p) return T;
    BiTNode *res = NULL;
    res = GetParent(T->lchild,p);
    if (res!=NULL) return res;
    return GetParent(T->lchild,p);
}
```

判断二叉树是否为完全二叉树（利用层次遍历思想）
```cpp
bool isCompleteBiTree(BiTree T){
    if(T==NULL) return true;
    queue<BiTNode*> Q;
    Q.push(T);
    bool mustHaveNoChild = false;
    while(!Q.empty()){
        BiTNode *p = Q.front;
        Q.pop();
        if(p==NULL){
            mustHavaNoChild = true;  //遇到空结点，后续必须全为空
        }
        else{
            if(mustHaveNoChild){
                return false;
            }
            Q.push(p->lchild);
            Q.push(p->rchild);
        }
    }
    return true;
}
```
