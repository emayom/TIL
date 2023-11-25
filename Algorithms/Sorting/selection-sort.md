# 선택 정렬(Selection Sort)  

| `Algorithm` | :thumbsup: `Best` | `Avg` | :thumbsdown: `Worst` | `Space Complexity` | `stable`| `in-place` |
| :---      |   :----:  |   :----:   |   :----:   |   :----:   |   :----:   |   :----:   |
| 선택 정렬(Selection Sort)   | O($n^2$) | O($n^2$) | O($n^2$) | O(1) | | :white_check_mark: |

## 동작 원리 

선택 정렬은 `최솟값(또는 최댓값) 선택`과 `교환`이 기본 동작 원리이다.  
집합을 순회하며 해당 자리에 남은 배열 중 가장 작은 값을 선택, 교환하여 정렬을 거듭한다.  
오름차 순의 경우, 왼쪽 앞에서 부터 순차적으로 위치가 정렬이 된다.   

#### 어쩌다 이름은 :ballot_box_with_check: 선택(Selection) 정렬일까?  
> 가장 작은 숫자를 선택하여 정렬하는 방식으로 선택 정렬(Selection Sort)이라고 명명되었다.   

<br>

## 구현 
#### 의사코드(pseudo code)
- 배열의 첫번째 요소부터 순차적으로 정렬을 시작한다.
- 특정 자리에 오게 될 최솟값(또는 최댓값)을 선택하기 위해 나머지 항목들을 비교하는 과정이 필요하여 이중 `for`문으로 구현한다.  
- 반복함에 따라 정렬에 필요한 비교 항목의 수가 감소하는 것을 구현하기 위해 
    - 내부 반복문(Inner)의 초기식 제어 변수 j의 값은 i + 1 부터 시작한다. 
- 비교 후 최종적으로 선택된 최솟값이 시작한 자리의 값이 아닐 경우 교환한다. 

```js
const array = [25, 4, 49, 22, 27, 32, 14, 7, 12, 40];

const swap = (arr, idx1, idx2) => {
  [arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]];
};

const selectionSort = (arr) => {
  for (let i = 0; i < array.length; i++) {
    let minIdx = i;

    for (let j = i + 1; j < array.length; j++) {
      if (arr[minIdx] > arr[j]) minIdx = j;
    }

    if (i !== minIdx) swap(arr, i, minIdx);
  }
};
```

<br>

## 시간 복잡도 
선택 정렬의 시간 복잡도는 `Best`, `Worst`, `Average` 모두 O($n^2$)이다.  


## 공간 복잡도 
주어진 배열 안에서 교환(swap)을 통해, 정렬이 수행되므로 공간복잡도는 O(1)이다.

<br>

## 결론 
선택 정렬 또한 비교적 단순한 알고리즘이지만, 버블 정렬과 마찬가지로 시간 복잡도가 `Best`의 경우 O($n^2$)으로 **입력 값이 커짐에 따라 연산은 제곱수의 비율로 증가**하는 비효율적인 알고리즘이다. 

버블 정렬의 경우 인접한 요소들 간의 교환(Swap)이 반복되는 반면, 선택 정렬의 경우 교환 횟수를 최소화할 수 있다.  
하지만, 여전히 시간 복잡도가 O($n^2$)으로 인해 대용량 데이터에서는 효율적이지 않은 알고리즘이다.
