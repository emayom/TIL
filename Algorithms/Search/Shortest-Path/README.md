---
title: "최단 경로(Shortest Path)"
category: "Algorithms"
tags: ["Algorithms", "Search", "Shortest Path"]
date: 2024-12-04
last_modified_at: 2024-12-06
---

# 최단 경로(Shortest Path)
> 너비 우선 탐색(BFS) 알고리즘은 특정 상황에서 최단 경로를 보장할 수 있는 탐색 알고리즘이다. 

최단 경로 탐색 알고리즘은 **그래프에서 두 노드 간의 가장 짧은 경로를 찾는 알고리즘**을 의미한다. 

그래프의 최단 경로를 찾는 문제는 **가중치가 없는 그래프(Unweighted Graph)** 에서 가장 적은 간선을 통해 도달하는 경로를 찾는 문제와, **가중 그래프(Weighted Graph)** 에서 간선의 가중치 합이 최소가 되는 경로를 찾는 문제로 나눌 수 있다. 여기서 가중치(Weight)의 경우 거리, 비용, 시간 등으로 표현될 수 있으며, 경우에 따라 음수로 주어지기도 한다.

#### 최단 경로 문제의 종류 
- 단일 출발지 최단 경로 (Single-Source Shortest Path, SSSP)
- 단일 도착지 최단 경로 (Single-Destination Shortest Path, SDSP)
- 모든 쌍 최단 경로 (All-Pairs Shortest Path, APSP)

#### 최단 경로 알고리즘의 종류

- 다익스트라(Dijkstra) 
- 벨먼-포드(Bellman–Ford)
- 플로이드-워셜(Floyd–Warshall)
- 에이스타(A*)	
- SPFA(Shortest Path Faster Algorithm)

## 다익스트라(Dijkstra) 
> **음수 사이클**이 존재한다면 사용할 수 없다.

다익스트라 알고리즘은 음수 사이클이 존재하지 않는 가중치 그래프에서 **하나의 정점에서 모든 다른 정점으로 가는, 즉, 단일 출발지 최단 경로(Single Source Shortest Path, SSSP)** 문제를 해결하는 대표적인 알고리즘이다.

다익스트라 알고리즘은 시작 정점에서부터 인접한 정점들 중 가장 작은 가중치를 가진 정점을 선택하고, 그 정점의 인접 정점들의 경로를 갱신하며 최단 경로를 탐색한다. 이 과정에서 중복 계산을 피하고, 점차적으로 최단 경로를 갱신하는 방식은 다이나믹 프로그래밍의 최적 부분 구조와 중복된 부분 문제 특성을 포함한다. 또한, 각 단계에서 가장 가중치가 작은 선택(최적의 해)을 우선적으로 하는 방식으로 진행되는 점에서 그리디 알고리즘으로 분류되기도 한다.

### 간단한 다익스트라 알고리즘 구현 - 순차 탐색 

```js
const input = [
    [0, 2, 5, 1, Infinity, Infinity], 
    [2, 0, 3, 2, Infinity, Infinity], 
    [5, 3, 0, 3, 1, 5], 
    [1, 2, 3, 0, 1, Infinity], 
    [Infinity, Infinity, 1, 1, 0, 2], 
    [Infinity, Infinity, 5, Infinity, 2, 0]
];

// O(V²)
function dijkstra(graph, source){   
    const n = graph.length; 
    const visited = new Array(n).fill(false);
    const distance = new Array(n).fill(Infinity);
        
    function getSmallIdx(){
        let min = Infinity;
        let idx = 0;

        distance.forEach((v, i) => {
            if(v < min && !visited[i]){
                min = v;
                idx = i;
            }
        });
        return idx;
    }

    distance.forEach((_, i) => distance[i] = graph[source][i]);
    
    // 출발지 초기화
    distance[source] = 0;    
    visited[source] = true;
    
    for(let i = 0; i < n - 2; i++){
        const current = getSmallIdx();
        
        visited[current] = true;
        
        for(let j = 0; j < n; j++){
            if(visited[j]) continue;

            const weight = distance[current] + graph[current][j];

            if(weight < distance[j]) distance[j] = weight;
        }
    }
    
    return distance;
}

dijkstra(input, 0); // [0, 2, 3, 1, 2, 4]
```

