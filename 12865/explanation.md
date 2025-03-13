## 문제
![](https://velog.velcdn.com/images/keumsiun0503/post/1db51b4a-d636-442b-9882-a34531c08104/image.png)
https://www.acmicpc.net/problem/12865
### 문제 설명
>
1. 여행에 들고 갈 배낭과, 여행에 필요한 N개의 물건들이 있다.
2. 각 물건은 무게 W와 가치 V를 가진다.
3. 배낭에는 최대 K만큼의 무게를 넣을 수 있는데, 이때 배낭 무게를 넘지 않으면서 최대 가치를 들고 여행을 가려고 한다.
4. 배낭에 넣을 수 있는 물건들의 가치합의 최댓값 출력

![](https://velog.velcdn.com/images/keumsiun0503/post/1ac7035e-7b63-4d8a-9f93-6bc4df25ab29/image.png)
📖테스트 케이스
첫 줄에 물품의 수 **4(N)**와, 배낭이 담을 수 있는 무게 **7(K)**이 주어진다.
둘째 줄부터 N개의 줄에 걸쳐 각 물건의 **무게 W**와 **가치 V**가 주어진다.
W V
6 13 (물건1)
4 8	 (물건2)
3 6  (물건3)
5 12 (물건4)
이때 배낭 무게 7을 넘지 않으면서 최대 가치로 물건을 담는 경우는
물건2, 물건3을 담는 것이다.

💡**DP(Dynamic Programming)**
동적 계획 기법(DP)이란 각 지역해의 값을 이전 지역해의 값을 참고하며
동적으로 할당하는 알고리즘 기법을 얘기한다.
배낭 문제는 동적 계획 기법의 기본 문제이다.

🔥**각 물건을 하나씩 고려할 때, 해당 무게 값에서 최대 가치를 구하기**
각 ~를 하나씩 고려, 해당 ~ 값에서 최대/최소를 구하기는 DP 알고리즘을 풀 때 항상 언급되는 말이다.
테스트 케이스에 적용시켜보자.
- 물건2 고려-> 배낭에 담을 수 있는 무게 0~3까지 가치가 0.
(물건2는 무게가 4이기 때문에 안 담은 것으로 인식)
만약 무게가 4~7 -> 가치는 **물건2의 가치인 8**. 따라서 물건2를 고려했을 때 dp 배열은
dp[2][i(0~7)] : 0 0 0 0 8 8 8 8이 된다.
- 물건3 고려-> 배낭에 담을 수 있는 무게 0~2까지 가치가 0
(물건3은 무게가 3이기 때문에 안 담은 것으로 인식)
만약 무게가 3~7 -> 가치는 **물건3의 가치인 6**이 될 것이다.
🔥**하지만 무게가 4일 때는 이전에 고려한 물건2**를 담는 것이 가치를 더 높일 수 있다. 
따라서 이전의 값(dp[2][[4])와 비교하여 더 가치가 높은 값을 dp[3][4]에 저장하는 것이다.
**또한** 고려하고 있는 물건3을 담고, 무게가 남는다면, 남는 무게만큼 해당 무게의 가치를 더해도 된다.🔥 **무게가 7일 때는 물건3(무게3)을 담고, 물건2(무게4)도 담을 수 있다.**
- 최종적으로 모든 물건에 대하여 위와 같은 방식을 적용하고
1. 만약 고려하고 있는 물건을 담고도 무게가 남는다면, 
(남는 무게만큼 이전의 dp값에서 최대 가치 + 현재 물건의 가치 V) , 이를 이전의 dp값과 비교하여 더 큰 값을 저장
2. 그렇지 않다면 이전의 dp값을 저장

### 문제 풀이 설계
>
1. 물건 갯수 N, 배낭에 담을 수 있는 무게 K값 입력 받기
2. 물건들의 무게 W, 가치 V를 담을 2차원 배열 stuff 정의, 값 할당 받기
3. **각 물건(i)을 하나씩 고려할 때, 해당 무게 값(j)에서 최대 가치**를 저장할 dp 2차원 배열 선언 (dp[i][j] 값은 i번째 물건을 고려할 때, j무게값에서 최대 가치값. 0번째 물건은 아무 물건도 고려하지 않은 것으로 간주하고 모든 가치가 0)
4. 각 물건을 하나씩 고려하며 W, V에 해당 물건의 무게와 가치를 담고, 배낭에 담긴 물건의 무게값이 j일 때 최대 가치 dp[i][j]값을 아래 규칙에 의해서 구하기
4-1. (j>=W) 물건을 담고도 무게가 남는다면 
dp[i-1][j](이전 dp)값과 
dp[i-1][j-W](남는 무게에서의 최대 가치) + V 
두 값을 비교하여 큰 값을 dp[i][[j]에 저장
4-2. 그렇지 않다면
dp[i][[j] = dp[i-1][j]
5. dp[N][[K] 출력(모든 물건을 고려하여, 배낭에 최대 무게(K)로 물건을 담았을 때 배낭의 가치)

## 문제 풀이
```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    static int N, K;
    static int[][] stuff, dp;
    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;
        //물건 갯수 N, 배낭 무게 K
        st=new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        K = Integer.parseInt(st.nextToken());
        //2. 물건들의 무게 W, 가치 V 를 변수 stuff[i][0], stuff[i][1]에 저장
        stuff = new int[N+1][2];
        for(int i=1; i<=N; i++){
            st=new StringTokenizer(br.readLine());
            stuff[i][0] = Integer.parseInt(st.nextToken());
            stuff[i][1] = Integer.parseInt(st.nextToken());
        }
        //3. 각 물건(i)을 하나씩 고려할 때, 해당 무게 값(j)에서 최대 가치 dp[i][j]
        dp = new int[N+1][K+1];
        //4. 각 물건의 무게  W, 가치 V를 고려, 이전 dp값 dp[i-1] 을 고려하여 dp[i][j]값 저장하기
        for(int i=1; i<=N; i++){
            int W = stuff[i][0];
            int V = stuff[i][1];
            for(int j=0; j<=K; j++){
                if(j>=W){
                    dp[i][j] = Integer.max(dp[i-1][j], dp[i-1][j-W] + V);
                } else {
                    dp[i][j] = dp[i-1][j];
                }
            }
        }
        //5. 모든 물건을 고려하여, 배낭에 최대 무게(K)로 물건을 담았을 때 배낭의 가치
        System.out.println(dp[N][K]);
    }
}
```
![](https://velog.velcdn.com/images/keumsiun0503/post/5291d375-7681-4b4e-a0a6-d5ddb9d1b6d6/image.png)

### 느낀점
>
학교에서 DP 알고리즘의 기본 문제라고 배웠던 문제라 금방 아이디어를 떠올릴 수 있었다. 문제를 봤을 때 풀이 방법을 생각하는 것도 중요하지만, 유명한 풀이 방식, 자주 나오는 알고리즘은 많이 풀어보고, 익히고, 암기하여 속도를 줄이는 것도 필요하다고 생각한다.