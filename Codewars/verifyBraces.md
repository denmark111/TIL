## Original Kata : [Integers: Recreation One](https://www.codewars.com/kata/valid-braces/solutions/cpp)  

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

이 문제는 임의로 나열된 `(){}[]` 들의 문자열을 입력받고 이 문자열이 유효한지 검사한다.  
모든 열림기호는 닫힘기호와 짝지어져야 한다.  

#### `bool valid_braces(std::string braces)`  
위 함수는 `(){}[]` 이 포함된 문자열을 입력받고 그 문자열이 올바른 형태인지 검사한다.  
그 반환값은 `bool` 형이다.  

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

C++에 스택을 구현하는 라이브러리의 존재를 알지못해서 `vector<int>`를 이용해 스택 비스무리한 구조를 구현했다;;  
~*이런건 그냥 재귀함수로 하면 편한데*~  
*(C++ 폴더에 스택 라이브러리 설명 추가요망)*  
설명의 용이를 위해 아래에서는 스택이라고 지칭.  

코드의 목적은 스택구조를 가진 벡터변수 `order` 에 인자로 주어진 문자열을 보고  
각 열림기호화 닫힘기호가 올바르게 짝지어져 있는지 판단하기 위함이다.  
입력된 문자열을 순서대로 탐색하면서 열림기호가 발견되면 어떤 열림기호인지 판단하고  
스택에 순서대로 저장한다.  

열림기호는 반드시 닫힌기호와 짝지어져야 하므로 검사과정에서 스택의 가장 마지막 원소가  
어떤 기호였는지 검사한 후 조건에 맞다면 스택에서 제거한다.  
만약 이 조건에 부합하지 않는 기호가 있다면 유효하지 않은 문자열이므로 `false`를 리턴한다.  
모든 검사가 완료되고 스택이 비어있다면 `true`를 리턴한다.  