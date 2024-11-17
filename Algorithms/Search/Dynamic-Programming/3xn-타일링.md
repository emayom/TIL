---
title: "프로그래머스 - 3 x n 타일링"
category: "Algorithms"
tags: ["Algorithms", "Search", "DP"]
date: 2024-11-16
last_modified_at: 2024-11-17
---

# 3 x n 타일링

<img src="https://img.shields.io/badge/-프로그래머스-1e2a3c" alt="프로그래머스"/> <img src="https://img.shields.io/badge/-Level 2-green" alt="Level 2"/> <img src="https://img.shields.io/badge/-Dynamic Programming-orangered" alt="Dynamic Programming"/> 

- [문제 풀러 가기](https://school.programmers.co.kr/learn/courses/30/lessons/12902)

### 풀이 1: 음수 값 발생 → 실패 ❌

```js
function solution(n) {
    if (n % 2) return 0; 

    const modular = 1000000007;
    const table = new Array(n+1).fill(0);

    table[0] = 1;
    table[2] = 3;

    for (let i = 4; i <= n; i += 2) {
        // ✅ 뺄셈 결과 음수 발생 가능 
        table[i] = (4 * table[i - 2] - table[i - 4]) % modular; 
    }

    return table[n];
}
```

### 풀이 2: 음수 값 방지 처리 

```js
function solution(n) {
    if (n % 2) return 0; 

    const modular = 1000000007;
    const table = new Array(n+1).fill(0);

    table[0] = 1;
    table[2] = 3;

    for (let i = 4; i <= n; i += 2) {
        // ✅ (x + modular) % modular는 항상 0 ~ modular−1 범위에 값을 보장
        table[i] = (4 * table[i - 2] - table[i - 4] + modular) % modular;
    }

    return table[n];
}
```

### 풀이 3: Top-Down 방식 

```js
function solution(n) {
    if(n % 2) return 0;   
    
    const modular = 1000000007;
    const table = new Array(n+1).fill(0);   // ✅ Memoization
    
    table[0] = 1;
    table[2] = 3;
    
    function calc(n){
        if (table[n] > 0) return table[n];
        return table[n] = (4 * calc(n-2) - calc(n-4) + modular) % modular;
    }

    return calc(n);
}
```

## Review 

```
제한사항
- 가로의 길이 n은 5,000이하의 자연수 입니다.
- 경우의 수가 많아 질 수 있으므로, 경우의 수를 1,000,000,007으로 나눈 나머지를 return해주세요.
```

### 점화식 설명 
> $f(n-4) * 2 + f(n-6) * 2 \, ... \, f(2) * 2 + 2$

점화식을 기반으로 가로 길이가 6인 직사각형을 채우는 방법의 수를 계산하면 다음과 같이 구할 수 있다. 

**예시: $n=6$일 때**
- $f(4) * 3 + f(2) * 2 + 2$  
    - $f(4) = 11$, $f(2) = 3$이므로
    - $f(6)=11∗3+3∗2+2=33+6+2=41$

점화식을 통해 구한 답을 증명하기 위해, 직사각형을 채우는 조합을 다음과 같이 세 부분으로 나누어 계산 과정을 설명할 수 있다.

<img src="https://github.com/user-attachments/assets/9efb415c-cc99-4103-8ad0-2bddca478d23" />

1. $f(n-2) * 3$ 

   직전의 타일링 조합의 수(`f(n-2)`)에 새로 추가된 가로 길이 2를 채우는 경우의 수(`f(2)` = 3)를 곱하여 확장된 조합의 수를 구한다.  
    
    <img alt="3xn 타일링 점화식 ① 예시" src="https://github.com/user-attachments/assets/d139ead4-b576-450e-9b22-cacbf1db4c03" />

2. $+ f(n-4) * 2 + f(n-6) * 2 + \, ... \, + f(2) * 2$
    > `n`이 4 이상일 경우에만 계산되며, `n`이 4보다 작으면 의미가 없기 때문에 계산에서 제외된다.

    `n >= 4` 부터는 이전 `n`들의 조합으로 만들 수 없는 특수한 조합, 즉, **고유 모양**(입출력 예 #1의 마지막 2개의 조합과 같은)이 발생하고,  
    `n >= 6` 부터는 이전 고유 모양과 새로 추가된 가로 길이 2를 채우는 조합 간의 대칭적 배치를 고려하여 경우의 수를 추가하는 것이 필요하다.   

    <img alt="3xn 타일링 점화식 ② 예시" src="https://github.com/user-attachments/assets/5958c67d-e9e2-417b-92be-c31fe4c26e15">

3. $+ 2$  

    마지막으로 새롭게 발생한 **고유 모양**의 수 2를 더한다.   

    <img alt="3xn 타일링 점화식 ③ 예시" src="https://github.com/user-attachments/assets/2fdbce50-d143-43cf-9550-eb09718ed948">

### 점화식 간략화 
> $f(n) = 4 * f(n-2) - f(n-4)$ 

| n | result |
|---|--------|
| 2 | 3 |
| 4 | 11 | 
| 6 | 41 | 
| 8 | 153 |

점화식의 $f(n-4) * 2 + f(n-6) * 2 + \, ... \, + f(2) * 2 + 2$ 는 대칭적 배치를 고려하여 추가된 조합의 수를 계산하기 위한 항으로,  
직전의 조합의 수(`f(n-2)`)에서 이전의 조합의 수(`f(n-4)`)를 뺀 값과 반복적으로 동일하다는 규칙을 발견할 수 있다.

점화식을 다시 정리하면, $f(n) = f(n-2) * 3 + f(n-2) - f(n-4)$ 로 나타낼 수 있으며,   
이를 간략화하면, $f(n) = 4 * f(n-2) - f(n-4)$ 라는 최종 점화식을 얻을 수 있다. 
