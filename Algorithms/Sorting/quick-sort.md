# 퀵 정렬(Quick Sort)  

| `Algorithm` | :thumbsup: `Best` | `Avg` | :thumbsdown: `Worst` | `Space Complexity` | `stable`| `in-place` |
| :---      |   :----:  |   :----:   |   :----:   |   :----:   |   :----:   |   :----:   |
| 퀵 정렬(Quick Sort) | O($n\:log\:n$) | O($n\:log\:n$) | O($n^2$)  | O($\:log\:n$) | | :white_check_mark: |

## 동작 원리 
퀵 정렬은 **분할 정복 알고리즘**의 하나로 `파티셔닝`과정을 통해 정렬을 수행한다. 

합병 정렬에서는 주어진 배열의 중간 인덱스를 찾아 반으로 나누어 분할했다면,  
퀵 정렬은 주어진 배열의 기준 원소, `피벗(pivot)`을 중심으로 배열을 분할한다.  
배열은 피벗을 기준으로 비균등한 크기로 분할되며 **<u>실행 속도는 피벗 선택 위치에 따라 달라질 수 있다.</u>**


#### 파티셔닝(Partitioning)
> 배열을 기준 원소(pivot)을 중심으로 두 개의 부분 배열로 나누는 과정을 의미한다.  

파티셔닝은 `피벗(pivot) 선택`, `파티션(partition)`의 과정으로 도식화할 수 있다. 
1. **피벗(pivot) 선택** : 배열에서 피벗을 선택한다. (피벗은 배열의 중간이나 마지막, 무작위로 선택된 값일 수 있다.)
2. **파티션(partition)** : 피벗을 기준으로 배열을 두 부분 배열로 분할한다. (작은 값은 왼쪽, 큰 값은 오른쪽으로 이동시킨다.)

<br>

## 구현 
#### 의사코드(pseudo code)
- 배열의 파티셔닝을 담당할 [헬퍼 함수](#헬퍼-함수)를 먼저 작성한다. 
    - 함수는 `배열`, `시작 인덱스`, `끝 인덱스` 세 개의 인수를 받는다.  
        (기본 값으로는 시작 인덱스는 `0`, 끝 인덱스는 `배열 길이-1`를 가진다.)
    - 편의상 주어진 배열의 첫번째 요소를 피벗으로 선택한다. 
    - 피벗을 기준으로 왼쪽은 작은 값, 오른쪽은 큰 값을 가지도록 배열을 정렬한다. 
    - 정렬된 배열에서의 피벗 인덱스를 반환한다. 
- 정렬을 수행할 시작 인덱스가 끝 인덱스보다 작을 때까지 `파티셔닝`을 반복하며 제자리 정렬을 수행한다.  
- 재귀 호출이 종료되면 정렬된 배열을 리턴한다. 


#### 헬퍼 함수 
```js
const array = [5, 2, 1, 8, 4, 7, 6, 3];

const swap = (arr, idx1, idx2) => {
  [arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]];
};

const pivot = (array, start = 0, end = array.length - 1) => {
  const pivot = array[start];
  let swapIdx = start;

  for (let i = start + 1; i <= end; i++) {
    if (pivot > array[i]) {
      swapIdx++;
      swap(array, swapIdx, i);
    }
  }

  swap(array, start, swapIdx);
  return swapIdx;
}; 
```

#### 퀵 정렬 
```js
const quickSort = (array, start = 0, end = array.length - 1) => {
  if (start < end) {
    const pivotIdx = pivot(array, start, end);

    quickSort(array, start, pivotIdx - 1);
    quickSort(array, pivotIdx + 1, end);
  }
  return array;
};
```

합병 정렬과 마찬가지로 먼저 구현한 헬퍼 함수와 재귀 함수 호출을 통해 나머지 로직들을 구현한다.   
하지만, 퀵 정렬의 경우 **정렬 과정에서 새로운 배열을 만들지 않는** 제자리 정렬을 수행한다. 

<br>

## 시간 복잡도(Time Complexity)

퀵 정렬의 시간 복잡도는 `Best`, `Average`의 경우 O($n\:log\:n$), `Worst`의 경우 O($n^2$)의 시간 복잡도를 가진다.   

#### `Worst` 케이스
이미 정렬된 배열 혹은 거의 정렬된 배열에서 가장 처음이나 마지막 요소를 선택하여 정렬을 시도할 경우 `Worst` 케이스의 시간 복잡도를 가지게 된다.   
이러한 경우를 피하기 위해서는 피벗을 랜덤하게 선택하거나 여러 피벗을 사용하는 등의 최적화 기법이 적용될 수 있다. 


## 공간 복잡도(Space Complexity)
평균 공간복잡도는 O($log\:n$)이지만 피벗을 기준으로 비균등한 크기로 분할되는 `Worst` 케이스의 경우 O($n$)일 수 있다. 

<br>

## 결론 
합병 정렬과 마찬가지로 평균 O($n\:log\:n$)의 시간 복잡도를 가지는 효율적인 정렬 알고리즘이다.  
또한 퀵 정렬은 `제자리 정렬(in-place)`으로 데이터셋이 큰 경우에도 추가적인 메모리를 사용하지 않고 정렬이 가능하다.   

하지만, 작은 데이터셋에서는 다른 간단한 정렬 알고리즘들이 더 효과적일 수 있으며 `Worst` 케이스의 경우 O($n^2$)의 성능을 나타낼 수 있다.   
또한 안정 정렬 알고리즘이 아니므로 **동일한 값에 대한 상대적인 순서를 보장하지 않는다**는 점에 유의하여 사용해야 한다. 
