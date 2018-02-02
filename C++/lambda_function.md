# Lambda Function 

### Before using Lambda
- Lambda 식은 C++11 부터 지원하며 C++14부터 인자로 auto를 받을 수 있다.
- 너무 많이 쓰면 가독성이 떨어지니 주의하자.  

### What is Lambda Function 
이름이 없는 함수(익명함수) 를 바로 생성하기 위한 기능이다.  
함수를 미리 정의하지 않더라도 Lambda 를 이용하면 코드 중간에 함수의 생성이 가능하다. 

### Why do we use Lambda Function
- Lambda 를 사용하면 몇 줄의 코드를 캡슐화 할 수 있다. 
- 코드의 간결화, 지연 연산을 통한 성능증대 등의 이점이 있다.
- 명령보단 설명을 통한 코드의 간결화 [Tell, Don't Ask 원칙](https://pragprog.com/articles/tell-dont-ask)

### Where to use Lambda Function
Lambda 는 알고리즘이나 함수에 전달되는 함수를 정의할 때 사용할 수 있다. 

### How to use Lambda Function
Lambda Function 을 선언할때는 선언하는 함수 앞에 **[]** 기호를 붙여 정의한다. 

**[]** 내부에는 *captures* 를 위한 문자가 사용될 수 있다.  
문자의 종류로는 &, =, this 등이 있으며 각 문자의 사용법은 [이곳](http://en.cppreference.com/w/cpp/language/lambda)을 참조.  

아래 예시는 `std::sort()` 에서의 Lambda 사용을 보여준다.  
`std::sort(array.begin(), array.end(), [](std::string const& s1, std::string const& s2) { return s1.size() < s2.size(); });`  
예시에서 `std::sort()` 함수의 3번째 인자로 함수를 넘겨준다. 

### Code simplification w/ Lambda
아래 예시는 C++ 에서의 간단한 for 반목문이다.  
``` C++
    vector<int> v = {0, 1, 2, 3, 4};
    for (int i=0; i<5; i++)
    {
        cout << v[i];
    }
```

위 코드는 for 구문의 매 실행단계마다 i변수의 값을 확인한 후  
탈출조건과 비교한다. 
그 뒤 증가하는 i값에 따라 벡터 v의 원소를 출력한다.
이는 불필요한 i의 연산을 사용되며 코드의 길이도 상대적으로 길다.

이를 Lambda 식으로 변형하면
``` C++
    vector<int> v = {0, 1, 2, 3, 4};
    for_each (begin(v), end(v), [](auto n) {cout << n});
```

위와 같이 코드의 크기와 추가적인 연산과정이 제거된다.

### Referrences
- [나무위키 : 람다식](https://namu.wiki/w/%EB%9E%8C%EB%8B%A4%EC%8B%9D)
- [티스토리 블로그 : 람다함수](http://itguru.tistory.com/196)