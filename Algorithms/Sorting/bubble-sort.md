# 버블 정렬(Bubble Sort)  

| `Algorithm` | :thumbsup: `Best` | `Avg` | :thumbsdown: `Worst` | `Space Complexity` | `stable`| `in-place` |
| :---      |   :----:  |   :----:   |   :----:   |   :----:   |   :----:   |   :----:   |
| 버블 정렬(Bubble Sort) | O($n^2$) | O($n^2$) | O($n^2$) | O(1) | :white_check_mark: | :white_check_mark: |

## 동작 원리 

버블 정렬은 `비교`와 `교환`이 기본 동작 원리이다.  
집합을 순회하며 항목과 다음 항목과 비교한다. 더 크면 교환을 하고, 다시 다음 항목과 비교 더 크면 교환을 거듭한다.   
외부 반복문(Outer)이 한 번 돌 때마다 하나의 요소의 위치가 확정되므로 정렬해야 하는 항목의 수는 감소한다. 

#### 어쩌다 이름은 🫧 버블(Bubble) 정렬일까?  
> 데이터를 정렬하는 과정이 마치 물 깊은 곳에서 일어난 거품이 수면을 향해 올라오는 **버블링**과 같다고 해서 버블 정렬(Bubble Sort)이라고 명명되었다.   
> 버블 정렬을 싱킹 정렬(Sinking Sort)라고도 하는데, 이 경우 가장 큰 값이 하단(왼쪽 끝)으로 간다.   

<br>

## 구현 
- 특정 항목과 나머지 항목들을 비교하는 것을 반복하기 위해 이중 `for`문으로 구현한다.  
- 반복함에 따라 정렬해야 하는 항목의 수는 감소하는 것을 구현하기 위해 
    - 외부 반복문(Outer)은 감소하도록
    - 내부 반복문(Inner)은 감소한 값의 -1보다 작을 때까지 반복하는 조건으로 설정한다.

```js
const array = [25, 4, 49, 22, 27, 32, 14, 7, 12, 40];

const swap = (arr, idx1, idx2) => {
  [arr[idx1], arr[idx2]] = [arr[idx2], arr[idx1]];
};

const bubbleSort = (array) => {
  for (let i = array.length; i > 0; i--) {
    for (let j = 0; j < i - 1; j++) {
      if (array[j] > array[j + 1]) 
        [arr[j], arr[j+1]] = [arr[j+1], arr[j]];
    }
  }
};
```

### 개선된 버블 정렬(Optimized Bubble Sort) 
위의 코드는 실제 배열의 정렬이 완료되었더라도 **반복문의 종료 조건이 만족되지 않았다면 내부 반복문은 계속 돌아간다.**   
만약 배열의 길이가 길다는 가정을 하면 쓸데없는 시간과 비용을 허비하게 된다!  

이 부분을 개선하기 위해 `isSorted`라는 `flag` 변수를 추가했다.  
내부의 반복문이 수행되는 동안 한 번도 스왑되지 않는다면 배열이 정렬되었으므로 비교를 멈춘다. 

```js
// ...

const bubbleSort = (array) => {
  let isSorted;

  for (let i = array.length; i > 0; i--) {
    isSorted = true;
    for (let j = 0; j < i - 1; j++) {
      if (array[j] > array[j + 1]) {
        swap(array, j, j + 1);
        isSorted = false;
      }
    }
    if (isSorted) break;
  }
};
```

+) [VisuAlgo](https://visualgo.net/en/sorting)를 보다가 `do...while`문으로 리팩터링을 해봤다! 

```js
// ...

const bubbleSort = (array) => {
  let isSorted;
  let unSortedLength = array.length;

  do {
    isSorted = true;
    unSortedLength--;

    for (let i = 0; i < unSortedLength; i++) {
      if (array[i] > array[i + 1]) {
        swap(array, i, i + 1);
        isSorted = false;
      }
    }
  } while (!isSorted);
};
```

<br>

## 시간 복잡도(Time Complexity)

버블 정렬의 시간 복잡도는 `Worst`, `Average`의 경우 O($n^2$), `Best`의 경우 O($n$)이다.  
개선된 버블 정렬(Optimized Bubble Sort)의 시간 복잡도는 `Best`의 경우 O($n$)이다. 

## 공간 복잡도(Space Complexity)
주어진 배열 안에서 교환(swap)을 통해, 정렬이 수행되므로 공간복잡도는 O(1)이다.

<br>

## 결론 
버블 정렬은 `비교`와 `교환`을 반복하는 매우 단순한 알고리즘이지만,   
시간 복잡도가 O($n^2$)으로 **입력 값이 커짐에 따라 연산은 제곱수의 비율로 증가**하는 비효율적인 알고리즘이다. 
