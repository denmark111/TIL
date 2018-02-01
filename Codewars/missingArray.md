## Original Kata [Length of missing array](https://www.codewars.com/kata/57b6f5aadb5b3d0ae3000611/solutions/cpp/all/best_practice)  

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

#### ``

#### ``

#### ``

#### ``

#### ``

#### ``

#### ``

#### ``

#### ``

#### ``

#### ``






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