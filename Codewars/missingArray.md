## Original Kata : [Length of missing array](https://www.codewars.com/kata/57b6f5aadb5b3d0ae3000611/solutions/cpp/all/best_practice)  

### Difficulty : 6kyu 
### Felt like : Easy ~ Medium  

### Clever Code
``` C++
#include <algorithm>

template<class T>
bool compareBySize(std::vector<T> const &a, std::vector<T> const &b)
{
    return a.size() < b.size();
}

template<class T>
int getLengthOfMissingArray(std::vector<std::vector<T>> arrayOfArrays)
{
    std::sort(arrayOfArrays.begin(), arrayOfArrays.end(), compareBySize<T>);
    
    int missing = arrayOfArrays[0].size();
    for (auto it = arrayOfArrays.begin(); it != arrayOfArrays.end(); it++, missing++)
        if ((*it).size() != missing || missing == 0)
            return missing;

    return 0;
}
```

위 함수는 정렬되지 않은 문자열의 벡터배열을 인자로 받고 어떤 문자열이  
빠져있는지 검사하는 기능을 수행한다.  
예를들어 배열에 {{a, a, a}, {b, b, b, b}, {c}} 의 형식으로 값이 저장되어 있다면,  
이 배열은 2의 길이를 가진 문자열이 빠져있다.  

#### `bool compareBySize(std::vector<T> const &a, std::vector<T> const &b)`  
아래 `std::sort()` 에서 사용할 인자로 전달되는 함수를 정의한다.  
이 함수는 인자로 주어지는 두개의 변수를 비교해 크기에 따라 true / false 값을 반환한다.  
인자로 받을 형식을 정하지 않기위해 `template<class T>` 로 정의된 형이 사용된것을 알 수 있다.  

#### `int getLengthOfMissingArray(std::vector<std::vector<T>> arrayOfArrays)`  
해당 문제에서 주어진 함수이다.  
인자로 원본 벡터배열을 입력받는다.  

#### `std::sort(arrayOfArrays.begin(), arrayOfArrays.end(), compareBySize<T>)`  
배열의 내부 원소를 길이순대로 정렬하기 위해 먼저 `std::sort()` 함수를 호출한다.  
이 함수의 호출을 위해서 `algorithm` 라이브러리가 사용되었다.  
위에서 선언한 크기비교 함수를 이용해 오름차순으로 정렬한다.  

#### ` int missing = arrayOfArrays[0].size()`  
일단 없다고 생각되는 원소를 첫번째 원소라고 가정한다.  

#### `for (auto it = arrayOfArrays.begin(); it != arrayOfArrays.end(); it++, missing++)`  
C++11 에서 도입된 `auto` 형을 사용하여 반복자를 지정한다.  
`auto` 형은 자동으로 변수의 형식을 인식하고 설정한다.  
선언된 반복자 `it` 를 이용해 배열을 탐색한다.  
탐색과정에서 아무런 문제가 없다면 반복자와 없다고 생각하는 원소의 수를 1개 증가시킨다.  

#### `if ((*it).size() != missing || missing == 0)`  
실질적으로 빠져있는 문자열을 검출하는 구문이다.  
이 구문은 만약 현재 반복자가 가리키는 원소가 없다고 생각하는 원소의 인덱스와 일치하지 않거나  
예외사항의 발생으로 없다고 생각하는 원소의 인덱스가 0인경우 함수를 종료하고 인덱스를 반환한다.  
결론적으로 배열을 한 원소씩 탐색하며 빠져있는 원소가 있는지 없는지 검출하는 기능을 수행한다.  

### My Code  
``` C++
template<class TYPE>
int findSmallest (std::vector<std::vector<TYPE>> arr)
{
  int size = arr[0].size();
  for (int i=0; i<arr.size(); i++)
  {
    if (size > arr[i].size())
    {
      size = arr[i].size();
    }
  }
  
  return size;
}

template<class TYPE>
int getLengthOfMissingArray(std::vector<std::vector<TYPE>> arrayOfArrays)
{
  if (arrayOfArrays.empty())
  {
    return 0;
  }
  
  int i;
  int size = arrayOfArrays.size();
  int start_size = findSmallest(arrayOfArrays);
  bool *check = new bool[size + 1];
    
  for (i=0; i<size+1; i++)
  {
    check[i] = false;
  }
  
  for (i=0; i<size; i++)
  {
    if (arrayOfArrays[i].empty())
    {
      return 0;
    }
    check[arrayOfArrays[i].size() - start_size] = true;
  }
  
  for (i=0; i<size+1; i++)
  {
    if (check[i] == false)
    {
      return start_size + i;
    }
  }
  
  delete [] check;
  return 0;
}
```

이 코드의 작동방식은 다음과 같다.  
1. 먼저 어떤 길이의 문자열이 부족한지 체크하기 위한 `bool` 배열을 생성한다.  
2. `bool` 배열의 초깃값은 모두 `false` 이다.  
3. 주어진 배열에서 가장 작은 길이를 가지고 있는 문자열을 찾는다.  
4. 그 문자열을 탐색의 시작으로 지정하고 전체 배열을 하나씩 탐색하여 존재하는 문자열은 위에서 선언된 `bool` 배열에 `true`로 체크한다.  
5. 만약 탐색과정에서 모든 문자열이 순서대로 존재한다면 0을 반환한다.  
6. 아니라면 `bool` 배열을 검사해 어떤 인덱스가 `false` 인지 확인하고 그 인덱스를 반환한다.  

위의 Clever Code에 비해서 My Code 는 정렬을 수행하지 않는다.  
따라서 시작시 가장 작은 길이를 가진 원소를 정해주어야 하는데 이 과정에서 어차피 주어진 배열을 모두 탐색해야 하므로 **탐색과 검출이 동시에 이루어지는 Clever Code 에 비해 비효율적이다.**  