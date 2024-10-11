![prograammers](https://github.com/user-attachments/assets/0c8ee936-25ad-482b-a2a9-82ac3abfdfe0)

# JadenCase 문자열 만들기

### 문제 설명
```
JadenCase란 모든 단어의 첫 문자가 대문자이고, 그 외의 알파벳은 소문자인 문자열입니다. 단, 첫 문자가 알파벳이 아닐 때에는 이어지는 알파벳은 소문자로 쓰면 됩니다. (첫 번째 입출력 예 참고)
문자열 s가 주어졌을 때, s를 JadenCase로 바꾼 문자열을 리턴하는 함수, solution을 완성해주세요.
```

### 제한사항
- s는 길이 1 이상 200 이하인 문자열입니다.
- s는 알파벳과 숫자, 공백문자(" ")로 이루어져 있습니다.
    - 숫자는 단어의 첫 문자로만 나옵니다.
    - 숫자로만 이루어진 단어는 없습니다.
    - 공백문자가 연속해서 나올 수 있습니다.
## 문제 의도
- 문자열 비교
- 문자 index 찾기 

## 정답
```
class Solution {
    public String solution(String s) {
        String[] str = s.split("");
        String answer = str[0].toUpperCase();
        int idx = 0;
        for(int k = 1; k < str.length; k++){
            if(str[k].equals(" ")) idx = k+1;
            
            if(idx == k) answer += str[k].toUpperCase();
            else answer += str[k].toLowerCase();
        }
        return answer;
    }
}
```
- 문자열을 배열로 변환
- 배열에 공백(" ")이면 다음 index를 저장 
- 그후 다음 인덱스는 대문자, 아니면 소문자로 저장

---
[문제 출처](https://school.programmers.co.kr/learn/courses/30/lessons/12951?language=java)