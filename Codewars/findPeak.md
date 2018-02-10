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

### My Code  
``` C++
#include <iostream>
#include <vector>

using namespace std;

PeakData pick_peaks(vector<int> v) {
  PeakData pd;
  // Your code here
  vector<pair<int, int>> sorted;

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