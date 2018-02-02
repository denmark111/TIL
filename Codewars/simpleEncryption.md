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