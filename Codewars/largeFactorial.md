## Original Kata : [Large Factorials](https://www.codewars.com/kata/large-factorials/solutions/cpp)  

### Difficulty : 4kyu  
### Felt like : Medium   

### Clever Code
``` C++
#include<string>
using namespace std;
string factorial(int factorial){
  int n = factorial;
  if (n < 0) return "";
  if (n <= 1)  return "1";
  std::vector<int> v = { 2 };
  for (int i = 3; i <= n; i++) {
    int r = 0; //remainder
    for (int j = 0; j < (int) v.size(); j++) {
      v[j] *= i;
      v[j] += r;
      r = v[j] / 10;
      v[j] %= 10;
    }
    while (r > 0) {
      v.push_back(r % 10);
      r /= 10;
    }
  }
  string result;
  for (auto i = v.rbegin(); i != v.rend(); i++) {
    result += '0' + (*i);
  }
  return result;
}
```

### My Code  
``` C++
#include <vector>

using namespace std;

string factorial(int factorial)
{
  string result;
  vector<int> tmp;
  int carry = 0, prod = 0;
  int size;
  
  if (factorial < 1)
  {
    return "";
  }
  
  tmp.clear();
  tmp.push_back(1);

  for (int i = 2; i <= factorial; i++)
  {
    size = tmp.size();
    for (int j = 0; j < size; j++)
    {
      prod = tmp[j] * i + carry;
      tmp[j] = prod % 10;
      carry = prod / 10;
    }

    while (carry)
    {
      tmp.push_back(carry % 10);
      carry /= 10;
    }
  }

  for (int i = tmp.size() - 1; i >= 0; i--)
  {
    result.append(to_string(tmp[i]));
  }
  
  return result;
}
```