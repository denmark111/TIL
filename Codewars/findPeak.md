## Original Kata : [Pick Peaks](https://www.codewars.com/kata/5279f6fe5ab7f447890006a7/solutions/cpp)  

### Difficulty : 4kyu  
### Felt like : Medium ~ Hard  

### Clever Code  
``` C++
#include <vector>

PeakData pick_peaks(std::vector<int> v)
{
  PeakData result;
  int currentHigh = INT_MIN;
  int currentHighPos = 0;

  for (size_t pos = 0; pos < v.size(); ++pos)
  {
    if (v[pos] > currentHigh)
      currentHighPos = pos;
    else if (v[pos] < currentHigh && currentHighPos > 0)
    {
      result.peaks.push_back(currentHigh);
      result.pos.push_back(currentHighPos);
      currentHighPos = 0;
    }
    currentHigh = v[pos];
  }

  return result;
}
```

이 문제는 주어진 숫자 배열에서 극댓값(Local Maxima) 탐색을 목표로한다.  
숫자 배열의 형식은 정수형 벡터이며 값은 항상 양의 정수이다.  
구해진 극댓값은 문제의 조건에 따라 컨테이너 변수인 `PeakData` 에 담아보내야 한다.  

#### `PeakData pick_peaks(std::vector<int> v)`  
함수의 원형이다.  
인자로 정수형의 벡터를 받아 연산을 진행한다.  

#### `for (size_t pos = 0; pos < v.size(); ++pos)`  
주어진 정수벡터를 탐색할 반복문을 설정한다.  

#### `if (v[pos] > currentHigh)`  
변수 `currentHigh`는 초기에 `INT_MIN`으로 설정되었다.  
따라서 위 구문은 벡터배열 중 순서대로 큰 원소를 찾는 역할을 한다.  
아래 `currentHighPos = pos;` 를 통해 큰 원소를 찾으면 그 인덱스를 저장한다.  

#### `else if (v[pos] < currentHigh && currentHighPos > 0)`  
`currentHigh` 값보다 현재 인덱스의 배열값이 작다는것은 극댓점을 찾았다는 뜻이다.  
또한 그 인덱스는 0보다 커야한다. (이는 문제에서 주어진 조건이다)   

#### `result.peaks.push_back(currentHigh);`  
극댓점을 찾았으므로 그 값을 반환할 컨테이너에 삽입한다.  
아래의 `result.pos.push_back(currentHighPos);` 구문은 인덱스를 삽입한다.  
`currentHighPos = 0;` 의 존재이유는 평지유무를 가려내기 위함으로 보인다.  
예를들어 값이 1, 2, 2, 2, 1 일 경우 2의 연속은 평지를 의미하는데  
이럴때는 문제에서 주어진 조건에 따라 극댓값이 평지의 시작점, 즉 2가 된다.  
이를 가려내기 위해 최고점을 평지에 처음 진입했을때만 적용하고 나머지는 무시한다.  

#### `currentHigh = v[pos];`  
벡터배열의 각 원소를 탐색하는 인덱스라고 보아도 무방하다.  

### My Code  
``` C++
#include <iostream>
#include <vector>

using namespace std;

PeakData pick_peaks(vector<int> v)
{
  PeakData pd;
  int size = v.size();
  int peak, i = 1;
  bool checkFlat = false;
  int possiblePeak, index;

  while (i < size - 1)
  {
    peak = v[i];
    if ((v[i - 1] < peak) && (v[i + 1] < peak))
    {
      pd.peaks.push_back(peak);
      pd.pos.push_back(i);
    }
    else if ((v[i - 1] < peak) && (v[i + 1] == peak) && !checkFlat)
    {
      possiblePeak = peak;
      index = i;
      checkFlat = true;
    }
    else if ((v[i + 1] < peak) && checkFlat)
    {
      pd.peaks.push_back(possiblePeak);
      pd.pos.push_back(index);
      checkFlat = false;
    }
    else if ((v[i + 1] > peak) && checkFlat)
    {
      checkFlat = false;
    }
    i++;
  }
  return pd;
}
``` 

이 코드는 총 3개의 검출방식으로 나누어져 있는데 각 부분은 다음과 같다.    
1. 현재 지정한 벡터배열의 인덱스와 그 이전, 다음 원소를 이용해 극댓값인지 판단한다.  
2. 평지일 경우를 판단하고 그 시작점을 기억한다.  
3. 가라앉은 평지인지 솟은 평지인지 판단한다.  

일반적인 극댓값이라면 찾는 과정은 간단하다.  
배열의 어느 한점을 선택하고 양쪽의 원소와 비교해 크다면 극댓점이다.  
하지만 평지의 경우 2개의 가짓수를 고려해야한다.  
먼저 평지의 형태를 살펴야한다.  

예를들어 평지는 2개의 형태를 가질 수 있다.  
1, 2, 2, 2, 1  
2, 1, 1, 1, 2  
두 형태 모두 중간에 2 의 평지, 1 의 평지를 가지고 있다.  
하지만 여기에는 차이점이 있는데, 2의 평지를 가진 경우 문제의 조건에 따라   
극댓값 2를 가질 수 있다.  
반면에 1의 평지를 가진 경우 극댓값을 가질 수 없다.  
따라서 이를 구분할 수 있는 코드가 필요하다고 생각했다.  