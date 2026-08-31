### 线性表

**静态分布**的线性表

```cpp
#define MaxSize 50  
typedef struct{
//ElemType指数据类型，可以是string也可以是int等等
   ElemType data[MaxSize];  
   int length;
}SqList;
```

**动态分布**的线性表

```cpp
#define InitSize 100
typedef struct{
    ElemType *data;
    int MaxSize,length;
}SeqList;
```

C的初始动态分配语句为：
```c
//        转换指针类型        ElemTyp类型的大小 X 元素个数 
//                           ->需要malloc在堆上分配一块连续的内存大小
L.data = (ElemType*) malloc (sizeof(ElemType)*InitSize);
```

C++的动态分配语句为：
```cpp
L.data = new ElemType[InitSize];
```
---
### 顺序表
静态初始化
```cpp
//Sqlist l
void InitList(SqList &L){
    L.length=0;
}
```

动态初始化
```cpp
void InitList(SeqList &L){
    L.data = (ElemType*) malloc (InitSize*sizeof(ElemType));
    L.length = 0;
    L.MaxSize = InitSize;
}
```

插入操作
```cpp
bool ListInsert(SqList &L,int i,Eletype e){
    if(i<1 || i>L.length+1) return false;
    if(L.length >= MaxSize) return false;
    for(int j=L.length; j>=i; j--){
        L.data[j] = L.data[j-1];
    }
    L.data[i-1] = e;
    L.length++;
    return true;
}
```

删除操作
```cpp
bool ListDelete(SqList &L, int i, ElemType &e){
    if(i<1 || i>L.length) return false;
    e = L.data[i-1];
    for(int j=i; j<L.length; j++){
        L.data[j-1] = L.data[j];
    }
    L.length--;
    return true;
}
```

按值查找
```cpp
int LocateElem(SqList L, ElemType e){
    for(int i=0;i<L.length;i++){
        if(L.data[i]==e) return i+1;
    }
}
```

---
### 单链表

结点结构
```cpp
typedef struct LNode{
    ElemType data;
    struct LNode *next;
}LNode, *LinkList;
```

初始化
```cpp
//带头结点
bool InitList(LinkList &L){
    L = (LNode*)malloc(sizeof(LNode));
    L->next = NULL;
    return true;
}

//不带头结点
bool InitList(LinkList &L){
    L = NULL;
    return true;
}
```


求表长
```cpp
int Length(LinkList L){
    int len = 0;
    LNode *p = L;
    while(p->next != NULL){
        p = p->next;
        len++;
    }
    return len;
}
```

按序号查找结点
```cpp
LNode *GetElem(LinkList L, int i){
    LNode *p = L;
    int j = 0;
    while(p!=NULL && j<i){
        p = p->next;
        j++;
    }
    return p;
}
```


按值查找结点
```cpp
LNode *LocateElem(LinkList L, ElemType e){
    LNode *p = L->next;
    while(p!=NULL && p->data!=a){
        p=p->next;
    }
    return p;
}
```

插入操作
```cpp
bool ListInsert(LinkList &L, int i, ElemType e){
    LNode *p = L;
    int j=0;
    while(p!=NULL && j<i-1){
        p = p->next;
        j++;
    }
    if(p==NULL) return false;
    LNode *s = (LNode*)malloc(sizeof(LNode));
    s->data = e;
    s->next = p->next;
    p->next = s;
    return true;
}
```

删除结点
```cpp
bool ListDelete(LinkList &L,int i,ElemType &e){
    LNode *p=L;
    int j=0;
    while(p->next != NULL && j<i-1){
        p = p->next;
        j++;
    }
    if(p->next==NULL || j>i-1) return false;
    LNode *q = p->next;
    e = q->data;
    p->next = q->next;  //相当于p->next = p->next->next;
    free(q);
    return true;
}
```

删除结点*P 
```cpp
//用删除*P的后继结点来实现
q = p->next;
p->data = p->next->next;
p->next = q->next;
free(q);
```

采用头插法建立单链表
```cpp
LinkList List_HeadInsert(LinkList &L){
    LNode *s; int x;
    L = (LNode*) malloc (sizeof(LNode));
    l->next = NULL;
    std::cin>>x; //scanf("%d",&x);
    while(x!=9999){
        s=(LNode*) malloc (sizeof(LNode));
        s->data = x;
        s->next = L->next;
        l->next = s;
        std::cin>>x;
    }
    return L;
}
```


