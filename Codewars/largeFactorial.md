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

위 코드는 임의의 정수를 입력받고 그 수의 팩토리얼을 구하는 코드이다.  
팩토리얼의 증가는 매우 빠르므로 정수범위를 초과하는 수를 처리 할 수 있어야 한다.  

#### `string factorial(int factorial)`  
이 함수는 구하려는 팩토리얼을 정수로 받고 문자열을 반환한다.  
문자열을 반환하는 이유는 구해진 팩토리얼값이 정수의 범위를 초과할 수 있기 때문이다.  

#### `if (n < 0) return "";`  
문제에서 주어진대로 입력된 인자의 값이 0보다 작다면 공백을 리턴한다.  
또한 주어진 인자의 값이 1이라면 문자 1을 리턴한다.  

#### `std::vector<int> v = { 2 };`  
입력된 인자가 0 또는 1 일 때는 무조건 반환하므로 연산의 시작은 2부터 진행된다.  

#### `for (int i = 3; i <= n; i++) {`  
연산을 시작할 숫자 2는 이미 벡터배열에 삽입되어 있으므로 반복문의 시작점은 3이 된다.  
입력된 인자인 `n` 까지 반복이 되면 연산이 끝난다.  

#### `int r = 0;`  
연산시 발생되는 올림수를 담기위한 변수이다.  
이러한 변수가 필요한 이유는 구하려는 숫자가 정수범위를 벗어날 가능성이 크기때문이다.  
이러한 조건에서 사용되는 산술 연산은 숫자변수를 직접적으로 사용할 수 없다.  
구체적인 연산방법은 아래에서 설명한다.  

#### `for (int j = 0; j < (int) v.size(); j++)`  
정수변수처럼 사용될 벡터배열을 탐색하는 반복문이다.  
이 반복문 내부에서 모든 연산이 이루어지는데 연산 과정은 다음과 같다.  

1. 먼저 배열의 첫번째 원소와 `i` 변수를 곱한다.  
(변수 `i`는 이번 차례에서 곱해야하는 팩토리얼 수를 가지고 있다.)  

2. 첫번째 원소와의 곱이 한자릿수라면 그대로 원래 위치에 반영해 삽입한다.  
만약 한자릿수를 초과한다면 일의자릿수를 남기고 나머지는 변수 `r` 에 삽입한다.  

3. 벡터배열의 끝까지 위 과정을 반복한다.  

#### `while (r > 0)`  
최종적으로 누적된 올림수, 즉 변수 `r`를 벡터배열에 다시 추가해야한다.  
올림수가 한자릿수을 초과한다면 일의자릿수를 남긴 나머지는 벡터배열의 끝에 새롭게 추가한다.  
일의자릿수는 기존 벡터배열의 마지막 원소에 합한다.  

#### `for (auto i = v.rbegin(); i != v.rend(); i++)`  
연산방식이 위와 같기때문에 벡터배열에는 자릿수가 거꾸로 담겨있다.   
이를 원래수로 되돌리기 위해 `rbegin(), rend()` 와 같은 역순 탐색방법이 사용되었다.  


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

위의 Clever Code와 99.9% 동일한 방식으로 작동한다.  
다만 이 코드는 처음 시작할 때 벡터배열에 1을 가지고 시작한다.  
그 이유는 작성된 코드가 2번째 원소를 기준으로 시작하기 때문이다.  

여기서 사용된 곱셈법에 대해서는 [C++/Handling_bignumbers](https://github.com/denmark111/TIL/blob/master/C%2B%2B/Handling_bignumbers.md) 에 기술되어있다.  