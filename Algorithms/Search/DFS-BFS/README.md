---
title: "깊이/너비 우선 탐색(DFS/BFS)"
category: "Algorithms"
tags: ["Algorithms", "Search", "DFS/BFS"]
date: 2024-11-04
last_modified_at: 2024-11-04
---

# 깊이/너비 우선 탐색(DFS/BFS)

### 순회(Traversal)

순회는 자료 구조의 **모든 정점(혹은 노드)을 최소한 한 번씩 방문하는 과정**을 의미한다. 

선형(linear) 자료 구조는 논리적(혹은 물리적)으로 연속된 형태로 인덱스 기반 순회가 가능하지만,  
비선형(non-linear) 자료 구조는 계층적이거나 복잡한 연결 구조를 가져 인덱스 기반의 순회는 불가능하거나 비효율적일 수 있다. 

#### 트리 순회(Tree Traversal)
> 트리는 사이클(cycle)이 존재하지 않는 비순환 그래프이다.   
> 즉, 한 번 방문한 노드를 다시 방문할 필요가 없으며, 순회 시 별도의 방문 여부 체크가 필요하지 않다. 

- 전위 순회 (Preorder)  
- 중위 순회 (Inorder)
- 후위 순회 (Postorder)
- 레벨 순회 (Level order traversal)

#### 그래프 순회(Graph Traversal)
> 그래프는 정점(vertex 혹은 node)와 간선(edge)으로 구성된 자료구조를 의미한다.  

- 깊이 우선 탐색(DFS)
- 너비 우선 탐색(BFS)

## 깊이 우선 탐색(DFS, Depth-First Search)
> 백트래킹(Backtracking)은 가지치기(pruning)를 통해 유망하지 않는 경우의 수는 배제한다.  

깊이 우선 탐색(DFS)은 **시작 정점에서 출발해 더 이상 갈 수 없을 때까지 최대한 깊이 내려간 뒤, 갈 수 있는 정점이 없으면 이전 정점으로 돌아가 다른 분기(branch)를 탐색**하는 방식으로 그래프를 순회하는 알고리즘이다. 

DFS는 **스택(Stack)** 자료 구조를 사용하거나 **재귀(Recursion)** 호출을 통해 구현되며, 재귀를 사용한 구현에서는 함수 호출 시마다 현재 함수의 상태가 내부적으로 스택 프레임에 저장되는 특성을 활용한다. 

<details markdown=1>
<summary markdown='span'><b>깊이 우선 탐색(DFS) 구현 - 스택</b></summary>

- 인접 리스트(adjacent list) 
    ```js
    const graph = {
        A: ['B', 'F'],
        B: ['C', 'D'],
        C: [],
        D: ['E'],
    };

    function DFS(graph, startNode){
        const stack = [ startNode ];
        const route = new Set();
        
        while(stack.length){
            const node = stack.pop();

            if(!route.has(node)){
                route.add(node);

                if(graph[node] && Array.isArray(graph[node])){
                    for (let i = graph[node].length - 1; i >= 0; i--) {
                        if(graph[node][i] && !route.has(graph[node][i])) {
                            stack.push(graph[node][i]);
                        }
                    }
                }
            }
        }
        return Array.from(route);   
    }

    DFS(graph, 'A');    // ['A', 'B', 'C', 'D', 'E', 'F']
    ```

- 인접 행렬(adjacent matrix)
    ```js
    const graph = [
        [0, 1, 0, 0, 0, 1], // A
        [1, 0, 1, 1, 0, 0], // B
        [0, 1, 0, 0, 0, 0], // C
        [0, 1, 0, 0, 1, 0], // D
        [0, 0, 0, 1, 0, 0], // E
        [1, 0, 0, 0, 0, 0]  // F
    ];

    function DFS(graph, startNode){
        const stack = [ startNode ];
        const route = new Set();
        
        while(stack.length){
            const node = stack.pop();

            if(!route.has(node)){
                route.add(node);

                for (let i = graph[node].length - 1; i >= 0; i--) {
                    if(graph[node][i] && !route.has(i)) stack.push(i);
                }
            }
        }
        return Array.from(route);   
    }

    DFS(graph, 0);    // [0, 1, 2, 3, 4, 5]
    ```
</details>
<details markdown=1>
<summary markdown='span'><b>깊이 우선 탐색(DFS) 구현 - 재귀</b></summary>

> 반드시 종료 조건인  베이스 케이스(base case)를 설정해야 하며, 탐색 깊이가 깊어질수록 스택 오버플로우에 유의해야 한다. 

