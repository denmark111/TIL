# Using list in C++  

C++ 를 사용하면서 데이터를 관리할 때 입력되는 순서를 유지해야 하는 경우가 있다.  
또한 그 데이터를 사용자의 입맛에 맞게 정렬, 분리, 병합 등의 기능이 필요한 경우도 있다.  

이 문서에서는 위의 경우에서 유용하게 사용할 수 있는 STL 의 `Sequence container ` 인 `list` 를 알아본다.     

### How does it works?  
위에서 설명한 바와 같이 리스트 컨테이너는 데이터의 정렬, 분리 등의 작업에 최적화 되어야 한다.  
따라서 `list` 컨테이너는 그 내부 구조로 `Doubly linked list` 를 사용한다.  

리스트에 입력되는 모든 데이터는 노드로 변환되어 저장되며, 컨테이너는  
`sort(), merge()` 등의 멤버함수로 그 노드들을 조작한다.  

`vector, map` 등의 컨테이너와는 다르게 리스트는 특정 원소를 인덱스로 접근 할 수 없다.  
당연히 그 이유는 모든 데이터가 노드로 관리되고 노드는 다음 노드를   
가리키는 주소만을 포함하기 때문이다.  
때문에 리스트를 탐색할 때는 `++, --` 등의 양방향 반복자를 사용하여 각 노드를 탐색해야 한다.  

위 사항을 제외하고 나머지 `push_back(), pop_back(), insert()` 등의 함수는 기본적으로  
사용이 가능하다.  

### How to use it?  
먼저 `list` 를 사용하기 위해서는 라이브러리의 추가가 필요하다.  
그 뒤 사용할 컨테이너 변수를 선언한다.  

``` C++  
#include <list>  

using namespace std;

list<int> lt;
```

위 코드는 정수형 데이터를 각 노드로 갖는 리스트 컨테이너를 선언한다는 뜻이다.  