## Original Kata : [Correct the time-string](https://www.codewars.com/kata/correct-the-time-string/cpp)  

### Clever Code  
``` C++
    #include <regex>
    #include <string>

    std::string correct(std::string timeString)
    { 
        if (timeString.empty())
            return timeString;

        std::regex reg("(\\d\\d):(\\d\\d):(\\d\\d)");
        std::cmatch cm;

        if (std::regex_match(timeString.c_str(), cm, reg)) 
        {
            int hours = atoi(cm[1].first);
            int minutes = atoi(cm[2].first);
            int seconds = atoi(cm[3].first);

            minutes += seconds / 60;
            hours += minutes / 60;
            seconds %= 60;
            minutes %= 60;
            hours %= 24; 

            char result[9];    
            sprintf(result, "%02d:%02d:%02d", hours, minutes, seconds);
            return std::string(result);
        }
        return "";
    }
```
   
위 코드는 잘못된 시간 형식이 주어졌을 때 올바른 형태로 고치는 기능을 수행한다. 
올바른 기능 수행을 위해 regex 라이브러리가 추가되었다.   

#### `std::string correct(std::string timeString)`  
이 함수는 HH:MM:SS 형식의 시간을 입력받는다.  
시간은 24시간 형식이며 문자열의 형태로 주어진다.  

#### `if (timeString.empty()) return timeString;`  
만약 주어진 인자의 문자열 변수가 비어있다면 그대로 반환 후 종료한다.  

#### `std::regex reg("(\\d\\d):(\\d\\d):(\\d\\d)");`  
입력된 시간의 형식이 올바른지 검사하기 위한 포멧변수이다.  
이 변수의 선언을 위해 `#include<regex>` 가 필요하다.  
포멧 문법은 [이곳](https://en.wikipedia.org/wiki/Regular_expression#Syntax) 에서 확인이 가능하다.  
위 reg에서 사용된 형식은 \(이스케이프)\d(숫자) 를 의미하며 HH:MM:SS 형식을 검사한다.  

#### `std::cmatch cm;`  
여기서 선언된 변수 `cm` 은 cmatch 형식으로서 아래에서 사용될 패턴매칭 함수인  
`std::regex_match()` 에서 도출된 결과를 담기위해 필요하다.  

#### `if (std::regex_match(timeString.c_str(), cm, reg))`  
`std::regex_match()` 함수를 사용해 주어진 포멧 `reg` 의 형식대로 문자열을 분리한다.  
분리된 각각의 결과는 `cm` 객체에 저장된다.  

#### Extraction  
``` C++
    int hours = atoi(cm[1].first);
    int minutes = atoi(cm[2].first);
    int seconds = atoi(cm[3].first);
```
위의 `std::regex_match()` 로 얻어진 결과가 저장된 `cm` 객체를 이용해  
문자열에서 산술연산이 가능한 정수형의 변수로 추출한다.  

#### Calculation  
``` C++
    minutes += seconds / 60;
    hours += minutes / 60;
    seconds %= 60;
    minutes %= 60;
    hours %= 24; 
```
잘못된 시간을 다시 계산하기 위한 수식이 지정되어있다.  
초, 분은 60을 넘기지 않기위해, 시간은 24를 넘기지 않기 위해  
%(모듈러) 연산이 사용되었다.  

#### `char result[9];`  
아래에서 사용될 `sprintf()` 함수는 `char` 형식의 변수를 입력받기 때문에,  
결과를 임시로 저장하기 위해서 사용된다.  

#### `sprintf(result, "%02d:%02d:%02d", hours, minutes, seconds);`
최종적으로 계산된 결과를 재포멧하기 위해 `sprintf()` 함수가 사용되었다.  
`"%02d:%02d:%02d"` 는 포멧을 정하기 위한 인수이며 `02` 는 2자리수로  
패딩할 것을 의미하며 d는 정수임을 의미한다.  

#### `return std::string(result);`  
프로그램 제작시 함수의 리턴값은 `String` 임을 명시하였으므로  
위에서 사용된 `char` 형의 변수를 변환 후 리턴한다.  

### My Code
``` C++
#include <sstream>
#include <regex>
#include <iomanip>

#define T_SEG        3
#define MAX_MINSEC  60
#define MAX_HOUR    24

void split(const std::string& str, std::vector<std::string>& vst, char delim)
{
  std::stringstream ss(str);
  std::string token;
  while (getline(ss, token, delim))
  {
    vst.push_back(token);
  }
}

void fixValue(std::vector<std::string>& vst, std::string& result)
{
  std::ostringstream os;
  int size = vst.size();
  int time[T_SEG];

  for (int i = 0; i < size; i++)
  {
    time[i] = atoi(vst[i].c_str());
  }

  if (time[2] >= MAX_MINSEC)
  {
    time[2] -= MAX_MINSEC;
    time[1] += 1;
  }

  if (time[1] >= MAX_MINSEC)
  {
    time[1] -= MAX_MINSEC;
    time[0] += 1;
  }

  if (time[0] >= MAX_HOUR)
  {
    while (time[0] >= MAX_HOUR)
    {
      time[0] -= MAX_HOUR;
    }
  }

  for (int i = 0; i < size; i++)
  {
    os << std::setw(2) << std::setfill('0') << time[i] << ":";
  }

  result = os.str();
  result.pop_back();
}

std::string correct(std::string timeString)
{ 
    std::string result;
    std::vector<std::string> stv;
    std::regex e("\\d\\d\\:\\d\\d\\:\\d\\d");
    
    if (!regex_match(timeString, e))
    {
      result.clear();
    }
    else
    {
      split(timeString, stv, ':');
      fixValue(stv, result);
    }
    stv.clear();
    
    return result;
}
```