C++ 문법 정리
=====
귀찮은 관계로 여기에 대략 6강 부터 15강 내용 다 정리 해버릴 거임 ㅅㄱ

## 6.1. NameSpace
----
변수를 선언하다 보면 이름이 같아질 수 있음.    
그러니 공간에 따라 구분하면 편할 것 같다는 생각을 한듯    
그래서 공간을 즉 펜스를 쳐주는 거임 문법은 다음과 같음

```cpp
#include <iostream>

namespace one{
    int var = 5;
}
namespace two{
    int var = 3;
}

int main(void){
    std::cout<< one::var <<std::endl;
    std::cout<< two::var <<std::endl;
    return 0;
}

```
뭐가 결과로 나올까?

<pre><code>5
3
</code></pre>

## 6.2. SRO(Scope Resolution Operator)
----
이름은 거창함 근데 쉬운거임    
위에서 출력 할때 one이나 two 뒤에 붙인  ``` ::: ``` 이거 이거임;

좀 유식한척 하면 다음과 같은 4가지 기능을 한다고함
<pre><code>1. To access a global variable     
    when there is local variable with same name.
2. To define a function outside of class.    
3. To define a class's static variables.
4. In case of multiple Inheritance.
</code></pre>
읽기 귀찮으니 코드로 이해해보자
```cpp
//global로 선언함
int y = 0;
int x = 0;

void f(){       // Functino is a new Block
    int x = 5;  // Local x = 5, it hides global variable
    ::x++;      // sco를 통한 global variable 접근 global x=1
    x++;        // local variable x = 6
    y++;        // global variable y = 1                
}

```

## 6.3. more GCC Compiler Options
----
옵션이 좀 여러가지 있음 원래는 이렇게 씀
``` 
g++ -std=c++2a -g file1.cpp -o prog
```
옵션은 대충 이렇게 됨

- \- o : 실행 파일 이름 설정
- \- std=c++2a : c++ 스탠다드 버전 결정하려고 쓰는거임 (c++11, c++14, c++17, c++2a )
- \- Wall : 뜨는 워닝 까지 포함해서 오류들 다보여줘라 
- \- g : gdb debugger 사용을 위해서 입력
- \- DDEBUG : 파일 안에 선언 해둔 디버거 명령어들 처리하라는 명령어
    - \- I : 포함된 폴더 이름 지정
    - \- L : 라이브러리 폴더 이름 지정
    - \- l : 라이브러리 파일 이름 지정    

<br/>

## 6.4. Input / Output
----
c++ 시작할때 stdio 말고 iostream 쓰지 않음?     
그때 4가지 객체가 초기화됨

- cin : std input
- cout : std output
- cerror : std error
- clog : std log

좀 중요한 놈들이어서 예시좀 보고 가겠음    

### cin object
사용자 한테 입력 받으면 variable한테 넣어줌

```cpp 
cin >> variable;
```
넣어주는 타입은 변수 선언을 따라감     
예를 들어, variable이 double 타입이면 double로 들어감    
좀 긴 예시를 보면 이럼
```cpp 
#include <iostream>

using namespace std;


int main(){
    int i,j;
    cout << "Give Two Numbers \n";
    cin >> i >> j;
    cout << "Sum = " << i+j << "\n";
    return 0;
}
```
실행은 이렇게 됨
```
Give Two Numbers 
1
3
Sum = 4
```
    
ㅋㅋ 근데 교수님이 cin 쓰지 말래(왜 가르킨거임)    
cin은 사용자 입력을 고려할 수 없으니까     
고려하기 위해서는 std::getline()쓰라는 교수님의 말씀
<br/>

### cout object

cout은 << operator랑 같이 사용됨 사용법은 다음과 같음
```cpp
#include <iostream>

int main(){
    int x = 20;
    cout<<"I'm "<< x <<"years old\n";
}
``` 
결과는 다음과 같이 나옴
``` 
I'm 20years old
``` 