采用尾插法建立单链表
```cpp
LinkList List_TailInsert(LinkList &L){
    int x;
    L = (LNode*) malloc (sizeof(LNode));
    LNode *s,*r=L;
    std::cin>>x;  //scanf("%d",&x);
    while(x!=9999){
        s = (LNode*) malloc (sizeof(LNode));
        s->data = x;
        r->next = s;
        r = s;
        std::cin>>x;
    }
    r->next = NULL;
    return L;
}
```

---
### 双链表
结点类型
```cpp
typedef struct DNode{
    ElemType data;
    struct DNode *prior, *next;
}DNode, *DLinkList; 
//同时创建了两个结构体，结构相同但名字不同
//它强制在代码中区分“节点”和“链表头”
//用 DNode *p 表示临时游标指针（正在遍历的某个节点）。
//用 DLinkList L 表示整个链表的头指针（代表这张表）。
```

初始化
```cpp
bool InitList(DLinkList &L){
    L = (DNode*) malloc (sizeof(DNode));
    if(L == NULL) return false;
    L->prior = NULL;
    L->next = NULL;
    return true;
}
```

求表长
```cpp
int Length(DLinkList L){
    int count = 0;
    DNode *p = L->next;
    while(p!=NULL){
        count++;
        p = p->next;
    }
    return count；
}
```

插入操作
```cpp
bool ListInsert(DLinkList &L, int i, ElemType e){
    DNode *p=L;
    int j=0;
    while(p!=NULL && j<i-1){
        p = p->next;
        j++;
    }
    if(p==NULL){
        return false;
    }
    DNode *s = (DNode*) malloc (sizeof(DNode));
    if(s==NULL) return false; //健壮性判定，内存分配失败
    s->data = e;
    s->next = p->next;
    s->prior = p;
    if(p->next != NULL){
        p->next->prior = s;
    }
    p->next = s;
    return true;
}
```

按位查找
```cpp
DNode* GetElem(DLinkList L,int i){
    if(i<0) return NULL;
    DNode *p = L;
    int j=0;
    while(p!=NULL && j<i){
        p=p->next;
        j++;
    }
    return p;
}
```

按值查找
```cpp
DNode* LocateElem(DLinkList L,ElemType e){
    DNode *p = L->next;
    while(p!+NULL && p->data!=e){
        p = p->next;
    }
    return p;
}
```

尾插法创建双链表
```cpp
void CreateList_Tail(DLinkList &L, int n){
    InitList(L);
    DNode *r = L;
    for(int i=0;i<n;i++){
        DNode *s = (DNode*) malloc(sizeof(DNode));
        std::cin>>s->data;
        s->prior = r;
        s->next = NULL;
        r->next = s;
        r = s;
    }
}
```

头插法创建双链表
```cpp
void CreateList_Head(DLinkList &L,int n){
    InitList(L);
    for(int i=0;i<n;i++){
        DNode *s = (DNode*) malloc (sizeof(DNode));
        std::cin>>s->data;
        s->next = L->next;
        s->prior = L;
        if(L->next != NULL){
            L->next->prior = s;
        }
        L->next = s;
    }
}
```

删除操作
```cpp
bool ListDelete(DLinkList &L, int i,ElemType &e){
    if(i<1) return false;
    DNode *p = L->next;
    int j = 1;
    while(p!=NULL && j<i){
        p = p->next;
        j++;
    }
    if(p==NULL) return false;
    e = p->data;
    p->prior->next = p->next;
    if(p->next != NULL){
        p->next->prior = p->prior;
    }
    free(p);
    return false;
}
```

摧毁整表
```cpp
void DestoryList(DLinkList &L){
    DNode *p = L;
    while(p!=NULL){
        DNode *q = p->next;
        free(p);
        p = q;
    }
    L = NULL;
}
```
---
### 循环单链表
结点类型
```cpp
typedef struct LNode{
    ElemType data;
    struct LNode *next;
}LNode, *LinkList;
```

初始化
```cpp
bool InitList(LinkList &L){
    L = (LNode*) malloc (sizeof(LNode));
    if(L==NULL) return false; //内存分配失败
    L->next = L;  //头结点的 next 指向自己，形成循环
    return true;
}
```

判空
```cpp
bool Empty(LinkList L){
    return L->next == L; //头结点的next指向它本身，说明为空
}
```

求表长
```cpp
int Length(LinkList L){
    int count=0;
    LNode *p = L->next;
    while(p!=L){
        count++;
        p=p->next;
    }
    return count;
}
```

