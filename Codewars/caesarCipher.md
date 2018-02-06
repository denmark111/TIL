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