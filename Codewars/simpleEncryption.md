## Original Kata [Basic Encryption](http://www.codewars.com/kata/5862fb364f7ab46270000078/solutions/cpp)

### Difficulty : 6kyu
### Felt like : Easy  

### Clever Code  
``` C++
string encrypt(string text, int rule) {
  std::for_each(text.begin(), text.end(), [&](char &c) {c += rule; });
  return text;
};
```

위 함수는 간단한 암호화 작업을 수행한다.  
먼저 인자로 암호화 할 대상 문자열을 입력받으며 암호화에 사용될 오프셋을 입력받는다.  
암호화의 작동방식은 다음과 같다 :  
암호문 = 문자 + 오프셋  
문자는 `char` 형으로 변환되며 이때 표준 ASCII 문자 코드를 따른다.  
과정이 끝나면 암호화된 문자열을 반환한다.  

#### `string encrypt(string text, int rule)` 
`string` 변수를 반환하는 암호화 함수이다.  
인자로 암호화 대상이 될 문자열과 오프셋을 입력받는다.  

#### `std::for_each(text.begin(), text.end(), [&](char &c) {c += rule; })`  
주어진 문자열의 각 원소를 하나씩 탐색하며 연산하기 위해  
`std::for_each()` 함수가 사용되었다.  
탐색은 주어진 문자열의 시작점과 끝점을 기준으로 한다.  
탐색시 비교할 조건으로 [람다식](https://github.com/denmark111/TIL/blob/master/C%2B%2B/lambda_function.md)이 주어졌다.  
`[&](char &c) {c += rule; }` 이 람다식은 `std::for_each()` 함수가 탐색을 하며  
전달되는 문자 포인터를 `char` 형식의 인자로 받은뒤 오프셋을 더해 암호화한다.  

### My Code  
``` C++
string encrypt(string text, int rule) 
{
  int size = text.size();
  char ch;
  const char *chr = text.c_str();
  string result;
  
  for (int i=0; i<size; i++)
  {
    ch = chr[i] + rule;
    if (ch >= 256)
    {
      ch -= 256;
    }
    result.push_back(ch);
  }
  
  return result;
};
```