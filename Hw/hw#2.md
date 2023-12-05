### C프로그래밍 13주차 과제 8조 - 김다은, 유시연 

* * *

#### :question: 문제: 멤버 변수로 제목, 지은이, 출판사라는 문자형 변수 3개, 쪽수와 가격이라는 정수형 변수 2개를 가진 구조체 Books를 정의하고, 다음의 책 정보를 담을 수 있는 구조체 배열을 만든다.
<details>
  <summary>문제 상세 내용</summary>
  <img width="863" alt="image" src="https://github.com/hiciz/C_pg-08_group/assets/138213248/33455d9e-1be5-42e9-ad68-06d8346189fc.png">
  
  1. [도서목록]을 선택하면, 책 전체 목록을 보여준다. (3점)
        
        예) Title   Authors   Press   Page   Price
        
           ----     ---------    ------   -------   -------
        
            Truth  John      Century 300    20,000
        
             Love  Paul       Goods   200   15,000
  2. [검색] 선택하면, “검색할 도서를 선택하세요”입력 창이 뜬다. 책 제목(Title)을 입력하면 책 정보를 제공해준다.   
     2-1. 책 제목을 대문자 또는 소문자로 입력해도 검색이 된다.   
     2-2. 보유하고 있지 않은 책 제목을 입력하면 보유하고 있지 않다고 알려준다.   
  3. [대출]을 선택하면, 대출할 책의 이름을 선택하는 문구가 나온다.   
     3-1. 선택한 책이 대출중이면, “대출 중이라 대출 할 수 없습니다” 문구가 나온다.   
     3-2. 선택한 책이 대출가능이면, “대출 되었습니다” 문구가 나오고, 대출중(borrowing)으로 변경   
     3-3. 보유하고 있지 않은 책 제목을 입력하면 보유하고 있지 않다고 알려준다.   
  4. [반납] 선택하면, 반납할 책의 이름을 선택하는 문구가 나온다. (4점)   
     4-1. 선택한 책이 대출증이면, “책이 반납 되었습니다” 문구가 나오고, 대출가능(available)으로 변경된다.   
     4-2. 선택한 책이 대출가능이면, “대출 되지 않은 책입니다.” 문구가 나온다.   
     4-3. 보유하고 있지 않은 책 제목을 입력하면 보유하고 있지 않다고 알려준다.   
  5. [종료] 버튼을 누르면, 프로그램이 종료된다.   
</details>




<details>
<summary>소스 코드</summary>

<div markdown="1">

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

//strcmp 오류시 대처 
#ifdef _WIN32
#define STRCASECMP _stricmp
#else
#define STRCASECMP strcasecmp
#endif

#define MAX_TITLE_LENGTH 100
#define MAX_AUTHORS_LENGTH 100
#define MAX_PRESS_LENGTH 100

typedef struct {
    char Title[MAX_TITLE_LENGTH];
    char Authors[MAX_AUTHORS_LENGTH];
    char Press[MAX_PRESS_LENGTH];
    int Page;
    int Price;
    int borrowed; // 1 -> 대출가능 , 0 -> 대출불가
} Book;

void printBook(Book list[], int num); //책목록
void searchBook(Book list[], int num); //책검색
void outBook(Book list[], int num); //책대출
void inBook(Book list[], int num); //책반납

int main() {
    int user_choice;
    int total_num_book = 5;

    Book list[5] = {
        {"Truth", "John", "Century", 300, 20000, 1},
        {"Love", "Paul", "Goods", 200, 15000, 1},
        {"Joy", "James", "Cookie", 250, 18000, 1},
        {"Thanks", "Mark", "Saesong", 240, 21000, 1},
        {"God", "Johnson", "Jungjo", 450, 35000, 1},
    };

    while (1) {
        printf("\n [도서목록] [검색] [대출] [반납] [종료]\n");
        printf("1번: [도서목록]\n");
        printf("2번: [검색]\n");
        printf("3번: [대출]\n");
        printf("4번: [반납]\n");
        printf("5번: [종료]\n");

        printf("\n 번호를 입력하세요: \n");
        scanf_s("%d", &user_choice);

        switch (user_choice) {
            case 1:
                printBook(list, total_num_book);
                break;

            case 2:
                searchBook(list, total_num_book);
                break;

            case 3:
                outBook(list, total_num_book);
                break;

            case 4:
                inBook(list, total_num_book);
                break;

            case 5:
                printf("프로그램을 종료합니다.\n");
                exit(0);

            default:
                printf("잘못입력했습니다.\n");
        }
    }

    return 0;
}

