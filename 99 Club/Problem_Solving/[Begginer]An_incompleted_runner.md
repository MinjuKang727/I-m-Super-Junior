99클럽 2기 | 자바 | 비기너
# 🚩🏃‍♂️ 해시 | [완주하지 못한 선수](https://school.programmers.co.kr/learn/courses/30/lessons/42576)

## 문제 설명
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

## 제한사항
- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.
  
## 입출력 예
| **participant**                                   | **completion**                           | **return** |
|---------------------------------------------------|------------------------------------------|------------|
| ["leo", "kiki", "eden"]                           | ["eden", "kiki"]                         | "leo"      |
| ["marina", "josipa", "nikola", "vinko", "filipa"] | ["josipa", "filipa", "marina", "nikola"] | "vinko"    |
| ["mislav", "stanko", "mislav", "ana"]             | ["stanko", "ana", "mislav"]              | "mislav"   |

## 입출력 예 설명

**예제 #1**  
"leo"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

**예제 #2**  
"vinko"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

**예제 #3**  
"mislav"는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.

*※ 공지 - 2023년 01월 25일 테스트케이스가 추가되었습니다.*

---

## ✔ Solution with .EntrySet()
```java
import java.util.HashMap;
import java.util.Map.Entry;

class Solution {
    // participant : 참여한 선수 이름 배열
    // completion : 완주한 선수 이름 배열
    // 미완주 선수 1명
    public String solution(String[] participant, String[] completion) {
        
        String answer = "";
        
        HashMap<String, Integer> hm = new HashMap<>();
        int n_participant = participant.length;
        
        for (int i = 0; i < n_participant; i++) {
            String p_runner = participant[i];
            hm.put(p_runner, hm.getOrDefault(p_runner, 0) + 1);
            if (i != n_participant - 1) {
                String c_runner = completion[i];
                hm.put(c_runner, hm.getOrDefault(c_runner, 0) - 1);
            }
            
        }
                
        for (Entry<String, Integer> entry : hm.entrySet()) {
            if(entry.getValue() == 1) return entry.getKey();
        }
        return answer;
    }
}
```

<details>
    <img src="https://github.com/MinjuKang727/I_am_Super_Junior/assets/108849480/7d263e65-6251-4c14-9bd1-32c254c44ae7" alt="채점 결과">
    <summary>채점 결과</summary>

</details>

## ✔ Solution with .keySet()
```java
import java.util.HashMap;

class Solution {
    // participant : 참여한 선수 이름 배열
    // completion : 완주한 선수 이름 배열
    // 미완주 선수 1명
    public String solution(String[] participant, String[] completion) {
        /* 
        풀이
        (참자자 중에는 동명이인이 있을 수 있으므로 HashSet은 적절하지 않다고 판단함.)
        참가 선수명을 키(Key), (해당 이름의 명수 + 1)을 값(Value)으로 HashMap에 추가(업데이트)
        완주 선수명을 키(Key), (해당 이름의 명수 + 1)을 값(Value)으로 HashMap에 추가(업데이트)
        HashMap에 남아있는 선수 명단을 .keySet() 메서드로 가져와 미완주 선수를 반환 
        */
        
        String answer = "";
        
        HashMap<String, Integer> hm = new HashMap<>();
        // 완주 선수 명수
        int n_completion = completion.length;
        // 참가 선수명, 완주 선수명 넣을 문자열 변수
        String p_runner, c_runner;
        
        // 완주 선수 명수는 참가 선수 명수보다 1명 적기 때문에 완주 선수 명수로 반복문 실행
        for (int i = 0; i < n_completion; i++) {
            // 참가 선수명 초기화
            p_runner = participant[i];
            // 완주 선수명 초기화
            c_runner= completion[i];
                     
            // 참가 선수명을 키(Key), (해당 이름의 명수 + 1)을 값(Value)으로 HashMap에 추가(업데이트)
            hm.put(p_runner, hm.getOrDefault(p_runner, 0) + 1);
            // 완주 선수명을 키(Key), (해당 이름의 명수 - 1)을 값(Value)으로 HashMap에 추가(업데이트)
            hm.put(c_runner, hm.getOrDefault(c_runner, 0) - 1);
        }
        
        // 마지막 참가 선수명 초기화
        p_runner = participant[n_completion];
        // 마지막 참가 선수명을 키(Key), (해당 이름의 명수 + 1)을 값(Value)으로 HashMap에 추가(업데이트)
        hm.put(p_runner, hm.getOrDefault(p_runner, 0) + 1);
                
        // HashMap의 keySet으로 for-each문을 돌려서 key에 대한 value값이 1인 key의 값을 구하자.
        // (해당 key = 미완주 선수 이름)
        for (String runner : hm.keySet()) {
            if(hm.get(runner) == 1) {
                answer = runner;
                break;
            }
        }
        return answer;
    }
}
```
<details>
    <img src="https://github.com/MinjuKang727/I_am_Super_Junior/assets/108849480/f52ee06c-5004-4844-a30a-fff1c64dbfd9" alt="채점 결과">
    <summary>채점 결과</summary>

</details>
