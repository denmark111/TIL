## Original Kata : [Speed Control]
(https://www.codewars.com/kata/speed-control/solutions/cpp)

### Clever Code
``` C++
class GpsSpeed
{
public:
  static int gps(int s, std::vector<double> &x) 
  {
      if(x.size() < 1)
      {
        return 0;
      }
      
      std::vector<double> bar(x.size());
      std::transform(x.begin()+1, x.end(), x.begin(), bar.begin(), std::minus<double>());
      
      return (3600 * (*max_element(bar.begin(), bar.end())) / s);
  }

};
```

위 코드는 단위시간당 이동거리가 주어졌을 때  
구간별 가장 빠른 속력을 계산한다.

### `static int gps(int s, std::vector<double> &x)`
위 함수는 GpsSpeed 클래스의 멤버함수로써 기록된 시간(초) 와  
이동한 거리가 기록된 실수벡터를 인수로 전달받는다.  

### `if(x.size() < 1)`
이 조건문은 만약 기록된 이동거리가 없을 경우 0을 반환함을  
나타내는 구문이다.  

### `std::vector<double> bar(x.size());`
이 벡터는 아래에 사용된 `std::transform()` 함수의 결과물  
저장을 위해 사용될 벡터이며 인자로 주어지는 `x` 와 같은 크기를 갖는다. 

### `std::transform(x.begin()+1, x.end(), x.begin(), bar.begin(), std::minus<double>());`
이 함수는 `vector` 라이브러리에 포함된 함수로써  
특정 연산을 통해 출력되는 결과를 인자로 주어진 또다른 벡터에 저장한다.  

- `x.begin()+1, x.end()`  
벡터변수 `x`의 2번째 원소를 가리킨다.  
벡터변수의 마지막 원소를 가리키는 인자또한 주어진다.    

- `x.begin()`
`std::transform()` 함수에서 사용될 연산의 피연산자가 시작되는 위치를 지정한다.  
이 문제에서는 같은 벡터변수의 원소를 연산하므로 1번째 원소가 피연산자의 시작점이 된다.  

- `bar.begin()`
연산의 결과가 저장될 벡터변수의 시작점을 지정한다.  

- `std::minus<double>()`
수행될 연산의 종류를 지정한다.  
이 문제의 경우 뺄샘이 사용되었다.  
이 인자는 함수를 받을 수 있으며 람다식도 사용이 가능하다!!  

### `return (3600 * (*max_element(bar.begin(), bar.end())) / s);`
이 함수의 결과물을 반환한다.  
그 과정을 위해 `max_element()` 함수가 사용되었다.  
이 함수는 `std::transform()` 함수의 결과가 저장된 벡터변수 `bar` 에서 최댓값을 반환한다.  
반환된 최댓값을 이용해 문제에서 주어진 공식에 대입해 최종 결과를 얻는다.  
*함수이름 앞에 왜 `*` 이 붙을까??*
