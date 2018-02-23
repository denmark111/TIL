### What is STL?  

STL (Standard Template Library) 는 C++를 위한 라이브러리로  
알고리즘, 컨테이너, 함수자, 반복자로 구성되어 있으며  
사용자의 편의성과 코드의 단순성을 위해 다양한 자료형과 배열 등을 제공한다.  

이 라이브러리를 알맞게 활용하면 다양한 자료형을 효율적으로 관리할 수 있으니  
직접 만드는 것보단 상황에 맞는 STL을 활용하자.  

### Types of STL  

STL에는 총 4가지의 종류가 있다
1. Sequence container  
2. Container adaptor  
3. Associative container  
4. Unordered associative container  

위 항목들은 어떠한 방식으로 데이터가 저장되는지에 따른 분류이다.  

먼저 Sequence container 에는 다음과 같은 컨테이너들이 포함된다.  
- array  
- vector  
- deque  
- forward_list  
- list  

이 컨테이너는 입력되는 데이터를 순차적으로 저장하여 관리한다.  
따라서 멤버함수로 `push_back(), pop_back(), at()` 등이 제공된다.  
입력되는 순으로 저장되므로 자동정렬등의 기능은 없다.  

Container adaptor 는 다음과 같은 자료형을 가진다.  
- stack  
- queue  
- priority_queue  

이 자료형은 어떠한 컨테이너들을 규칙에 맞게 관리한다.  
따라서 멤버함수로 `pop(), push(), top()` 등이 제공된다.  
각각의 규칙이 정해져 있기때문에 정렬은 일어나지 않는다.  

Associative container 는 다음과 같은 자료형을 가진다.  
- set  
- multiset  
- map  
- multimap  

이 컨테이너들은 데이터를 자동으로 정렬해 관리한다.  
세부적인 정렬기법은 상이하지만 크게 이진 탐색 트리를 사용해 관리한다.   
`set` 과 `map` 은 키 값과 연결된 데이터를 이용해 저장한다는 점에서 비슷하지만     
`set` 의 경우 직접 키 값의 비교 기준을 정해 줄 수 있다는 점에서 차이가 있다.  
주의할 점은 이 자료형의 경우 값의 삽입과 제거가 발생하면 매번 정렬이 이루어 지므로  
값의 삽입과 제거가 빈번한 경우에 사용하기에는 적합하지 않다.  

Unordered associative containers 의 경우 위의 associative container 와 동일하지만  
자동정렬이 이루어지지 않는다는 점이 다르다.   