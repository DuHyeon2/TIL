![prograammers](https://github.com/user-attachments/assets/0c8ee936-25ad-482b-a2a9-82ac3abfdfe0)

# 문자열 내 p와 y의 개수

### 문제 설명
```
대문자와 소문자가 섞여있는 문자열 s가 주어집니다. s에 'p'의 개수와 'y'의 개수를 비교해 같으면 True, 다르면 False를 return 하는 solution를 완성하세요. 'p', 'y' 모두 하나도 없는 경우는 항상 True를 리턴합니다. 
단, 개수를 비교할 때 대문자와 소문자는 구별하지 않습니다.

예를 들어 s가 "pPoooyY"면 true를 return하고 "Pyy"라면 false를 return합니다.
```

### 제한사항
- 문자열 s의 길이 : 50 이하의 자연수
- 문자열 s는 알파벳으로만 이루어져 있습니다.

## 문제 의도
- 문자열을 찾고 비교하는 알고리즘을 요구

## 정답
```
class Solution {
    boolean solution(String s) {
        return s.chars().filter(c -> c == 'p' || c == 'P').count() == s.chars().filter(c -> c == 'y' || c == 'Y').count();
    }
}
```
- 문자열에서 P와Y를 찾아서 count 한 후 비교값을 Return 하는 방식을 사용
- filter 를 사용하였다.

---
[문제 출처](https://school.programmers.co.kr/learn/courses/30/lessons/12916?language=java)