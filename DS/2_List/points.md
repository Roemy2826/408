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








