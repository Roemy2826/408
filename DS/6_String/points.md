## 串
---

### 串的存储结构
顺序存储
```cpp
#define MAXLEN 255
typedef struct{
    char ch[MAXLEN];
    int length;
}SString;
```

堆分配存储
仍然是以一组地址连续的存储单元存放串值的字符序列，但存储空间是在程序执行过程中动态分配得到的
```cpp
typedef struct{
    char *ch;
    int length;
}HString;
```

块链存储
核心思想：每个结点可以存储多个字符（称为一个块），块之间用指针链接
```cpp
#define CHUNSIZE 4 //通常取 4 8 16
//块结点结构
typedef struct Chunk{
    char ch[CHUNSIZE];
    struct Chunk *next;
}Chunk;
//块链结构
typedef struct{
    Chunk *head;
    Chunk *tail;
    int length;
}LString
```;

### 串的模式匹配
简单的模式匹配算法,**模式匹配**是指在主串中找到与模式串相同的子串，并返回其所在的位置
```cpp
int Index(SString S,SString T){
    int i=1,j=1; // i 主串 ， j 模式串
    while(i<=S.length && j<=T.length){
        if(S.ch[i]==T.ch[j]){
            ++i;++j;
        }
        else{
            i = i-j+2; //因为 i，j 都从1开始
            j = 1;
        }
    }
    if(j>T.length)   //j从1开始因此要>length,若j从0开始则>=length;
        return i-T.length;  //i 已经在配对的末尾了，要返回起始位置需要减去模式串的长度
    else return 0;
}
```

KMP算法
部分匹配值是指字符串的前缀和后缀的最长相等前后缀长度
例如 “ababa” 的部分匹配值为 “00123”
$$j右滑位数 = 已匹配的字符数-对应的部分匹配值$$
部分匹配值越小，滑动位数越多。因为不对称因此一定不同，无需再比较
$$next[j]=j-右滑位数=j-(已匹配的字符数-对应的部分匹配值)=PM[j-1]+1$$
PM[]是部分匹配值数组
结论：将模式串的PM表右移一位，并且每位都+1,就得到了next数组
```cpp
void get_next(SString T,int next[]){
    int i=1,j=0;
    next[1]=0;
    while(i<T.length){
        if(j==0 || T.ch[i]==T.ch[j]){
            ++i;++j;
            next[i]=j; //如果Pi=Pj，next[j+1]=next[j]+1;
        }
        else{
            j = next[j];
        }
    }
}

int Index_KMP(SString S,SString T,int next[]){
    int i=1;j=1;
    while(i<=S.length && j<=T.length){
        if(j==0 || S.ch[i]==T.ch[j]){
            ++i;++j;
        }
        else{
            j = next[j];
        }
    }
    if(j>T.length)  return i-T.length; //匹配成功
    else  return 0;
}
```
优化后的next数组
如果$P_j=P_{next}[j]$ ，那么$next[j]=next[next[j]]$
因为相等因此无需再进行比较
```cpp
void get_nextval(SString T,int nextval[]){
    int i=1,j=0;
    nextval[1]=0;
    while(i<T.length){
        if(j==0 || T.ch[i]!= T.ch[j]){
            ++i;++j;
            if(T.ch[i]!=T.ch[j]) nextval[i]=j;
            else nextval[i] = nextval[j];
        }
        else {
            j = nextval[j];
        }
    }
}
```

