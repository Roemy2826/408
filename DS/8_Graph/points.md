## 图
图的类型：有向图，无向图，简单图，多重图，强连通图，完全图，稠密图，稀疏图

### 邻接矩阵存储（适合稠密图）
结构定义
```cpp
#include <iostream>
#include <queue>
#include <stack>
#include <climits>  //INT_MAX
using namespace std;
#define MaxVertexNum 100;
#define INFINITY INT_MAX;
typedef char VertexType;
typedef int EdgeType;
typedef struct{
    VertexType vexs[MaxVertexNum];
    EdgeType edges[MaxVertexNum][MaxVertexNum];
    int vexNum,edgeNum;
}Mgraph;
```

初始化
```cpp
void InitGraph(MGraph &G,int n){
    G.vexNum = n;
    G.edgeNum = 0;
    //初始化顶点数据为：A，B，C ...
    for(int i=0;i<n;i++){
        G.vexs[i] = 'A' + i;
    }
    //初始化邻接矩阵：对角线为0,其他全为INFINITY
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            if(i==j) G.edges[i][j]=0;
            else G.edges[i][j] = INFINITY;
        }
    }
}
```

创建
```cpp
bool AddEdge(MGraph &G,int i,int j,int weight){
    if(i<0 || i>=G.vexNum || j<0 || j>=G.vexNum){
        return false;
    }
    G.edges[i][j] = weight;
    G.edges[j][i] = weight;
    G.edgeNum++;
    return true;
}
```

深度优先遍历
```cpp
bool visited[MaxVertexNum];
void DFS(MGraph G,int v){
    visited[v] = true;
    cout<<G.vexs[v]<<" ";
    for(int i=0;i<G.vexNum;i++){
        if(G.edges[v][j] != INFINITY && G.edges[v][i]!=0 && !visited[i]){
            DFS(G,i);
        }
    }
}
void DFSTraverse(MGraph G){
    for(int i=0;i<G.vexNum;i++){
        visited[i] = false;
    }
    for(int i=0;i<G.vexNum;i++){
        if(!visited[i]){
            DFS(G,i);
        }
    }
}
```

广度优先遍历
```cpp
void BFS(MGraph G,int v){
    queue<int> Q;
    visited[v] = true;
    Q.push(v);
    while(!Q.empty()){
        int u = Q.front();
        Q.pop();
        cout<<G.vexs[u]<<" ";
        for(int i=0;i<G.vexNum;i++){
            if(G.edges[u][i]!=INFINITY && G.edges[u][i]!=0 && !visited[i]){
                visited[i] = true;
                Q.push(i);
            }
        }
    }
}
void BFSTraverse(MGraph G){
    for(int i=0;i<F.vexNum;i++){
        visited[i] = false;
    }
    for(int i=0;i<G.vexNum;i++){
        if(!visited[i]){
            BFS(G,i);
        }
    }
}
```

求最短路径（Dijkstra算法）（单源点 $V_0$ 到其余各点）
```cpp
void Dijkstra(MGraph G,int v0){
    int dist[MaxVertexNum];  //存储最短路径
    bool s[MaxVertexNum];  //标记是否已找到最短路径
    int path[MaxVertexNum];  //记录前驱结点（用来回溯）
    //初始化
    for(int i=0;i<G.vexNum;i++){
        dist[i] = G.edges[v0][i];
        s[i] = false;
        if(G.edges[v0][i]<INFINITY) path[i] = v0;
        else path[i] = -1;
    }
    s[v0] = true;
    dist[v0] = 0;
    for(int i=1;i<G.vexNum;i++){
        int minDist = INFINITY;
        int u=v0;
        for(int j=0;j<G.vexNum;j++){
            if(!s[j] && dist[j]<minDist){
                minDist = dist[j];
                u = j;
            }
        }
        s[u] = true;
        for(int j=0;j<G,vexNum;j++){
            if(!s[j] && G.edges[u][j]!=INFINITY){
                int newDist = dist[u] + G.edges[u][j];
                if(newDist<dist[j]){
                    dist[j] = newDist;
                    path[i] = u;
                }
            }
        }
    }
    for(int i=0;i<G.vexNum;i++){
        cout<<"从"<<G.vexs[v0]<<"到"<<G.vexs[i]<<"的最短距离为"<<dist[i]<<"\n";
    }
}
```


### 邻接表存储（适用于稀疏图）
结构定义
```cpp
//边表结点
typedef struct ArcNode{
    int adjVex;
    int weight;
    struct ArcNode *next;
}ArcNode;
//顶点表结点
typedef struct VNode{
    VertexType data;
    ArcNode *firstEdge;
}VNode;
//图
typedef struct{
    VNode adjList[MaxVertexNum];
    int vexNum,edgeNum;
}ALGraph;
```

创建
```cpp
void InitALGraph(ALGraph &G,int n){
    G.vexNum = n;
    G.edgeNum = 0;
    for(int i=0;i<n;i++){
        G.adjList[i].data = 'A' + i;
        G.adjList[i].firstEdge = NULL;
    }
}
```
插入有向边（$i -> j$）
```cpp
void AddArc(ALGraph &G,int i,int j,int w=1){
    ArcNode *p = (ArcNode*)malloc(sizeof(ArcNode));
    p->adjVex = j;
    p->weight = w;
    p->next = G.adjList[i].firstEdge;
    G.adjList[i].firstEdge = p;
    G.edgeNum++;
}
```

插入无向边
```cpp
void AddUndirectedEdge(ALGraph &G,int i,int j,int w=1){
    AddArc(G,i,j,w);
    AddArc(G,j,i,w);
}
```

DFS(遍历边表)
```cpp
void DFS(ALGraph G,int v){
    visited[v] = true;
    cout<<G.adjList[v].data<<" ";
    ArcNode *p = G.adjList[v].firstEdge;
    while(P!=NULL){
        int j = p->adjVex;
        if(!visited[j]) DFS(G,j);
        p = p->next;
    }
}
```

BFS
```cpp
void BFS(ALGraph G,int v){
    queue<int> Q;
    visited[v] = true;
    Q.push(v);
    while(!Q.empty()){
        int u = Q.front();
        Q.pop();
        cout<<G.adjList.data<<" ";
        ArcNode *p = G.adjList[u].firstEdge;
        while(p!=NULL){
            int j = p->adjVex;
            if(!visited[j]){
                visited[j] = true;
                Q.push(j);
            }
            p = p->next;
        }
    }
}
```

