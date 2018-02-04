## Original Kata [Integers: Recreation One](https://www.codewars.com/kata/55aa075506463dac6600010d/solutions/cpp)  

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

####

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