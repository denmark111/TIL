## Original Kata : [Sorted?yes?no?how?](https://www.codewars.com/kata/580a4734d6df748060000045)  

### Difficulty : 7kyu  
### Felt like : Easy  

### Clever Code
``` C++
    std::string is_sorted_and_how(const std::vector<int>& a)
    {
        return std::is_sorted(std::begin(a), std::end(a)) ? "yes, ascending" :
           std::is_sorted(std::rbegin(a), std::rend(a)) ? "yes, descending" :
           "no";
    }
```

위 코드는 주어진 정수 벡터를 탐색 및 정렬 여부를 판별한다.  
  
#### `std::string is_sorted_and_how(const std::vector<int>& a)`
is_sorted_and_how 함수가 주어진다.  
이 함수는 인자로 정수벡터를 입력받는다.  
리턴값의 형식은 String 이다.  
   
#### `return std::is_sorted(std::begin(a), std::end(a)) ? "yes, ascending" :`
함수의 반환값은 `std::is_sorted`의 결과로 결정된다.  
위에서 사용된 `std::is_sorted` 함수는 인자로 2개의 인덱스를 입력받으며  
입력받은 인덱스를 순차적으로 탐색하며 정렬여부를 검사한다.  

벡터의 인덱스는 `std::begin(`*nameOfVector*`), std::end(`*nameOfVector*`)`로 얻을 수 있다.  

기본 정렬 검색형식은 **오름차순**이며 다른 정렬 방식을 검사하기 위해서  
3번째 인자에 사용자가 지정한 검출 함수를 입력할 수 있다. (e.g Lambda..)  

함수가 정상적으로 실행되고 주어진 배열 또는 벡터가 **오름차순**으로 정렬되어 있을 때  
`std::is_sorted`는 **true** 를 리턴한다.

#### `std::is_sorted(std::rbegin(a), std::rend(a)) ? "yes, descending" :`  
위에서 설명한 `std::is_sorted`와 동일하지만 인덱스의 범위가 변경되었다.  

`std::begin(`*nameOfVector*`), std::end(`*nameOfVector*`)` 와 달리 `std::rbegin(), std::rend()` 는 반대순서의 개념을 가진다.  

따라서 `std::rbegin()` 은 벡터의 끝 인덱스를 시작위치로 지정하며 내림차순 검색을 의미한다.  

#### `:`, `?` 연산자  
각 반환 항목은 삼항연산자를 사용하여 1줄로 축소되었다.  
3항연산자는 `a ? b : c` 의 형식으로 사용된며 **a 가 참이면 b, 아니라면 c 를 반환**  
의 의미를 가지고 있다.  

### My Code  
``` C++
    std::string is_sorted_and_how(std::vector<int> array)
    {
        std::string result;
    
        if (is_sorted(array.begin(), array.end()))
        {
            result = "yes, ascending";
        }
        else if (is_sorted(array.begin(), array.end(), [](int const& v1, int const& v2) {return v1 > v2;}))
        {
            result = "yes, descending";
        }
        else
        {
            result = "no";
        }
    
        return result;
    }
```

먼저 결과를 저장할 문자열을 선언한다.  
그 뒤 `is_sorted()` 함수를 호출해 정렬되어있는지 판단한다.
`is_sorted()` 함수가 기본적으로 오름차순 정렬여부를 검사하기 때문에  
내림차순으로 검사하기 위해서는 사용자지정 함수가 필요하다.  
이를 위해 람다식이 사용되었다.  

이처럼 빌트인 함수를 이용하여 간단하게 해결가능하다.  