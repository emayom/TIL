---
title: "프로그래머스 - 2 x n 타일링"
category: "Algorithms"
tags: ["Algorithms", "Search", "DP"]
date: 2024-11-14
last_modified_at: 2024-11-14
---

# 2 x n 타일링

<img src="https://img.shields.io/badge/-프로그래머스-1e2a3c" alt="프로그래머스"/> <img src="https://img.shields.io/badge/-Level 2-green" alt="Level 2"/> <img src="https://img.shields.io/badge/-Dynamic Programming-orangered" alt="Dynamic Programming"/> 

- [문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/12900)

### 풀이 1: Bottom-Up 방식

```js
function solution(n) {
    const table = new Array(n-1);
    const modular = 1000000007;
    
    table[0] = 1;
    table[1] = 2;

    for(let i = 2; i <= n; i++){
        table[i] = (table[i-1] + table[i-2]) % modular;
    }

    return table[n-1]
}
```

### 풀이 2: Bottom-Up 방식 - 공간 복잡도 개선 

```js
function solution(n) {
    const modular = 1000000007;

    let a = 1;
    let b = 2;
    let temp;

    for (let i = 2; i < n; i++) {
        temp = (a + b) % modular;
        a = b;
        b = temp;
    }

    return b;
}
```
##### Review 

```
제한사항
- 가로의 길이 n은 60,000이하의 자연수 입니다.
- 경우의 수가 많아 질 수 있으므로, 경우의 수를 1,000,000,007으로 나눈 나머지를 return해주세요.
```
