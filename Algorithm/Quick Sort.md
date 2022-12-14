### 퀵정렬이란?

- 특정 값을 기준으로 큰 값들과 작은 값들로 나누어 두개의 부분 배열로 분할해 나가는 알고리즘이다.
- 분할된 부분배열에 대해서도 특정 값을 기준으로 큰 값과 작은 값의 부분 배열로 분할한다.
- 부분 배열의 크기가 1이 되어 더이상 분할할 수 없게 되었을 때 전체 배열이 정렬된다.
<br>

### 방법

배열을 분할할 때 기준이 되는 특정 값을 피봇이라 부른다.
피봇을 정할 때 되도록이면 실제 중앙 값이면 최선이지만, 알 수 없으므로 물리적인 중앙 값을 설정한다.
어떤 풀이들은 물리적인 맨 첫번째 값을 피봇으로 설정하기도 한다.
<br>

> 피봇보다 작은 값들은 피봇의 왼쪽에, 피봇보다 큰 값들은 피봇의 오른쪽에 위치해야 한다는 정렬 규칙을 세운다.
<br>

이제 위 정렬 기준을 충족시키기 위해 피봇보다 왼쪽에 있는 값 중에서 큰 값들을 오른쪽으로 옮기고, 피봇보다 오른쪽에 있는 값 중에서 작은 값들을 왼쪽으로 옮긴다. 
왼쪽에서부터 피봇 보다 크거나 같은 값을 만나면 해당 인덱스를 기억한다. 마찬가지로 오른쪽에서부터 피봇 보다 작거나 같은 값을 만나면 해당 인덱스를 기억한다. 
두 값을 교환하면 해당 인덱스들의 값은 피봇보다 작은 값은 왼쪽에, 큰 값은 오른쪽에 있어야 한다는 정렬 기준을 충족하게 되고, 왼쪽 인덱스++, 오른쪽인덱스-- 한다.
<br>

위 과정을 왼쪽 인덱스와 오른쪽 인덱스가 교차(왼쪽 인덱스 > 오른쪽 인덱스) 할 때 까지 반복하면, 하나의 피봇을 기준으로 한 한번의 정렬이 완료된다. 
이제 피봇보다 작은 값들로 구성된 왼쪽 부분배열과 피봇 보다 큰 값들로 구성된 오른쪽 부분배열로 나뉘었다. 
두 부분 배열에 대해 다시 피봇을 세워 배열을 분할하는 과정을 부분 배열의 크기가 1이 될 때 까지 반복하면 된다.
<br>

### 코드

이제 자바 코드로 퀵정렬 알고리즘을 구현해보자.

``` java
public class QuickSort {

    private int partition(int[] arr, int start, int end) {
        int pivot = arr[(start + end) / 2]; // (1)

        while (start <= end) { // end가 start보다 작아지면 탈출.
            
            while (arr[start] < pivot) start++;
            while (arr[end] > pivot) end--; // (2)
            
            if(start<=end) {
                swap(arr, start, end);
                start++;
                end--; 
            } // (3)
        }

        return start; // 두번째 부분배열의 시작 인덱스를 리턴.
    }

    private void swap(int[] arr, int start, int end) {
        int tmp = arr[start];
        arr[start] = arr[end];
        arr[end] = tmp;
    }

    public void quickSort(int[] arr, int start, int end) { // (4)
        int start2 = partition(arr, start, end);
        if (start < start2 - 1) quickSort(arr, start, start2 - 1); // 왼쪽 부분배열 크기 > 1
        if (start2 < end) quickSort(arr, start2, end); // 오른쪽 부분배열 크기 > 1
    }
}
```

1.  피봇을 배열의 물리적 중앙 값으로 설정한다.
2.  피봇의 왼쪽 인덱스 값들 중 피봇 보다 큰 값의 인덱스를 기억하고, 피봇의 오른쪽 인덱스 값들 중 피봇 보다 작은 값의 인덱스를 기억한다.
3.  이때 왼쪽 인덱스와 오른쪽 인덱스가 교차되지 않았다면 두 값을 교환한다.
4.  부분 배열의 크기가 1 이상일 때 퀵 정렬을 메서드를 호출한다.
