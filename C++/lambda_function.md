# Lambda Function 

## What is Lambda Function 
이름이 없는 함수를 바로 생성하기 위한 기능이다.
함수를 미리 정의하지 않더라도 Lambda 를 이용하면 코드 중간에 함수의 생성이 가능하다. 

## Why do we use Lambda Function
Lambda 를 사용하면 몇 줄의 코드를 캡슐화 할 수 있다. 

## Where to use Lambda Function
Lambda 는 알고리즘이나 함수에 전달되는 함수를 정의할 때 사용할 수 있다. 

## How to use Lambda Function
아래 예시는 `std::sort()` 에서의 Lambda 사용을 보여준다. 
    (std::sort(array.begin(), array.end(), [](std::string const& s1, std::string const& s2) { return s1.size() < s2.size(); });)