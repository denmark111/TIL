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
검사할 문자들을 `map<>` 에 넣는다.  
각 닫힘기호는 열림기호를 key로 가지게 된다.  

#### `vector<char> v = {'(', '{', '['}`
열림기호만을 담고있는 벡터배열을 생성한다.  

#### `stack<char> s`
스택의 역할을 할 변수 s를 선언한다.  
이 자료구조를 위해 `stack` 라이브러리가 사용되었다.  

#### `for (auto c : braces)`  
`auto` 형으로 선언된 변수 c가 인자로 주어진 문자열을 탐색한다.  

#### `if (find(v.begin(), v.end(), c) != v.end())`  
문자열 탐색 과정중 `map` 변수인 dict에 key로 저장되어있는 문자가 발견되면  

#### `s.push(dict[c]);`
그 문자를 key로하는 값을 dict 배열에서 추출해 스택인 s 에 삽입한다.  
예들들어 문자열이 `{}()[]` 이라면  
`{` 문자가 가장 처음 발견되고 이 문자를 key로 하는 `dict`의 값인 `}` 가  
스택변수 `s`에 삽입된다.  
모든 문자열을 탐색하고 나면 스택의 최종 결과는 아래와 같다.  
`s = "}, ), ]"`

#### `else if (s.empty() || c != s.top())`  
스택 `s` 가 비어있다는 것은 문자열에 아무런 열림기호가 존재하지 않는다는 말이며,  
`s`의 가장 위 원소가 반복자인 `c`가 아니라는 것은 열림기호와 닫힘기호가 서로  
짝지어지지 않았다는 뜻이다.  
따라서 이 두개의 경우 중 하나라도 발생하면 바로 `false`를 반환하고 함수를 종료한다.  

#### `else if (c == s.top())`  
만약 제대로 스택의 위 원소가 반복자와 일치한다면 기호가 올바르게 짝지어져 있다는 뜻이다.  
따라서 스택에서 올바르다고 판단된 원소를 제거하고 다음원소를 검사한다.  

#### `return s.empty()`   
모든 과정이 다 끝나면 스택에 남아있는 원소가 있는지 검사한다.  
만약 원소가 남아있다면 짝지어지지 못한 기호가 있다는 뜻이므로 `false`가, 
남아있지 않다면 모든 기호가 짝지어졌다는 뜻이므로 `true` 가 반환된다.  

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

*역시나 코드의 중복이 심하게 일어났다. 젠장*

C++에 스택을 구현하는 ~*고오급*~ 라이브러리의 존재를 알지못해서 `vector<int>`를 이용해 스택 비스무리한 구조를 구현했다;;  
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

중간에 코드작성을 완료하고 업로드를 했더니 `SIGSEGV` 시그널이 발생했다.  
그 이유는 `!order.empty() && order.back() == ....` 에 있었다.  
코드를 작성할 때 원래 `order.back() == ....` 조건만 판단하도록 되어있었다.  
이 때문에 `}}}))]])}` 따위의 열림기호가 없는 입력이 들어오면 입력 문자열은 내용이 있는데,  
열림기호의 순서를 저장하는 `order` 벡터는 완전히 비어있는 상태이다.  
따라서 비어있는 메모리의 참조가 일어나므로 Segmentation fault 인 `SIGSEGV` 이 발생한다.  
이를 해결하기 위해 조건에 추가로 `!order.empty()` 를 삽입해 비어있는지 판단한다.  