줄바꿈 같은 것들은 escape sequence 라고 부름 종류는 다음과 같음(개많음)
- ```\n``` : Newline
- ```\t``` : Tab
- ```\b``` : Backspace
- ```\r``` : Return (대충 커서 시작 인듯)
- ```\\``` : for Black Slash print
- ```\'``` : 작은 따옴표 출력
- ```\''```: 큰 따옴표 출력

<br/>

## 6.5. inline functions
----
In C,메크로를 원래 썼었음 근데 C++에서는 권장을 안함    
그럼 뭐씀? --> inline이라는 친구를 사용함    
원래 이렇게 씀
```c
#define sq(x) ((x)*(x))          //(x)
#define max(x,y) (y < x? x:y)    //(x)
```
대신 이제 이렇게 쓰기로함
```cpp
inline  int sq(int x) {return x * x};
inline  int max(int x, int y) {return (y < x? x : y)};
```
이해가 잘 안될것 같아서 아래에 좀 길게 써둠    
```cpp
#include <iostream>

using namespace std;

inline int max(int x, int y){
    return y<x? x:y;
}
int main(){
   cout<<max(3,4);
    return 0;
}
```
출력은 다음과 같다
```
4
```


## 7.1. Defalut Function Arguments
----
함수에 인자 안넘겨줘도 오류안나게 기본 설정가능함
```cpp
int exp(int x, int k =3){
    if(k==2) return x*x;
    return (exp(x,k-1)*x);
}
```
근데 무조건 defalut arg앞에는 variable이 있어야함
```cpp
int foo(int i, int j = 7);                 // O
int foo(int i==1, int j );                 // X
int foo(int i, int j = 7,int k = 3);       // O
int foo(int i==1, int j = 7,int k = 3);    // O
int foo(int i==1, int j ,int k = 3);       // X
```


## 7.2. Reperence Operator
쉽게 말해서 python에서 사용하는 deep copy라고 생각하면 된다.  
그리고 인자는 포인터 위치에 원래는 주소를 나타내는 문자 ```&```를 사용하면 된다.

```cpp
#include <iostream>

using namespace std;

int main(){
    int x;
    int& foo = x;
    foo = 34;
    cout<< x;
    return 1;
}
```
결과는 무엇이 나올까?
```
34
```
이런식으로 주소를 통한 간접 참조가 가능하다.

<br/>

아래 예시는 reference 참조를 이용한 array 변경에 대한 예시이다.
```cpp
#include <iostream>

using namespace std;


int& max(int a[],int N){
    int  i = 0;

    for(int j= 0; j<N; j++){
        if(a[i]<a[j]) i =j;
    }
    return a[i];
}

int main(void){
    int array[] = {12,42,33,99,63};
    int n = 5; 

    max(array,5)=5;

    for(int i= 0; i<n; i++){
        cout<<array[i]<<" ";
    }
    return 1;
}

```
```
12 42 33 5 63 
```

<br/>

## 7.3. malloc & free vs new &  delete

c++에서는 new 와 delete 문법을 통하여 c에서 진행하던걸 구현한다.   
예시코드
```cpp
#include <iostream>

using namespace std;




int main(void){
    int N =3;

    int*p = new int[N];
    

    for(int i = 0; i<N; i++) p[i] = i+1;
    for(int i = 0; i<N; i++) cout<<p[i]<<" ";

    delete[] p;
    cout<< " "<<endl;

    for(int i = 0; i<N; i++) cout<<p[i]<<" ";

    return 1;
}

```
```
1 2 3  
0 0 0 
```

한개 한개 동적 할당 또한 가능하고 문자열 전체를 할당할 수도 있다.    
참고로 여기서는 initialize 또한 다음과 같이 가능하다.
```cpp
int* p = new int;                       // uninitialized int
int* p = new int(7);                    // 7값으로 초기화

string*ps = new string("hello");        // hello로 초기화

int*p = new int[7];                     // 안됨
int*p = new int[7]();                   // 0으로 초기화됨

int *p = new int[5]{0,1,2,3,4};         // 5개 초기화
sring *ps = new string[2]{"a","hello"}; // 문자열 2개 초기화
```