void printBook(Book list[], int num) { //목록함수구현
    printf("Title\tAuthors\tPress\tPage\tPrice\tBorrow\n");
    printf("-----\t------\t-----\t-----\t-----\t------\n");
    for (int i = 0; i < num; i++) {
        printf("%s\t%s\t%s\t%d\t%d\t%s\n", list[i].Title, list[i].Authors, list[i].Press, list[i].Page, list[i].Price,
            (list[i].borrowed == 1) ? "대출가능(available)" : "대출중(borrowing)"); //1씩 증가하면서 리스트 출력
    }
}

void searchBook(Book list[], int num) { //검색함수구현
    char searchTitle[MAX_TITLE_LENGTH];
    printf("검색할 도서를 선택하세요: ");
    scanf_s("%s", searchTitle, (unsigned)_countof(searchTitle));

    int found = 0;
    for (int i = 0; i < num; i++) {
        if (STRCASECMP(list[i].Title, searchTitle) == 0) { //대소문자구별없이검색
            printf("Title\tAuthors\tPress\tPage\tPrice\tBorrow\n");
            printf("-----\t------\t-----\t-----\t-----\t------\n");
            printf("%s\t%s\t%s\t%d\t%d\t%s\n", list[i].Title, list[i].Authors, list[i].Press, list[i].Page, list[i].Price,
                (list[i].borrowed == 1) ? "대출가능(available)" : "대출중(borrowing)");
            found = 1;
            break;
        }
    }

    if (!found) {
        printf("보유하고 있지 않은 책 제목입니다.\n");
    }
}

void outBook(Book list[], int num) { //대출함수
    char borrowTitle[MAX_TITLE_LENGTH];
    printf("대출할 책의 이름을 선택하세요: ");
    scanf_s("%s", borrowTitle, (unsigned)_countof(borrowTitle));

    int found = 0;
    for (int i = 0; i < num; i++) {
        if (STRCASECMP(list[i].Title, borrowTitle) == 0) { //입력받은 책 제목이 일치한 경우 
            if (list[i].borrowed == 1) {
                list[i].borrowed = 0;
                printf("대출 되었습니다.\n"); //대출될경우 0으로 변경 
            } else {
                printf("대출 중이라 대출 할 수 없습니다.\n"); //대출되어 0인 경우
            }
            found = 1;
            break;
        }
    }

    if (!found) {
        printf("보유하고 있지 않은 책 제목입니다.\n");
    }
}

void inBook(Book list[], int num) { //반납함수
    char returnTitle[MAX_TITLE_LENGTH];
    printf("반납할 책의 이름을 선택하세요: ");
    scanf_s("%s", returnTitle, (unsigned)_countof(returnTitle)); //부호없는 정수

    int found = 0;
    for (int i = 0; i < num; i++) {
        if (STRCASECMP(list[i].Title, returnTitle) == 0) {
            if (list[i].borrowed == 0) {
                list[i].borrowed = 1;
                printf("책이 반납 되었습니다.\n");
            } else {
                printf("대출 되지 않은 책입니다.\n");
            }
            found = 1;
            break;
        }
    }

    if (!found) {
        printf("보유하고 있지 않은 책 제목입니다.\n");
    }
}


```
</div>
</details>

<details>
<summary>실행 결과 사진</summary>

<div markdown="1">

1. 1번[도서목록] 선택 시:         
<img src="https://github.com/hiciz/C_pg-08_group/assets/138213248/1703a9b1-273e-4ba9-8fe2-fc17785a7908.png" width="40%" height="30%" alt="1번"></img>   
    
2. 2번 [검색] 선택 시:   
<img src="https://github.com/hiciz/C_pg-08_group/assets/138213248/141dc6cb-e44f-4e2e-863a-5e44c51e1ee7.png" width="40%" height="30%" alt="2번"></img>   
📌대소문자 상관없이 검색이 되는 모습
   
3. 3번 [대출] 선택 시:   
<img src="https://github.com/hiciz/C_pg-08_group/assets/138213248/5ae96f0f-33ae-4dc7-9797-eedf8258b7c3.png" width="40%" height="30%" alt="3번"></img>   
📌대출 선택을 하면, 대출중으로 바뀐다.  
   
4. 4번 [반납] 선택 시:   
<img src="https://github.com/hiciz/C_pg-08_group/assets/138213248/9e523793-f988-4510-bba4-b2a2dcdc7b86.png" width="40%" height="30%" alt="4번"></img>   
📌없는 제목을 작성하면 없다고 뜨며 초기 화면으로 돌아간다.   

5. 5번 [종료] 선택 시:   
<img src="https://github.com/hiciz/C_pg-08_group/assets/138213248/8e2c2dc2-31e9-45ca-8cf1-cf31ca301af4.png" width="40%" height="30%" alt="5번"></img>   
📌프로그램이 종료되는 모습.   



</div>
</details>