- 인접 리스트(adjacent list) 
    ```js
    const graph = {
        A: ['B', 'F'],
        B: ['C', 'D'],
        C: [],
        D: ['E'],
    };

    function DFS(graph, startNode){
        if(!graph[startNode]){
            console.warn(`Start node ${startNode} does not exist in the graph.`);
            return []; 
        }

        const route = new Set();

        function traverse(node){
            if(!route.has(node)){
                route.add(node);

                if(graph[node] && Array.isArray(graph[node])){
                    graph[node].forEach(traverse);
                }
            }
        }

        traverse(startNode);

        return Array.from(route);   
    }

    DFS(graph, 'A');    // ['A', 'B', 'C', 'D', 'E', 'F']
    ```

- 인접 행렬(adjacent matrix)
    ```js
    const graph = [
        [0, 1, 0, 0, 0, 1], // A
        [1, 0, 1, 1, 0, 0], // B
        [0, 1, 0, 0, 0, 0], // C
        [0, 1, 0, 0, 1, 0], // D
        [0, 0, 0, 1, 0, 0], // E
        [1, 0, 0, 0, 0, 0]  // F
    ];

    function DFS(graph, startNode){
        if(!graph[startNode]){
            console.warn(`Start node ${startNode} does not exist in the graph.`);
            return []; 
        }

        const route = new Set();

        function traverse(node){
            if(!route.has(node)){
                route.add(node);
                graph[node].forEach((v, i) => v && traverse(i));
            }
        }

        traverse(startNode);

        return Array.from(route);   
    }

    DFS(graph, 0);    // [0, 1, 2, 3, 4, 5]
    ```

</details>

## 너비 우선 탐색(BFS, Breadth-First Search)
> 가까운 인접 정점(adjacent vertex)을 먼저 방문한다는 점에서 DFS와 다르다. 

너비 우선 탐색(BFS)은 **시작 정점에서 출발해 가까운 인접 정점부터 우선적으로 탐색하고, 이후 다음 레벨의 인접 정점을 순서대로 탐색**하는 방식으로 그래프를 순회하는 알고리즘이다. 

BFS는 **큐(Queue)** 자료 구조를 사용하여 구현되며, 한 레벨의 모든 인접한 노드를 큐에 저장하기 때문에 일반적으로 깊이 우선 탐색(DFS)보다 더 많은 메모리를 필요로 한다. 

> 레벨 순회는 트리에 맞게 조정된 BFS의 변형(variant)으로도 볼 수 있다.

<details markdown=1>
<summary markdown='span'><b>너비 우선 탐색(BFS) 구현 - 큐</b></summary>

> [큐 클래스 구현 참고](../../Data-Structure/Stack-Queue/queue.md)

- 인접 리스트(adjacent list) 
    ```js
    const graph = {
        A: ['B', 'F'],
        B: ['C', 'D'],
        C: [],
        D: ['E'],
    };

    function BFS(graph, startNode){
        const queue = new Queue([ startNode ]);
        const route = new Set();
        
        while(queue.size){
            const node = queue.dequeue();

            if(!route.has(node)){
                route.add(node);

                if(graph[node] && Array.isArray(graph[node])){
                    graph[node].forEach(v => queue.enqueue(v));
                }            
            }
        }
        return Array.from(route);   
    }

    BFS(graph, 'A');    // ['A', 'B', 'F', 'C', 'D', 'E']
    ```

- 인접 행렬(adjacent matrix)
    ```js
    const graph = [
        [0, 1, 0, 0, 0, 1], // A
        [1, 0, 1, 1, 0, 0], // B
        [0, 1, 0, 0, 0, 0], // C
        [0, 1, 0, 0, 1, 0], // D
        [0, 0, 0, 1, 0, 0], // E
        [1, 0, 0, 0, 0, 0]  // F
    ];

    function BFS(graph, startNode){
        const queue = new Queue([ startNode ]);
        const route = new Set();
        
        while(queue.size){
            const node = queue.dequeue();

            if(!route.has(node)){
                route.add(node);
                graph[node].forEach((v, i) => v && queue.enqueue(i));         
            }
        }
        return Array.from(route);   
    }

    BFS(graph, 0);    // [0, 1, 5, 2, 3, 4]
    ```
</details>

## 깊이/너비 우선 탐색(DFS/BFS))의 시간 복잡도
> 여기서 `N`은 노드의 개수, `E`는 간선의 개수이다. 

| `Case` | `Time Complexity` |
| :--- | :----: | 
| 인접 리스트(adjacency list) | O(N+E) | 
| 인접 행렬(adjacency matrix) | O(N^2) | 
