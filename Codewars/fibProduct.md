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