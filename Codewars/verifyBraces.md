## Original Kata [Integers: Recreation One](https://www.codewars.com/kata/55aa075506463dac6600010d/solutions/cpp)  

### Difficulty : 4kyu  
### Felt like : Medium ~ Hard  

### Clever Code  
``` C++
#include <map>
#include <stack>
using namespace std;

bool valid_braces(std::string braces) 
{
  map<char, char> dict = {{'(',')'}, {'[',']'}, {'{','}'}};
  vector<char> v = {'(', '{', '['};
  stack<char> s;
  for (auto c : braces) {
    if (find(v.begin(), v.end(), c) != v.end())
      s.push(dict[c]);
    else if (s.empty() || c != s.top())
      return false;
    else if (c == s.top())
      s.pop();
  }
  return s.empty();
}
```  



#### `bool valid_braces(std::string braces)`  
위 함수는 `(){}[]` 이 포함된 문자열을 입력받고 그 문자열이 올바른 형태인지 검사한다.  

#### `map<char, char> dict = {{'(',')'}, {'[',']'}, {'{','}'}}`

#### `vector<char> v = {'(', '{', '['}`

#### `stack<char> s`

#### `for (auto c : braces)`

#### `if (find(v.begin(), v.end(), c) != v.end())`

#### `s.push(dict[c]);`

#### `else if (s.empty() || c != s.top())`  

#### `return false`  

#### `else if (c == s.top())`  

#### `s.pop()`   

#### `return s.empty()`   

### My Code  
``` C++
#define PARENTHESE  1
#define CURLYBRACE  2
#define BRACKET     3

bool valid_braces(std::string braces) 
{
  int size = braces.size();
  std::vector<int> order;
  
  for (int i = 0; i < size; i++)
  {
    switch (braces[i])
    {
    case '(':
      order.push_back(PARENTHESE);
      break;
    case '{':
      order.push_back(CURLYBRACE);
      break;
    case '[':
      order.push_back(BRACKET);
      break;
    case ')':
      if (!order.empty() && order.back() == PARENTHESE)
      {
        order.pop_back();
        break;
      }
      else
      {
        return false;
      }
    case '}':
      if (!order.empty() && order.back() == CURLYBRACE)
      {
        order.pop_back();
        break;
      }
      else
      {
        return false;
      }
    case ']':
      if (!order.empty() && order.back() == BRACKET)
      {
        order.pop_back();
        break;
      }
      else
      {
        return false;
      }
    }
  }
  
  if (!order.empty())
  {
    return false;
  }
  
  return true;
}
```  