## Original Kata : [Count the Digit](https://www.codewars.com/kata/566fc12495810954b1000030/solutions/cpp)  

### Clever Code
``` C++
namespace CountDig
{
  int nbDig(int n, int d)
  {
    std::string digits;
    for (int k = 0; k <= n; ++k)
      digits += std::to_string(k * k);
  
    return std::count(digits.begin(), digits.end(), std::to_string(d)[0]);
  }
}
```

위 코드는 주어진 인자를 바탕으로 숫자를 생성하고 특정 숫자가 몇번 출현했는지를 측정한다.  

#### `int nbDig(int n, int d)`  
이 함수는 입력으로 인자를 2개 받는다.  
`n`은 생성할 숫자의 갯수이고, `d`는 검출할 숫자를 의미한다.  

#### `std::string digits;`  
생성한 수를 저장할 문자열 변수를 선언한다.  

#### `for (int k = 0; k <= n; ++k)`  
주어진 인자 `n` 만큼 숫자를 생성하기 위해 반복문을 구동한다.  

#### `digits += std::to_string(k * k);`  
선언한 문자열에 정해진 규칙으로 숫자를 추가한다.  
문제에서 주어진 규칙은 제곱수이다.  
`std::to_string()` 함수를 사용해 정수를 문자열로 변환한다.  

#### `return std::count(digits.begin(), digits.end(), std::to_string(d)[0]);`  
`algorithm` 라이브러리의 `std::count()` 함수를 사용해 생성된 전체 문자열에서 `d` 문자를 검색한다.  
