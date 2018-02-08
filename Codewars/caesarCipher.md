## Original Kata : [First Variation on Caesar Cipher](https://www.codewars.com/kata/5508249a98b3234f420000fb/solutions/cpp)

### Difficulty : 5kyu  
### Felt like : Hard  

### Clever Code  
``` C++
class CaesarCipher
{
  static char shift_char(char c, int shift)
  {
    if (c >= 'a' && c <= 'z') 
      return 'a' + (c - 'a' + shift) % 26;
    if (c >= 'A' && c <= 'Z')
      return 'A' + (c - 'A' + shift) % 26;
    return c;
  }
public:
  static std::vector<std::string> movingShift(const std::string &s, int shift)
  {    
    std::vector<std::string> ret(5, "");
    int fifth = (s.size() + 4) / 5;
    for (int i = 0; i != s.size(); ++i, shift = (shift + 1) % 26)
      ret[i/fifth] += shift_char(s[i], shift);
    
    return ret;
  }
  static std::string demovingShift(const std::vector<std::string> &s, int shift)
  {
    
    int size = s[0].size() * 4 + s[4].size(), fifth = s[0].size();
    std::string ret(size, ' ');
    for (int i = 0; i != size; ++i, shift = (shift + 1) % 26)
      ret[i] = shift_char(s[i/fifth][i%fifth], (26-shift) % 26);
    return ret;
  }
};
```

이 함수는 간단한 `Caesar Cipher` 를 구현한다.  
작동방식은 다음과 같다.  
1. 문자열을 입력받는다.  
문자는 숫자, 특수문자, 알파벳, 띄어쓰기를 포함한다.  

2. 입력받은 문자열을 주어진 `shift`를 기준으로 1씩 증가시키며 더한다.  
예를들어 `abc`의 문자열이 주어지고 `shift` 가 1이라면 문자 순서대로 1, 2, 3이 더해진다.  
문자열의 각 문자는 표준 아스키코드로 변환하여 계산된다.  
즉 `abc`의 암호문은 `bdf`가 된다.  

3. 위 과정을 반대로 구현하여 암호문을 복호화한다.  

주의해야할 점은 `Caesar Cipher` 는 알파벳만을 암호화한다는 점이다.  
특수문자, 띄어쓰기, 숫자등은 암호화되지 않는다.  
만약 암호화 과정중 알파벳의 범위를 벗어난다면 다시 처음을 기준으로 시작한다.  
예를들어 문자 `z` 는 아스키코드로 변환시 112의 정수값을 가지게 되는데  
이때 주어진 `shift` 가 0보다 크다면 알파벳표현범위를 벗어나게 된다.  
따라서 이럴때에는 다시 알파벳 `a`로 돌아가 `shift` 를 센다.  

#### `static char shift_char(char c, int shift)`  
이 함수는 인자로 주어진 문자를 시프팅하는 과정을 수행한다.  
인자로 변환할 문자와 시프팅할 수가 주어진다.  

#### `if (c >= 'a' && c <= 'z') `  
인자로 주어진 문자가 알파벳 소문자인지 대문자인지 검사한다.  
이는 `Caesar Cipher` 의 특성을 만족하기 위해 필요한 검사문이다.  

#### `return 'a' + (c - 'a' + shift) % 26;`  
만약 위 조건문이 충족된다면 시프팅을 수행한다.  
`a` 를 더하는 이유는 시작 오프셋을 정해주기 위해서이다.  
또한 모듈러연산을 사용해 지정된 범위만 리턴되도록 설계되었다.  
이후의 구문도 위와 같은 연산을 수행하지만 문자만 대문자로 변경되었다.  

#### `static std::vector<std::string> movingShift(const std::string &s, int shift)`  
문자를 암호화하는 함수이다.  
문제에서 주어지는 함수로써 인자로 암호화할 문자열과 시프팅 수가 주어진다.  

#### `std::vector<std::string> ret(5, "");`  
문제에서 요구하는 리턴조건을 충족하기 위한 변수이다.  
요구되는 리턴값은 배열인데, 이 배열은 총 5개의 원소를 반드시 가져야한다.  
이 5개의 원소는 각각 문자열이며 조건은 다음과 같다.  

1. 암호화된 문자열은 총 5부분으로 나누어진다.  

#### ``

#### ``

#### ``

#### ``

#### ``

#### ``

#### ``

### My Code  
``` C++
#include <cmath>
#include <map>

#define TOTAL_ALPHA 26
#define TOTAL_PARTS 5

#define UPPERCASE_START 65
#define LOWERCASE_START 97

using namespace std;

class CaesarCipher
{
  public:
    static vector<string> movingShift(const string &s, int shift);
    static string demovingShift(vector<string> &s, int shift);
};

vector<string> CaesarCipher::movingShift(const string &s, int shift)
{
  int cnt = 0, pt = 0, elementPerParts, str;
  int shiftCount = shift;
  
  vector<string> vt(TOTAL_PARTS);
  map<int, string> ref;
  string tmp;
  
  for (int i = 0; i < TOTAL_ALPHA; i++)
  {
    ref[LOWERCASE_START + i] = 'a' + i;
    ref[UPPERCASE_START + i] = 'A' + i;
  }
  
  if (s.size() < TOTAL_PARTS)
  {
    elementPerParts = s.size();
  }
  else
  {
    elementPerParts = ceil((double)(s.size()) / TOTAL_PARTS);
  }
  
  for (int i = 0; i < TOTAL_PARTS; i++)
  {
    vt[i].clear();
    for (int j = pt; j < pt + elementPerParts; j++)
    {
      if (isalpha(s[j]))
      {
        str = s[j] + shiftCount;
      }
      else
      {
        str = s[j];
      }
      
      if ((islower(s[j]) && !islower(str)) ||
        (isupper(s[j]) && !isupper(str)))
      {
        str -= TOTAL_ALPHA;
      }

      if (cnt == s.size())
      {
        return vt;
      }
      else
      {
        if (ref.find(str) != ref.end())
        {
          tmp = ref[str];
        }
        else
        {
          tmp = s[j];
        }
        vt[i].append(tmp);
      }
      cnt++;
      if (shiftCount == TOTAL_ALPHA)
      {
        shiftCount = 1;
      }
      else
      {
        shiftCount++;
      }
    }
    pt += elementPerParts;
  }

  return vt;
}

string CaesarCipher::demovingShift(vector<string> &s, int shift)
{
  int str = 0;
  int size = s.size();
  int shiftCount = shift;
  string result;

  for (int i = 0; i < size; i++)
  {
    if (s.empty())
    {
      result.clear();
      break;
    }
    for (int j = 0; j < s[i].size(); j++)
    {
      if (isalpha(s[i][j]))
      {
        str = s[i][j] - shiftCount;
        if ((islower(s[i][j]) && !islower(str)) ||
          (isupper(s[i][j]) && !isupper(str)))
        {
          str += TOTAL_ALPHA;
        }
        result.push_back(str);
      }
      else
      {
        result.push_back(s[i][j]);
      }
      
      if (shiftCount == TOTAL_ALPHA)
      {
        shiftCount = 1;
      }
      else
      {
        shiftCount++;
      }
    }
  }

  return result;
}
```