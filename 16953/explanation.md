## 문제
![](https://velog.velcdn.com/images/keumsiun0503/post/3de398ba-ab25-478d-991d-306470879298/image.png)
https://www.acmicpc.net/problem/16953

### 문제 설명 
>
1. 정수 A를 다음 두 가지 연산 중 하나씩 고르며 B로 바꾸려고 한다.
2. (2를 곱하기) 또는 (1을 수의 가장 오른쪽에 추가하기).
3. 정수 A와 B가 입력으로 주어졌을 때, A를 B로 바꾸는데 필요한 연산의 최솟값 구하기
- 조건 1: A를 B로 바꾸는데 필요한 연산의 최솟값에 1을 더한 값을 출력
- 조건 2: A를 B로 바꿀 수 없다면 -1을 출력

📖테스트 케이스 살펴보기
![](https://velog.velcdn.com/images/keumsiun0503/post/d6624b51-31a8-420f-abe3-69277f5023cc/image.png)
(친절하게 바꾸는 과정도 적혀있다)

- 예제 입력 1을 예시로 살펴보자면,
2에서 두가지 연산 중 하나씩 고르며 162를 만들어야 한다.
2x2 = 4
4x2 = 8
8에 1 추가 = 81
81x2 = 162
(최소 연산의 수) 4 + 1 = 5
와 같은 방식으로 문제를 해결할 수 있다.

그렇다면 A가 주어졌을 때 두가지 연산 중 무엇을 고를지 어떻게 정할까?
🤔떠오른 방법은 **역 연산**이다.
B의 값에 2를 나누거나, 끝자리에서 1을 빼면서 A로 바꾸는 연산을 진행하는 것이다.
역 연산을 하는 데에 세 가지 조건이 부여된다.

💡그리디
그리디는 각 단계마다 **지역 최적해**를 찾는 방식.
각 단계마다 최대한 욕심내어 취할 수 있는 만큼 값을 취하고,
각 단계의 해를 모아 **최적 해의 근사 값**을 구함. 

🔥역 연산 과정에서, 세 가지 조건 중 만족하는 경우의 연산을 취한다.
>
1. 만약 B가 홀수이면서, 끝자리가 1이 아니면 연산을 종료한다.
(A는 정수이기 때문에 홀수를 2로 나누는 경우에 A를 절대 만들 수 없음)
2. 만약 B의 끝자리가 1이라면, B의 끝자리에서 1을 뺀다.
3. 만약 B가 짝수라면 B를 2로 나눈다

이런 조건을 B가 A보다 작거나 같아질 때까지 반복하여 거친다.
반복이 끝나고 
- 만약 B==A라면 연산 횟수 +1을 출력
- 그렇지 않다면 -1을 출력

### 문제 풀이 설계
>
1. **정수** A, B값 입력 받기
2. 연산 횟수를 셀 **count**변수 정의
3. B>A 라면 다음 **세 조건**을 반복하여 해당하는 조건의 연산을 취해줌
3-1. 만약 B가 홀수인데, 끝자리가 1이 아니면 연산 종료
3-2. 만약 B의 끝자리가 1이라면, B의 끝자리에서 1을 뺀다(B/=10) count++
3-3. 만약 B가 짝수라면, B를 2로 나눈다(B/=2) count++
4. B와 A가 같아졌다면 count + 1 출력, 그렇지 않다면 -1 출력
**int result = B==A ? count + 1 : -1**

## 문제 풀이
```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        StringTokenizer st = new StringTokenizer(br.readLine());
        //1. 정수 A, B 값
        int A = Integer.parseInt(st.nextToken());
        int B = Integer.parseInt(st.nextToken());
        //2. 연산 횟수 count
        int count = 0;
        //3. B>A라면 세 조건 반복
        while(B>A){
            //3-1. B가 홀수인데 끝자리가 1이 아닐 때
            if(B%10!=1 && B%2!=0){
                break;
            }
            //3-2. B의 끝자리가 1일 때
            if(B%10==1){
                B/=10;
            }
            //3-3. B가 짝수일 때
            else {
                B/=2;
            }
            count++;
        }
        //4. B가 A와 같아졌는지 여부에 따라 result값 출력
        int result = B==A ? count + 1 : -1;
        System.out.println(result);
    }
}
```
![](https://velog.velcdn.com/images/keumsiun0503/post/b31bef0c-7655-4da9-b5cb-5f0a157148cc/image.png)

### 느낀점
>
"A와 B가 정수라면 B가 홀수일 때 2로 나누어 A를 만들 수 없다"는 생각을 하지 못 해서 첫번째 조건을 빼먹고 문제를 풀다가 당황했던 문제다. 
해당 문제는 그리디를 적용한 역 연산이 아니라 탐색으로도 풀 수 있는데, 
A에서 연산 두 가지 모두를 택하고, 또 해당 결과값에서 연산 두 가지 모두를 택하는 너비 우선 탐색 방식으로도 풀 수 있다.
**단 역 연산이 아닌 경우에는 x2 또는 x10 과정에서 정수 자료형의 범위를 넘어갈 수 있으므로**
범위를 벗어날 수 있는 값들은 Long타입으로 선언하는 것이 중요하다.

#### 다른 풀이 방식

```java
import java.io.*;
import java.util.*;

public class Main2 {
    static long A, B;
    static int min = Integer.MAX_VALUE;
    static void BFS(long A, long B, int count){
        if(A>=B){
            min = A==B ? Math.min(min, count+1) : min;
            return;
        }
        BFS(A*2, B, count+1);
        BFS(A*10 + 1, B, count+1);
    }
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        StringTokenizer st = new StringTokenizer(br.readLine());

        A = Long.parseLong(st.nextToken());
        B = Long.parseLong(st.nextToken());
        BFS(A, B, 0);
        int result = min == Integer.MAX_VALUE ? -1 : min;
        System.out.println(result);
    }
}
```