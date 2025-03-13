## 문제
![](https://velog.velcdn.com/images/keumsiun0503/post/229ae46e-cec3-4a0b-a9f1-d7c71413a2e5/image.png)
https://www.acmicpc.net/problem/2885

### 문제 설명
>
1. 막대 모양 초콜릿이 있고, 초콜릿의 크기(정사각형의 개수)는 항상 2의 제곱 형태이다.(1, 2, 4, 8, ...)
2. 초콜릿을 적어도 K개의 정사각형을 먹으려고 한다.
3. 초콜릿을 먹으려면 초콜릿을 쪼개야 하는데, 막대 초콜릿은 항상 가운데로만 쪼개진다. 즉, 정사각형이 D개 있는 막대는 D/2개 막대 두 조각으로 쪼개진다.
4. K개 정사각형을 만들기 위해서, 사야하는 가장 작은 초콜릿의 크기와, 최소 몇 번 초콜릿을 쪼개야 하는지 출력
- 조건 1: 초콜릿을 하나만 살 수 있다
- 조건 2: 꼭 한 조각이 K개일 필요는 없고, 여러 조각에 있는 정사각형의 합이 K개면 된다.

![](https://velog.velcdn.com/images/keumsiun0503/post/cdb5cb47-805d-45f8-8af4-3b6c660165e3/image.png)
📖테스트 케이스
예제 입력 1을 보면, K=6, 출력은 8 2이다.
6개의 초콜릿 조각을 먹기위해서는 최소한 8크기의 막대 초콜릿이 필요하며, 두 번 쪼개면 된다는 의미다.
1. 8/2 = 4(두 조각)
2. 4/2 = 2(두 조각)
두 번 쪼개면 나오는 조각은 4, 2, 2이다. 따라서 6개의 초콜릿 조각을 먹을 수 있게 된다.

💡그리디
그리디는 각 단계마다 **지역 최적해**를 찾는 방식.
각 단계마다 최대한 **욕심내어** 취할 수 있는 만큼 값을 취하고,
각 단계의 해를 모아 **최적 해의 근사 값**을 구함.

- **필요한 최소 조각**
K개의 조각을 먹으려고 할 때 반드시 필요로 하는 최소 조각이 있다.
1. 6개의 조각을 먹으려고 할 때 2개짜리 조각이 필요하다.
2. 5개의 조각을 먹으려고 할 때 1개짜리 조각이 필요하다.
3. 10개의 조각을 먹으려고 할 때(초콜릿 크기는 16) 2개짜리 조각이 필요하다.

🤔그렇다면 필요한 최소 조각을 어떻게 알고 그 조각까지 쪼개면 될까?
1. 우선 K개의 초콜릿 조각을 먹기 위해서 **필요한 초콜릿 크기**는
(초콜릿 크기 > K) 가 될 때까지 초콜릿 크기 = 1 에 2를 곱하면 된다.
2. K가 0이 될 때 까지 **초콜릿을 쪼개면서 나온 값을 K에서 빼준다**
3. 만약 K에서 **초콜릿을 쪼갠 값을 뺄 수 있다면** 빼준다
4. 그렇지 않다면 **초콜릿을 쪼갠다**,  cut++;

이런 방식대로 푼다면, 필요한 최소 조각까지 초콜릿을 쪼갤 수 있다.

### 문제 풀이 설계
>
1. 먹으려는 조각의 수 **K**값 입력 받기
2. 필요한 초콜릿 크기 **size**값 구하기
3. 쪼개는 횟수를 저장할 변수 **cut**, size로부터 쪼개진 조각의 크기를 담을 변수 **piece**
4. K가 0이 될 때까지 piece를 쪼개어 빼준다.
4-1. 만약 K>=piece라면, K-piece
4-1. 그렇지 않다면, piece/2, cut++
5. size와 cut을 출력

## 문제 풀이
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        //1. K값 입력 받기
        int K = Integer.parseInt(br.readLine());
        //2. size값 구하기
        int size = 1;
        while(size<K) size *=2;
        //3, 쪼개는 횟수 cut, 쪼개진 조각 piece
        int cut = 0;
        int piece = size;
        //4. K가 0이 될 때까지 piece를 쪼개어 빼준다.
        while(K>0){
            if(K>=piece){
                K-=piece;
            } else {
                piece/=2;
                cut++;
            }
        }
        //5. size, cut 출력
        System.out.println(size + " " + cut);
    }
}
```
![](https://velog.velcdn.com/images/keumsiun0503/post/3b082887-105c-495f-9bfc-353b40262eb8/image.png)
### 느낀점
>
쉬워 보였는데 생각보다 머리가 안 돌아가서 당황했다. 그리디 문제들의 특징은 문제를 읽었을 때는 쉬워보이는 것이다. 하지만 그리디는 수학적 사고 능력을 필요로 하므로 다양한 그리디 문제를 풀면서 수학적 사고를 기르는 것이 중요할 것 같다.