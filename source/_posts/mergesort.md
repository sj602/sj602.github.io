---
title: "[알고리즘] 병합정렬(머지소트/Merge Sort)"
categories:
    - 알고리즘
tags:
- 알고리즘
- 병합정렬
- 머지소트
- mergesort
thumbnail:
---

병합정렬(Merge Sort)


나누는 부분인 mergesort()와
나눈 부분을 정렬을 하면서 합치는 merge()로 구분할 수 있다.
중간 중간 정렬된 원소들이 들어갈 임시 배열 sorted를 선언해줘야 한다.


~~~
int arr[10];
int sorted[10];

mergesort(int arr[], int left, int right){
    // left가 right보다 작으면 계속 반씩 나눠준다. 크다면 아무런 동작도 하지 않고 빠져 나와 재귀함수의 merge부분으로 들어간다.
    if(left < right) {
        int mid = (left+right)/2;
        
        mergesort(arr, left, mid);
        mergesort(arr, mid+1, right);
        merge(arr, left, mid, right); // 반씩 나눈 부분을 합친다.
    }    
}

merge(int arr[], int left, int mid, int right){
    int i = left, j = mid+1, k = left;
    // 나눈 부분을 합친다
    while(i <= mid && j <= right) {
        if(arr[i] < arr[j]) {
            sorted[k] = arr[i];
            i++;
        } else {
            sorted[k] = arr[j];
            j++;
        }
        k++;
    }
    // 왼쪽 부분이 먼저 끝날수도, 오른쪽 부분이 먼저 끝날수도 있기 때문에 남아있는 원소를 넣어준다.
    if(i > mid) {
        for(; j <= right; j++) {
            sorted[k] = arr[j];
            k++;
        }
    } else {
        for(; i <= mid; i++) {
            sorted[k] = arr[i];
            k++;
        }
    }
    // 최종적으로 sorted배열에 있는 원소를 실제 배열인 arr로 옮겨준다
    for(int m = left; m <= right; m++) {
        arr[m] = sorted[m];
    }
}
~~~

시간복잡도는 평균적인 경우나 최악의 경우에도 O(NlogN)을 보장한다.
왜냐면 원소들의 순서가 어떻든, 퀵소트처럼 이미 정렬이 됬든 안됬든, 무조건 낱개가 될때까지 반씩 나눈 다음에
낱개가 된 원소들끼리 합치면서 정렬을 하기 때문이다.
이미 공부를 했던 정렬인데 영상을 참고하면서 따라했다.
언제나 개념은 알고있지만 실제로 코드로 옮기는 부분에 있어서 문제가 많다.
Keep Going
