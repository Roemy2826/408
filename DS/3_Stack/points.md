## 栈
---
### 顺序栈
结构定义
```cpp
#define MaxSize 50
typedef struct{
    ElemType data[MaxSize];
    int top;
}SqStack;
```


初始化
```cpp
void InitStack(SqStack &S){
    S.top=-1;
}
```

判栈空
```cpp
bool StackEmpty(SqStack S){
    if(S.top == -1) return true; //为空
    else return false;  //不为空
}
```

入栈
```cpp
bool Push(SqStack &S,ElemType x){
    if(S.top == MaxSize-1) return false; 
    S.data[++S.top]=x;
    return true;
}
```

出栈
```cpp
bool Pop(SqStack &S,ElemType &x){
    if(S.top == -1) return false;
    x = S.data[S.top--];
    return true;
}
```

读栈顶元素
```cpp
bool GetTop(SqStack S,ElemType &x){
    if(S.top==-1) return false;
    x = S.data[S.top];
    return true;
}
```

---
### 共享栈
结构定义
```cpp
#define MaxSize 50
typedef struct{
    ElemType data[MaxSize];
    int top1,top2;
}ShStack;
```

初始化
```cpp
void InitStack(ShStack &S){
    S.top1 = -1;
    S.top2 = MaxSize;
}
```

判栈空
```cpp
bool Stack1Empty(ShStack S){
    return S.top1 == -1;
}

bool Stack2Empty(ShStack S){
    return S.top2 == MaxSize;
}
```

判栈满
```cpp
bool StackFull(ShStack S){
    if(S.top1+1 == S.top2) return true;  //栈满
    else return false;
}
```

入栈
```cpp
bool Push1(ShStack &S,ElemType x){
    if(S.top1+1 == S.top2) return false;
    S.data[++S.top1] = x;
    return true;
}

bool Push2(ShStack &S,ElemType x){
    if(S.top1+1 == S.top2) return false;
    S.data[--S.top2] = x;
    return true;
}
```

出栈
```cpp
bool Pop1(ShStack &S, ElemType &x){
    if(S.top1 == -1) return false;
    x = S.data[S.top1--];
    return true;
}

bool Pop2(ShStack &S, ElemType &x){
    if(S.top2 == MaxSize) return true;
    x = S.data[S.top2++]；
    return true;
}
```

读栈顶元素
```cpp
bool GetTop1(ShStack S,ElemType &x){
    if(S.top1 == -1) return false;
    x = S.data[S.top1];
    return true;
}

bool GetTop2(ShStack S,ElemType &x){
    if(S.top2 == MaxSize) return false;
    x = S.data[S.top2];
    return true;
}
```

求栈长
```cpp
int Stack1Length(ShStack S){
    return S.top1+1;
}
int Stack2Length(ShStack S){
    return MaxSize-S.top2;
}
```
---
### 链式存储栈（链栈）
结构定义(带头结点)
```cpp
typedef struct LinkNode{
    ElemType data;
    struct LinkNode *next;
}LinkNode;

typedef struct{
    LinkNode *top;
    int length;
}LinkStack;
```

初始化(带头结点)
```cpp
void InitStack(LinkStack &S){
    S.top = (Stack*) malloc (sizeof(LinkNode));
    S.top->next = NULL;
    S.length = 0;
}
```

判栈空(带头结点)
```cpp
bool StackEmpty(LinkStack S){
    if(S.top->next == NULL) return true;
    else return false;
}
```

入栈(带头结点)
```cpp
bool Push(LinkStack &S,ElemType x){
    LinkNode *p = (LinkStack*) malloc (sizeof(LinkNode));
    if(p==NULL) return false;
    p->data = x;
    p->next = S.top->next;
    S.top->next = p;
    S.length++;
    return true;
}
```

出栈(带头结点)
```cpp
bool Pop(LinkStack &S,ElemType &x){
    if(S.top->next == NULL) return false;
    LinkNode *p = S.top->next;
    x = p->data;
    S.top->next = p->next;
    free(p);
    S.length--;
    return true;
}
```
读栈顶元素(带头结点)
```cpp
bool GetTop(LinkStack S,ElemType &x){
    if(S.top->next == NULL) return false;
    x = S.top->next->data;
    return true;
}
```

销毁栈(带头结点)
```cpp
void DestoryStack(LinkStack &S){
    LinkNode *p = S.top->next;
    LinkNode *q;
    while(p! = NULL){
        q = p->next;
        free(p);
        p = q;
    }
    free(S.top);
    S.length=0;
}
```

清空栈（保留头结点）
```cpp
void ClearStack(LinkStack &S){
    LinkNode *p = S.top->next;
    LinkNode *q;
    while(p!=NULL){
        q = p->next;
        free(p);
        p = q;
    }
    S->top->next = NULL;
    S.length=0;
}
```

求栈长(带头结点)
```cpp
int StackLength(LinkStack S){
    return S.length;
}
//如果没有length字段
int StackLength(LinkStack S){
    int count=0;
    LinkNode *p = S.top->next;
    while(p!=NULL){
        count++;
        p = p->next;
    }
    return count;
}
```

结构定义（不带头结点）
```cpp
typedef struct LinkNode{
    ElemType data;
    struct LinkNode *next;
}LinkNode;

typedef struct{
    LinkNode *top;
    int length;
}LinkStack;
```

初始化（不带头结点）
```cpp
void InitStack(LinkStack &S){
    S.top=NULL;
    S.length=0;
}
```

判栈空（不带头结点）
```cpp
bool StackEmpty(LinkStack S){
    return S.top==NULL;
}
```

入栈（不带头结点）
```cpp
bool Push(LinkStack &S,ElemType x){
    LinkNode *p = (LinkNode*) malloc (sizeof(LinkNode));
    if(p == NULL) return false;
    p->data = x;
    p->next = S.top;
    S.top = p;
    S.length++;
    return true;
}
```

出栈（不带头结点）
```cpp
bool Pop(LinkStack &S,ElemType &x){
    if(S.top == NULL) return false;
    LinkNode *p = S.top;
    x = p->data;
    S.top = p->next;
    free(p);
    S.length++;
    return true;
}
```

读栈顶元素（不带头结点）
```cpp
bool GetTop(LinkStack S,ElemType &x){
    if(S.top==NULL) return false;
    x = S.top->data;
    return true;
}
```