#### 의사코드(pseudo code)

- 방문 여부를 기록할 배열 `visited`를 생성하고, `false`로 초기화한다.
- 각 노드까지의 최소 거리를 저장할 배열 `distance`를 생성하고, 무한대(∞)로 초기화한다. 
- 출발지 노드로부터 다른 모든 노드까지의 초기 거리를 `graph`에서 가져와 `distance` 배열에 저장한다.
- 출발지 노드의 거리를 0으로 설정한다.
- 출발지 노드를 방문 처리한다.
- `i`를 2부터 `n-2`까지 1씩 증가시키며 반복한다. 
    - 아직 방문하지 않은 노드 중 최소 거리를 가진 노드의 인덱스를 `current`에 저장한다. 
    - `current` 노드를 방문 처리한다.
    - `j`를 0부터 `n`까지 1씩 증가시키며 반복한다. 
        - `j`가 이미 방문되었으면 건너뛴다. 
        - `current`를 경유하여 `j`에 도달하는 거리를 계산한다. 
        - 계산된 거리가 현재 `distance[j]`보다 작으면, `distance[j]`를 갱신한다.
- `distance` 배열을 반환한다. 


### 개선된 다익스트라 알고리즘 구현 - 우선순위 큐(Priority Queue)
> [우선순위 큐(Priority Queue) 구현 참고](../../Data-Structure/README.md)

```js
// O((V + E) log V) → E = 간선의 수
function dijkstra(graph, source){  
    const n = graph.length;  
    const pq = new PriorityQueue();
    const distance = new Array(n).fill(Infinity);

    // 출발지 초기화
    distance[source] = 0;   
    pq.enqueue([ distance[source], source ]);
        
    while(!pq.isEmpty()){
        const [dist, current] = pq.dequeue();

        // 최단 거리가 아닐 경우 
        if (distance[current] < dist) continue;

        for(let j = 0; j < n; j++){
            const weight = dist + graph[current][j];

            if (weight < distance[j]){
                distance[j] = weight;
                pq.enqueue([ weight, j ]);
            }
        }
    }
    return distance;
}
```

#### 의사코드(pseudo code)

- 우선순위 큐 `pq`를 생성한다.
- 각 노드까지의 최소 거리를 저장할 배열 `distance`를 생성하고, 무한대(∞)로 초기화한다. 
- 출발지 노드의 거리를 0으로 설정한다.
- `(0, 출발지 노드)`를 우선순위 큐에 삽입한다.
- 우선순위 큐가 비어 있지 않은 동안 반복한다.
    - 큐에서 최소 거리를 가진 노드를 꺼내 `dist`와 `current`로 저장한다.
    - 꺼낸 노드의 현재 거리가 `distance[current]`보다 크다면, 이미 방문되었므로 건너뛴다.
    - `j`를 0부터 `n`까지 1씩 증가시키며 반복한다. 
        - `current`를 경유하여 `j`에 도달하는 거리를 계산한다. 
        - 계산된 거리가 현재 `distance[j]`보다 작으면, 
            - `distance[j]`를 갱신한다.
            - `(갱신된 거리, 인접 노드)`를 큐에 삽입한다.
- `distance` 배열을 반환한다. 

## 벨먼-포드(Bellman–Ford)
> 그래프에서 음수 가중치를 가진 간선이 있을 때 최단거리를 구하는 알고리즘 (음수 사이클이 있으면 동작하지 않습니다.)

## 플로이드 워셜(Floyd Warshall)

플로이드-워셜 알고리즘은 한 번 실행하여 모든 노드 간 최단 경로

## 에이스타(A*)	

## SPFA(Shortest Path Faster Algorithm)