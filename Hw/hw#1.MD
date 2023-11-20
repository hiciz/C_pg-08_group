### C프로그래밍 11주차 과제 8조 - 김다은, 유시연 

#### 문제: 정수형 배열 b[ ] = {20, 34, 12, 24, 54, 91, 9, 40, 81, 10}의 최대값, 최소값과 내림차순으로 정렬하는 함수를 만들어 출력하는 프로그램을 작성하시오. 
#### 내림차순으로 정렬하는 함수명은 Sorting() 이고, 1차원 포인터와 배열의 크기를 인자로 받는다. 내림차순 정렬의 예는 다음과 같다.


<details>
<summary>소스 코드</summary>

<div markdown="1">

```c
#include <stdio.h>

int Max(int* pArr, int size);
int Min(int* pArr, int size);
void Sorting(int* pArr, int size);

int main() {
    int b[] = { 20, 34, 12, 24, 54, 91, 9, 40, 81, 10 };
    int size = sizeof(b) / sizeof(b[0]);

    // 정렬 전 배열
    printf("[정렬 전 배열]\n");
    for (int i = 0; i < size; i++) {
        printf("%d ", b[i]);
    }
    printf("\n");

    // 최대값 최소값
    int max_value = Max(b, size);
    int min_value = Min(b, size);
    printf("최대값: %d\n", max_value);
    printf("최소값: %d\n", min_value);

   
    Sorting(b, size);

    
    printf("[내림차순 정렬 후 배열]\n");
    for (int i = 0; i < size; i++) {
        printf("%d ", b[i]);
    }
    printf("\n");

    return 0;
}
int Max(int* pArr, int size) {
    int max = pArr[0];
    for (int i = 1; i < size; i++) {
        if (pArr[i] > max) {
            max = pArr[i];
        }
    }
    return max;
}

int Min(int* pArr, int size) {
    int min = pArr[0];
    for (int i = 1; i < size; i++) {
        if (pArr[i] < min) {
            min = pArr[i];
        }
    }
    return min;
}

void Sorting(int* pArr, int size) {
    for (int i = 0; i < size - 1; i++) {
        for (int j = 0; j < size - i - 1; j++) {
            if (pArr[j] < pArr[j + 1]) {
                int temp = pArr[j];
                pArr[j] = pArr[j + 1];
                pArr[j + 1] = temp;
            }
        }
    }
}

```
</div>
</details>
