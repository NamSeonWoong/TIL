# BFS,DFS공통

먼저 정점(Vertex)의 갯수와 간선(Edge)의 갯수를 받고 연결관계(graph)를 받아서 인접행렬(adjacent array)을 생성한다.

이 정보를 바탕으로 BFS,DFS를 실시한다.

## 1.BFS

1. (Vertex)정점의 갯수만큼 queue를 만든다.

2. visited 행렬을 (V+1)만큼 만든다.

3. front,rear를 -1로 초기화 한뒤 r+=1 상태에서 시작,

4. queue[r]에 V(시작점)을 넣어준다.

5. visited[V]=1로 해준뒤 while(f!=r)을 통해 계속 반복해준다.

   ```
       while (f!=r):
           f+=1
           V=queue[f]
           print(V)
   
           for i in range(1,N+1):
               if adj[V][i]==1 and visited[i]==0:
                   r+=1
                   queue[r]=i
                   visited[i]=1
   ```

   ## 2.DFS
   
   1.현재 위치는 방문표시를 해준다.
   
   2.재귀를 이용해 풀것이므로 언제 종료할것인지 만들어 주는것이 중요하다.
   
   3.for문을 이용해 숫자가 작은것부터 우선적으로 DFS를 실시해준다.
   
   ```
       for i in range(N+1):
           if adj[V][i]==1 and d_visited[i]==0:
               d_visited[i] = 1
   
               if DFS(i,N,cnt+1)==1:
                   return
                   d_visited[i] = 0
   ```
   
   4.방문한 위치는 visited에 1표시후 바로 재귀 호출을 통해 재호출하며, 동시에 cnt를 1씩늘려간다. 호출바로 다음 문장에 visited를 0으로 바꾸어 더이상 갈곳이 없을때 다시 돌 수 있게끔 만들어 준다.



### 전체코드

```
def BFS(V,N): # V는 시작점,N은 도착지점

    queue=[0]*(N)
    f=-1
    r=-1
    visited=[0]*(N+1)

    r+=1
    queue[r]=V
    visited[V]=1

    while (f!=r):
        f+=1
        V=queue[f]
        BFS_arr.append(V)

        for i in range(1,N+1):
            if adj[V][i]==1 and visited[i]==0:
                r+=1
                queue[r]=i
                visited[i]=1

def DFS(V,N,cnt):
    global d_visited
    d_visited[V] = 1

    if cnt==N+1:
        return 1
    DFS_arr.append(V)
    for i in range(N+1):
        if adj[V][i]==1 and d_visited[i]==0:
            d_visited[i] = 1

            if DFS(i,N,cnt+1)==1:
                return
                d_visited[i] = 0

N,M,V=map(int,input().split())
graph=[list(map(int,input().split())) for _ in range(M)]
adj = [[0]*(N+1) for _ in range(N+1)]
for i in range(M):
    n1,n2=graph[i][0],graph[i][1]
    adj[n1][n2] = 1
    adj[n2][n1] = 1

d_visited=[0]*(N+1)
DFS_arr=[]
BFS_arr=[]
DFS(V,N,0)
BFS(V,N)
DFS_arr=map(str,DFS_arr)
BFS_arr=map(str,BFS_arr)
print(' '.join(DFS_arr))
print(' '.join(BFS_arr))

```

