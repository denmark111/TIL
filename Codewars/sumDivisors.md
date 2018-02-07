## Original Kata : [Integers: Recreation One](https://www.codewars.com/kata/55aa075506463dac6600010d/solutions/cpp)  

### Difficulty : 5kyu  
### Felt like : Easy ~ Medium  

### Clever Code  
``` C++
#include <cmath>

using namespace std;

class SumSquaredDivisors
{
public:
    static std::string listSquared(long long m, long long n);
};

string SumSquaredDivisors::listSquared(long long m, long long n)
{
  string ret;
  for (int i = m; i <= n; i++) {
    int sum = 0;
    for (int j = 1; j <= i; j++)
      if (i % j == 0) sum += j * j;
    double sqrt_sum = sqrt(sum);
    if (floor(sqrt_sum) == sqrt_sum)
      ret += "{" + to_string(i) + ", " + to_string(sum) + "}, ";
  };
  return "{" + ret.substr(0, ret.size() - 2) + "}";
}
```

먼저 2개의 정수를 입력받는다.  
이 문제의 목적은 입력받은 2 정수사이의 모든 정수에 대하여 약수를 구한뒤,  
그 약수들을 각각 제곱한다.  
제곱된 모든 약수를 더한 결과의 제곱근이 정수인지 판단한다.  

#### `static std::string listSquared(long long m, long long n)`  
이 함수는 입력으로 두개의 정수를 입력받는다.  
위에서 설명한 것처럼 이 두 정수는 범위를 지정한다.  

#### `string ret`  
결과물을 저장할 문자열 변수이다.  

#### `for (int i = m; i <= n; i++)`  
이 반복문은 인자로 주어진 정수범위를 탐색하기 위해 사용된다.  

#### `for (int j = 1; j <= i; j++)` 
범위내의 수를 모두 탐색하며 각각 나누어보기 위해 사용된다.

#### `if (i % j == 0) sum += j * j`  
범위내의 수가 서로 나누었을 때 그 나머지가 0인지 검사한다.  
0이라면 나누어 떨어졌다는 뜻이므로 그 수는 약수가 된다.  
따라서 문제의 요구사항에 따라 약수의 곱을 누적한다.  
어차피 알아야하는것은 약수제곱의 합이므로 `sum` 변수에 누적하는것이 알맞다.  

#### `double sqrt_sum = sqrt(sum)`  
누적된 결과를 제곱근 함수를 이용해 저장한다.  
이 과정에서 `sqrt()` 함수를 이용하기 위해 `cmath` 라이브러리가 사용되었다.  

#### `if (floor(sqrt_sum) == sqrt_sum)`  
이 구문은 제곱근된 결과와 그 결과에서 소수점을 버린 결과가 같은지 검사한다.  
만약 위 제곱근 과정에서 완벽하게 나누어 떨어지지 않았다면 반드시 소수점이 발생하고,  
소수점 버림과정을 거치면 기존의 값과 다른 값을 가지게 된다.  
이 과정에서 `floor()` 함수를 이용하기 위해 `cmath` 라이브러리가 다시 사용되었다.  

#### `ret += "{" + to_string(i) + ", " + to_string(sum) + "}, "`  
문제에서 요구하는 결과의 형식은 문자열이다.  
위 과정을 통해 정수의 값을 가지는 변수를 문자열에 삽입한다.  
`i` 변수는 인덱스를 의미하며 `sum` 변수는 약수들의 합을 의미한다.  
물론 조건을 만족하는 결과가 하나 이상일 수 있으므로 문자열에 계속 추가한다.  

#### `return "{" + ret.substr(0, ret.size() - 2) + "}"`  
문제의 최종 결과물은 2차원 배열의 형식인데 이를 문자열로 표현하기 위해 사용되었다.  
예를들면 결과는 `{{A, A}, {B, B}, {C, C}}` 의 형태여야 한다.  
하지만 위의 `ret` 변수에 저장하는 방식은 무조건 `{A, A}, ` 의 형식을 따른다.  
즉 최종 결과물의 끝 두자리에는 `, ` 가 채워져 있게된다.  
이를 해결하기위해 `substr` 함수를 사용해 끝 문자 2개를 잘라낸다.  

### My Code  
``` C++
#include <cmath>
#include <utility>
#include <string>

class SumSquaredDivisors
{
public:
    static std::string listSquared(long long m, long long n);
    static long long getDivisorPlus(long long target);
};

long long SumSquaredDivisors::getDivisorPlus(long long target)
{
  long long result = 0;
  int tmp;

  for (long long i = 1; i*i <= target; i++)
  {
    tmp = target / i;
    if (target % i == 0)
    {
      result += i * i;
      if (i != tmp)
      {
        result += (tmp * tmp);
      }
    }
  }

  return result;
}

std::string SumSquaredDivisors::listSquared(long long m, long long n)
{
  long long start, res, sqt;
  bool check = false;
  std::string str;
  
  start = m;
  str.clear();
  
  str.push_back('{');
  while (start <= n)
  {
    res = getDivisorPlus(start);
    
    sqt = sqrt(res);
    if (sqt * sqt == res)
    {
      check = true;
      str.push_back('{');
      str.append(std::to_string(start));
      str.push_back(',');
      str.push_back(' ');
      str.append(std::to_string(res));
      str.push_back('}');
      str.push_back(',');
      str.push_back(' ');
    }
    start++;
  }
  
  if (check)
  {
    str.pop_back();
    str.pop_back();
  }
  str.push_back('}');
  
  return str;
}
```

비교적 쉬운 문제였으나, 마지막 문자열을 반환하는 과정에서 꽤나 어려움을 많이 겪었다.  
위 코드는 총 2개의 함수를 가지고 있다.  
하나는 문제에서 기본으로 제공하는 함수이며 나머지 하나는 직접작성한 함수로써  
대상 정수를 인자로 전달하면 그 인자의 약수를 모두 구한 뒤 각각의 약수를 제곱해  
합친 결과를 리턴한다.  

문제에서 주어진 함수는 비교적 간단하다.  
인자로 주어진 정수사이의 모든 수를 하나씩 `getDivisorPlus()` 의 인자로 넘겨준다.  
결과로 반환되는 수를 제곱근 한 뒤 나머지가 있는지 검사한다.  
만약 그렇다면 문자열화 하고 아니라면 건너뛰고 다음수로 넘어간다.  