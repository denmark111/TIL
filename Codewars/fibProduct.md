## Original Kata [Product of consecutive Fib numbers](https://www.codewars.com/kata/5541f58a944b85ce6d00006a)

### Difficulty : 5kyu
### Felt like : Hard    

### Clever Code  
``` C++
#include <vector>
typedef unsigned long long ull;
class ProdFib
{
public:
  static std::vector<ull> productFib(ull prod)
  {
      ull a = 0, b = 1;
      while (a * b < prod) {
          std::swap(a, b);
          b += a;
      }
      return {a, b, ((a*b == prod) ? true : false)};
  }
};
```

위 함수는 인자로 주어진 수를 나타낼 수 있는 2개의 연속된 수를 피보나치 수열에서 추출한다.  
만약 피보나치 수열에서 추출한 두 수의 곱이 인자로 주어진 수와 일치한다면 그대로 반환,  
일치하지 않는다면 그 곱이 인자의 수보다 큰 피보나치 수열의 2개 수를 반환한다.  

#### `static std::vector<ull> productFib(ull prod)`  
위 함수는 `ull` 형의 변수를 인자로 받으며 벡터를 리턴한다.  
`ull` 은 `typedef` 로 지정된 `unsigned long long` 형 이다.  

#### `ull a = 0, b = 1;`  
이 두 변수는 순서대로 피보나치 수열을 탐색하기 위한 변수이다.  

#### `while (a * b < prod)`  
각각 0 과 1로 시작하는 두 변수를 기준으로 그 곱이 주어진 수와 어떤 관계에 있는지 판단한다.  

#### `std::swap(a, b)`  
인자로 주어진 수 보다 그 곱이 작다면 두 변수의 값을 교환한다.  

#### `b += a;`  
실질적으로 피보나치 수열이 계산되는 부분이다.  
EXAMPLE)
a = 0, b = 1 일 때  
1PASS => b = 0, a = 1 => b = 1  
2PASS => b = 1, a = 1 => b = 2  
3PASS => b = 1, a = 2 => b = 3  
4PASS => b = 2, a = 3 => b = 5  
5PASS => b = 3, a = 5 => b = 8  

계속 진행하면 변수 b는 피보나치 수열을 1개씩 전진하며 탐색하게된다.  

#### `return {a, b, ((a*b == prod) ? true : false)}`  
위 탐색 과정에서 찾아낸 a, b 변수값과 삼항연산자를 사용해 두 수의 곱이 주어진 인자와  
일치하는지를 판단한다.  
문제에서 주어진 조건에 따라 일치한다면 `true` 를 아니라면 `false` 를 리턴한다.  

### My Code
``` C++
#include <vector>
#include <cmath>

typedef unsigned long long ull;
class ProdFib
{
public:
  static std::vector<ull> productFib(ull prod);
  static void getFib(int index, int *result);
};

void ProdFib::getFib(int index, int *result)
{
  int tmp;
  
  result[0] = 0;
  result[1] = 1;
  result[2] = 1;
  
  if (index > 3)
  {
    result[0] = 1;
    result[1] = 1;
    result[2] = 2;

    for (int i = 0; i < index - 1; i++)
    {
      tmp = result[1] + result[2];
      result[0] = result[1];
      result[1] = result[2];
      result[2] = tmp;
    }
  }
}

std::vector<ull> ProdFib::productFib(ull prod)
{
  int closest;
  int pre, post;
  int *fibValue = new int[3];
  double phi = (1 + sqrt(5)) / 2;
  std::vector<ull> vul;
  
  vul.clear();
  
  closest = log(sqrt(5 * prod)) / log(phi);
  getFib(closest, fibValue);
  
  pre = fibValue[0] * fibValue[1];
  post = fibValue[1] * fibValue[2];
  
  if (pre == (int)prod)
  {
    vul.push_back(fibValue[0]);
    vul.push_back(fibValue[1]);
    vul.push_back(1ULL);
  }
  else if (post == (int)prod)
  {
    vul.push_back(fibValue[1]);
    vul.push_back(fibValue[2]);
    vul.push_back(1ULL);
  }
  else
  {
    if (pre < (int)prod && post > (int)prod)
    {
      vul.push_back(fibValue[1]);
      vul.push_back(fibValue[2]);
    }
    else
    {
      vul.push_back(fibValue[0]);
      vul.push_back(fibValue[1]);
    }
    vul.push_back(0ULL);
  }
  
  return vul;
}
```