删除操作
```cpp
bool ListDelete(LinkList &L,int i,ElemType &e){
    if(i<1 || L->next==L) return false;
    LNode *p = L;
    int j=0;
    while(p->next!=NULL && j<i-1){
        p = p->next;
        j++;
    }
    if(j!=i-1 || p->next==L) return false;
    LNode *q = p->next;
    e = q->data;
    p->next = q->next;
    free(q);
    return true;
}
```

按位查找
```cpp
LNode* GetElem(LinkList L,int i){
    if(i<1) return NULL;
    LNode *p = L->next;
    int j=1;
    while(p!=L && j<i){
        p = p->next;
        j++;
    }
    if(p==L) return NULL;
    return p;
}
```

按值查找
```cpp
LNode* LocateElem(LinkList L,ElemType e){
    LNode *p = L->next;
    while(p!=L && p->data!=e){
        p = p->next;
    }
    if(p==L) return NULL;
    return p;
}
```

尾插法创建循环单链表
```cpp
void CreateList_Tail(LinkList &L,int n){
    InitList(L);
    LNode *r = L; // r 指向尾结点（初始指向头结点）
    for(int i=0;i<n;i++){
        LNode *s = (LNode*) malloc (sizeof(LNode));
        std::cin>>s->data;
        s->next = L;
        r->next = s;
        r = s;
    }
}
```

头插法创建循环单链表
```cpp
void CreateList_Head(LinkList &L,int n){
    InitList(L);
    for(int i=0;i<n;i++){
        LNode *s = (LNode*) malloc (sizeof(LNode));
        std::cin>>s->data;
        s->next = L->next;
        L->next = s;
    }
    // 注意：头插法一定要保证尾结点指向头结点
    // 需要找到尾结点，把它的 next 指向 L
    LNode *p = L;
    while(p->next != L){
        p = p->next;
    }
    p->next = L;
}
```

摧毁整表
```cpp
void DestoryList(LinkList &L){
    LNode *p = L->next;
    LNode *q;
    while(p!=L){
        q = p->next;
        free(p);
        p = q;
    }
    free(L);
    L = NULL;
}
```

---
### 循环双链表
结点类型
```cpp
typedef struct DNode{
    ElemType data;
    struct DNode, *DLinkList;
}
```

初始化
```cpp
bool InitList(DLinkList &L){
    L = (DNode*) malloc (sizeof(DNode));
    if(L==NULL) return false; //内存分配失败
    L->prior = L;  //头结点的prior指向自己
    L->next = L;  //头结点的next指向自己
    return true;
}
```


判空
```cpp
bool Empty(DLinkList L){
    return l->next == L;
}
```

插入操作
```cpp
bool ListInsert(DLinkList &L,int i,ElemType e){
    if(i<1) return false;
    DNode *p = L;
    int j=0;
    while(p->next!=NULL && j<i-1){
        p = p->next;
        j++;
    }
    if(j!=i-1) return false;
    DNode *s = (DNode*) malloc (sizeof(DNode));
    s->data = e;
    s->next = p->next;
    s->prior = p;
    p->prior->next = s;
    p->next = s;
    return true;
}
```

删除操作
```cpp
bool ListDelete(DLinkList &L,int i,ElemType e){
    if(i<1 || L->next==L) return false;
    DNode *p = L->next;
    int j=1;
    while(p!=L && j<i){
        p = p->next;
        j++;
    }
    if(p==L) return false;
    e = p->data;
    p->prior->next = p->next;
    p->next->prior = p->prior;
    free(p);
    return true;
}
```

尾插法创建循环双链表
```cpp
void CreateList_Tail(DLinkList &L,int n){
    InitList(L);
    DNode *r=L;
    for(int i=0;i<n;i++){
        DNode *s = (DNode*) malloc (sizeof(DNode));
        std::cin>>s->data;
        s->next = L;
        s->prior = r;
        r->next = s;
        L->prior = s;
        r = s;
    }
}
```

正序遍历
```cpp
void PrintList(DLinkList L) {
    DNode *p = L->next;
    while (p != L) {
        std::cout<<s->data;
        p = p->next;
    }
    std::cout<<"\n";
}
```

逆序遍历
```cpp
void PrintListReverse(DLinkList L) {
    DNode *p = L->prior; // 从尾结点开始
    while (p != L) {
        std::cout<<s->data;
        p = p->prior;
    }
    std::cout<<"\n";
}
```








