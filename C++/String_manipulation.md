# String Manipulation  

## Purpose of document  
이 문서는 종종 사용되는 C++ 의 다양한 문자열 연산 및 조작을 저장하여  
필요한 경우 참고를 목적으로 한다.

### Basic member functions of String class
1. `.assign()`  
대상 스트링 변수에 값을 설정한다.
파라미터 1 개 : 문자열
파라미터 2 개 : 갯수, 문자
파라미터 3 개 : 문자열, 시작위치, 갯수
함수의 인자는 1개 ~ 3개 까지 가능하며 기능에 따라 적합한 형태를 사용한다.  

2. `.append()`  
**+** 기호와 같은 기능을 수행한다.  
위 함수는 대상 스트링 변수에 문자를 추가한다.  
추가시에는 변수의 뒤에 문자를 삽입한다.  

3. `clear()`  
대상 스트링 변수의 모든 내용을 제거한다.  
제거 후 값은 NULL이 아닌 비어있는 값이 된다.  

4. `.compare()`  
대상 문자열과 인자로 주어지는 문자열을 비교한다.  
리턴 값은 두 문자열이 같을 때 0, 대상 문자열이 더 길 때 양수를 반환한다.  

5. `.erase()`  
이 함수는 인자로 시작위치와 갯수를 입력받는다.  
시작위치로 부터 지정된 갯수만큼 대상 변수에서 문자를 지운다.  

6. `.find()`  
대상 스트링 변수에서 인자에 따른 문자열을 검색한다.
파라미터 1 개 : 문자 또는 문자열
파라미터 2 개 : 문자열, 시작위치

7. `.replace()`  
특정 문자열을 대체한다. 
함수의 인자로는 시작위치, 갯수, 문자열 을 입력받는다. 

8. `.insert()`  
인자로 주어진 문자열을 특정 위치에 삽입한다. 
함수의 인자로는 시작위치, 문자열 을 입력받는다. 

9. `.pop_back()` & `.push_back()`  
각각 주어진 스트링 변수에서 1개씩 문자를 제거하거나 추가한다. 
모든 연산은 문자열의 가장 마지막에서 일어난다. 

10. `.size()` & `.length()`  
대상 스트링 변수의 문자열 길이를 반환한다. 
스트링 변수의 최대 저장 길이를 알기 위해서는 `.max_size()` 를 사용할 수 있다. 

11. `.capacity()`  
대상 변수에 저장될 수 있는 최대 문자의 갯수를 반환한다.  
문자열이 반환된 갯수보다 많더라도 자동으로 메모리를 할당받는다.  

12. `.c_str()`  
스트링 변수의 시작 인덱스를 char* 형으로 반환한다.  
문자열을 정수형 으로 변환할 때 사용된다. 

### Some examples of efficient programming
잘못된 시간이 주어졌을 때 올바른 포멧으로 변경/계산 후 리턴하는 코드이다. 
    #include <regex>
    #include <string>

    using namespace std;

    string correct(string timeString)
    { 
        if (timeString.empty())
        {
            return timeString;
        }
        regex reg("(\\d\\d):(\\d\\d):(\\d\\d)");
        cmatch cm;

        if (regex_match(timeString.c_str(), cm, reg)) 
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
            return string(result);
        }
        return "";
    }

