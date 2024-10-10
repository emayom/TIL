[프로그래머스 - 가장 큰 수](https://school.programmers.co.kr/learn/courses/30/lessons/42746)

```js
function solution(numbers) {
   const answer = numbers.map(String).sort((a, b) => (b+a) - (a+b)).join('');

    return Number(answer)? answer : '0';
}
```

[프로그래머스 - H-Index](https://school.programmers.co.kr/learn/courses/30/lessons/42747)
```js
// 1. 오름차순 정렬 O(nlogn)
function solution(citations) {
    const idx = citations.sort((a, b)=> a-b).findIndex((v, idx)=> v >= citations.length - idx);  
    
    if(idx === -1) ? 0 : citations.length - idx;
}

function solution(citations) {
    return citations.sort((a, b)=> a-b).filter((v, idx)=> v >= citations.length - idx).length;
}

// 2. 내림차순 정렬 O(nlogn)
function solution(citations) {
    return citations.sort((a, b)=> b-a).filter((v, idx)=> v >= idx+1).length;
}

// 3. 정렬 ❌ O(n)
function solution(citations) {
    const n = citations.length;
    const count = new Array(n+1).fill(0); 

    citations.forEach(citation => (citation >= n)? count[n]++ : count[citation]++);
    
    return count.reduceRight((total, current, idx)=> {
        if(total >= idx) return total;
        if(total + current >= idx) return idx;
        
        return total += current;
    }, 0);
}
```

##### Review 

```
제한 사항
- 과학자가 발표한 논문의 수는 1편 이상 1,000편 이하입니다.
- 논문별 인용 횟수는 0회 이상 10,000회 이하입니다.
```

의사 코드를 작성하기 전에 제한 사항을 먼저 확인했다.   
제한 사항에 따르면 매개변수 `citations` 배열의 최대 크기는 1,000으로 O(n²)의 시간 복잡도를 가진 알고리즘을 사용해도 시간 초과 없이 문제를 해결할 수 있을 것으로 보였다.  

다음으로는 구하고자하는 `H-Index`를 만족하는 조건을 확인했다.   
어떤 과학자가 발표한 논문 `n`편 중  
1) `h`번 이상 인용된 논문이 `h`편 이상
2) <u>나머지 `h`번 이하</u>로 인용되었을 때, 그 최댓값이 H-Index입니다.

첫번째 풀이는 자바스크립트 내장 정렬 메서드 `sort`를 사용하여 배열을 오름차순으로 정렬한 후, `findIndex`를 통해 조건을 만족하는 인덱스를 찾아 H-Index를 계산했다. `sort`는 O(n log n)의 시간 복잡도를 가지며, `findIndex`는 O(n) 시간 복잡도를 가진다. 전체 시간 복잡도는 O(n log n + n)으로, 최종적으로 O(n log n)이 된다.

두 번째 풀이는 내림차순으로 정렬한 후, `filter`를 사용하여 조건을 만족하는 요소들을 필터링하고 그 길이를 리턴하도록 작성하였다. 이 경우에도 `sort`는 O(n log n), `filter`는 O(n)의 시간 복잡도를 가지며, 최종적으로 O(n log n)이 된다. `filter`는 새로운 필터링된 배열을 반환하므로 O(n)의 추가 공간이 필요하지만, 배열의 최대 크기가 1,000이므로 메모리 측면에서는 문제가 되지 않는다.

세번째 풀이는 정렬을 생략하고, 카운팅 배열을 사용하여 각 논문의 인용 횟수를 기록한 뒤 `reduceRight`를 통해 누적합을 계산했다. `forEach`는 O(n), `reduceRight`도 O(n)의 시간 복잡도를 가지므로 전체 시간 복잡도는 O(n + n), 즉 O(n)이다. 이 풀이가 시간 복잡도 면에서는 가장 효율적이지만, 문제의 제한 사항을 고려했을 때 정렬을 사용하는 풀이도 충분히 효율적이다. 
