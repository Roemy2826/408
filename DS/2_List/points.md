### 线性表

**静态分布**的线性表

```cpp
#define MaxSize 50  
typedef struct{
   Elemtype data[MaxSize];
   int length;
}SqList;
```

**动态分布**的线性表

```cpp
#define InitSize 100
typedef struct{
    Elemtype *data;
    int MaxSize,length;
}SeqList;
```

C的初始动态分配语句为：
```c
L.data = (Elemtype*) malloc (sizeof(Elemtype)*InitSize);
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
    if(i<1 || i>L.length+1){
        return false;
    }
    if(L.length >= MaxSize){
        return false;
    }
    for(int i=L.length; j>=i; j--){
        L.data[i-1] = e;
        L.length++;
        return true;
    }
}
```


