## 矩阵
---
### 对称矩阵
规律 $a[i][j] == a[j][i]$;
压缩后一维数组大小 $n*(n+1)/2$
$$
f(x)=
\begin{cases}
i*(i+1)/2 + j, & i >= j \\
j*(j+1)/2 + i, & i < j
\end{cases}
$$

```cpp
#define ElemType int
class SymmetricMatrix{
private:
    ElemType *data;
    int n;
    int size;
    int getIndex(int i,int j) const{
        if(i<j) std::swap(i,j);
        return i*(i+1)/2 + j;
    }
public:
    SymmetricMatrix(int n):n(n){   //把传进来的参数 n 的值，赋给成员变量 n
        size = n*(n+1)/2;
        data = new ElemType[size];
    }
    ~SymmetricMatrix(){delete[] data;}
    void Set(int i,int j,ElemType val){
        if(i<j) std::swap(i,j);
        data[i*(i+1)/2+j] = val;
    }
    ElemType Get(int i,int j) const{
        if(i<j) std::swap(i,j);
        return data[i*(i+1)/2+j];
    }
    void print() const{  //const告诉编译器：这个函数是只读的，不会改变对象的状态
        for(int i=0; i<n; i++){
            for(int j=0; j<n; j++){
                std::cout<<Get(i,j)<<" ";
            }
            std::cout<<"\n";
        }
    }
};
```


---
### 三角矩阵
规律：一般区域为常数C（通常为0），存储：下三角元素+一个常数
一维数组大小：$n*(n+1)/2 + 1$
```cpp
class LowerTirMatirx{
private:
    ElemType *data;
    int n,constVal;
    int size;
public:
    LowerTriMatrix(int n,int constant=0):n(n),constVal(constant){
        size = n*(n+1)/2 + 1;
        data = new ElemType[size];
        data[size-1]=constVal;
    }
    //":"的作用，不需要手动赋值
    // LowerTriMatrix(int n, int constant = 0) {
    //this->n = n;           // 手动赋值
    //this->constVal = constant;}
    ～LowerTriMatrix(){delete[] data};
    bool Set(int i,int j,ElemType val){
        if(i<j) return false;
        data[i*(i+1)/2+j] = val;
        return true;
    }
    ElemType Get(int i,int j){
        if(i<j) return data[size-1];
        return data[i*(i+1)/2+j];
    }
};
```


---
### 三对角矩阵
规律：只有主对角线，主对角线上下两条副对角线有非零元素，其余为0。
0-based $k=2*i+j$ (i，j 以及 存储都从0开始)
1-based $k=2*i+j-3$ （i，j从1开始，存储从0开始）
```cpp
class TriDiagonalMatrix{
private:
    ElemType *data;
    int n;
    int size;
    bool isVaild(int i,int j) const{
        return std::abs(i-j) <= 1;
    }
public:
    TriDiagonalMatrix(int n):n(n){
        size = 3*n-2;
        data = new ElemType[size](); //因为三对角矩阵其余全为0,因此加（）初始化为0
    }
    ～TriDiagonalMatrix(){delete[] data;}
    bool Set(int i,int j,ElemType val){
        if(!isValid(i,j)) return false;
        data[2*i+j] = val;  //这里用的是0-based
        return true;
    }
    ElemType Get(int i,int j){
        if(!isValid(i,j)) return 0;
        return data[2*i+j];
    }
    void print() const{
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                std::cout<<Get(i,j)<<" ";
            }
            std::cout<<"\n";
        }
    }
};
```

---
### 稀疏矩阵
规律：非零元素个数num远小于$m*n$，使用三元组$(row,col,val)$仅存储非零元素
```cpp
#include <vector>
#include <algorithm>
using namespace std;

typedef struct {
    int row, col;
    ElemType val;
} Triple;

class SparseMatrix {
private:
    int rows, cols;      // 矩阵维度
    int num;             // 非零元个数
    vector<Triple> data; 
public:
    SparseMatrix(int m, int n) : rows(m), cols(n), num(0) {}

    void insert(int r, int c, ElemType v) {
        if (v == 0) return;
        data.push_back({r, c, v});
        num++;
    }

    // 普通转置（时间复杂度 O(cols * num)）
    SparseMatrix transpose() {
        SparseMatrix result(cols, rows);
        for (int c = 0; c < cols; c++) {  // 按列扫描原矩阵
            for (int i = 0; i < num; i++) {
                if (data[i].col == c) {
                    result.insert(c, data[i].row, data[i].val);
                }
            }
        }
        return result;
    }

};
```