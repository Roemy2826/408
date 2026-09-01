## 队列
---
### 顺序队列
结构定义
```cpp
#define MaxSize 50
typedef struct{
    ElemType data[MaxSize];
    int front； //队头指针
    int rear;   //队尾指针
}SqQueue;
```

初始化
```cpp
void InitQueue(SqQueue &Q){
    Q.front = 0;
    Q.rear = 0;
}
```

判队空
```cpp
bool QueueEmpty(SqQueue Q){
    if(Q.front == Q.rear) return true;
    else return false;
}
```
判队满
```cpp
bool QueueFull(SqQueue Q){
    if(Q.rear == MaxSize) return true;
    else return false;
}
```

入队 
```cpp
//缺点：会出现“假溢出”，即rear到达MaxSize，但front前面因为出队还有空闲位置
bool EnQueue(SqQueue &Q,ElemType x){
    if(Q.rear == MaxSize) return false;
    Q.data[Q.rear++] = x;
    return true;
}
```

出队
```cpp
bool DeQueue(SqQueue &Q,ElemType &x){
    if(Q.rear == Q.front) return false;
    x = Q.data[Q.front++];
    return true;
}
```

读队头元素
```cpp
bool GetHead(SqQueue Q,ElemType &x){
    if(Q.front == Q.rear) return false;
    x = Q.data[Q.front];
    return true;
}
```

---
### 循环队列
结构定义
```cpp
#define MaxSize 50
typedef struct{
    ElemType data[MaxSize];
    int front; //队头
    int rear;  //队尾
}CirQueue;
```

初始化
```cpp
void InitQueue(CirQueue &Q){
    Q.front = 0;
    Q.rear = 0;
}
```

判队空
```cpp
bool QueueEmpty(CirQueue Q){
    return Q.front == Q.rear;
}
```


判队满
```cpp
bool QueueFull(CirQueue Q){
    return (Q.rear+1)%MaxSize == Q.front;
}
```

```cpp
typedef struct{
    ElemType data[MaxSize];
    int front,rear;
    int size;
}CirQueue;

bool QueueFull(CirQueue Q){
    return Q.size == MaxSize;
}
```

```cpp
typedef struct{
    ElemType data[MaxSize];
    int front,rear;
    int tag;
}CirQueue;

bool QueueFull(CirQueue Q){
    return Q.front == Q.rear && Q.tag == 1;
}
```

入队
```cpp
bool EnQueue(CirQueue &Q,ElemType x){
    if((Q.rear+1)%MaxSize == Q.front) return false;
    Q.data[Q.rear] = x;
    Q.rear = (Q.rear+1) % MaxSize;
    return true;
}
```

出队
```cpp
bool DeQueue(CirQueue &Q, ElemType &x){
    if(Q.front == Q.rear) return false;
    x = Q.data[Q.front];
    Q.front = (Q.front+1)%MaxSize;
    return true;
}
```

读队头元素
```cpp
bool GetHead(CirQueue Q,ElemType &x){
    if(Q.front == Q.rear) return false;
    x = Q.data[Q.front];
    return true;
}
```

求队列长度
```cpp
int QueueLength(CirQueue Q){
    return (Q.rear - Q.front + MaxSize) % MaxSize;
}
```

---
### 链式存储队列（链队）
结构定义
```cpp
typedef struct LinkNode{
    ElemType data;
    struct LinkNode *next;
}LinkNode;

typedef struct{
    LinkNode *front;
    LinkNode *rear;
    int length;
}LinkQueue;
```

初始化
```cpp
void InitQueue(LinkQueue &Q){
    Q.front = (LinkNode*) malloc (sizeof(LinkNode));
    Q.rear = (LinkNode*) malloc (sizeof(LinkNode));
    Q.front->next = NULL;
    Q.length=0;
}
```

判队空
```cpp
bool QueueEmpty(LinkQueue Q){
    return Q.front == Q.rear;
}
```

入队
```cpp
bool EnQueue(LinkQueue &Q,ElemType x){
    LinkNode *p = (LinkNode*) malloc (sizeof(LinkNode));
    if(p == NULL) return false;
    p->data = x;
    p->next = NULL;
    Q.rear->next = p;
    Q.rear = p;
    Q.length++;
    return true;
}
```

出队
```cpp
bool DeQueue(LinkQueue &Q,Elemtype &x){
    if(Q.front == Q.rear) return false;
    LinkNode *p = Q.front->next;
    x = p->data;
    Q.front->next = p->next;
    if(Q.rear == p){  //删除后队空
        Q.rear = Q.front;
    }
    free(p);
    Q.length--;
    return true;
}
```

读队头元素（带头结点）
```cpp
bool GetHead(LinkQueue Q,ElemType &x){
    if(Q.front == Q.rear) return false;
    x = Q.front->next->data;
    return true;
}
```

销毁队列
```cpp
void DestoryQueue(LinkQueue &Q){
    LinkNode *p = Q.front->next;
    LinkNOde *q;
    while(p!=NULL){
        q=p->next;
        free(p);
        p = q;
    }
    free(Q.front);
    Q.front = NULL;
    Q.rear = NULL;
    Q.length = 0;
}
```

---
### 双端队列
结构定义
```cpp
//用循环数组实现
#define MaxSize 50
typedef struct{
    ElemType data[MaxSize];
    int front;
    int rear;
    int size;
}Deque;  //Double-Ended Queue
```

初始化
```cpp
void InitDeque(Deque &Q){
    Q.front = 0;
    Q.rear = 0;
    Q.size = 0;
}
```

判队空
```cpp
bool DequeEmpty(Deque Q){
    return Q.size == 0;
}
```

判队满
```cpp
bool DequeFull(Deque Q){
    return Q.size == MaxSize;
}
```

队头插入
```cpp
bool PushFront(Deque &Q,ElemType x){
    if(Q.size == MaxSize) return false;
    Q.front = (Q.front-1+MaxSize)%MaxSize;
    Q.data[Q.front] = x;
    Q.size++;
    return true;
}
```

队尾插入
```cpp
bool PushBack(Deque &Q,ElemType x){
    if(Q.size == MaxSize) return false;
    Q.data[Q.rear] = x;
    Q.rear = (Q.rear+1)%MaxSize;
    Q.size++;
    return true;
}
```

队头删除
```cpp
bool PopFront(Deque &Q,ElemType &x){
    if(Q.size==0) return false;
    x = Q.data[Q.front];
    Q.front = (Q.front+1)%MaxSize;
    Q.size--;
    return true;
}
```

队尾删除
```cpp
bool PopBack(Deque &Q,ElemType &x){
    if(Q.size == 0) retur false;
    //假设rear 指向队尾元素的“下一个位置”
    Q.rear = (Q.rear-1+MaxSize)%MaxSize;
    x = Q.data[Q.rear];
    Q.size--;
    return true;
}
```

读队头元素
```cpp
bool GetFront(Deque Q,ElemType &x){
    if(Q.size == 0) return false;
    x = Q.data[Q.front];
    return true;
}
```

读队尾元素
```cpp
bool GetBack(Deque Q,ElemType &x){
    if(Q.size == 0) return false;
    x = Q.data[Q.rear-1+MaxSize]%MaxSize;
    return true;
}
```

