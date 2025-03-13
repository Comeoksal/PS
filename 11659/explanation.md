## 문제
![](https://velog.velcdn.com/images/keumsiun0503/post/bbeff75e-634c-4228-9c72-3cb58478d714/image.png)
https://www.acmicpc.net/problem/11659

### 문제 설명
>
1. 수 N개가 주어지면 i번째 수부터 j번째 수까지 합을 출력
2. 수의 개수 N개와 합을 구해야 하는 횟수 M이 주어짐.
3. 둘째 줄에는 N개의 수가 주어짐, 셋째 줄부터 M개의 줄에는 합을 구해야 하는 구간 i와 j가 주어짐.
- 조건 1: 시간 제한 1초
- 조건 2: 수는 1000보다 작거나 같은 자연수이며, 수의 개수는 최대 100,000개

📖테스트 케이스 살펴보기
![](https://velog.velcdn.com/images/keumsiun0503/post/13bbfcbc-46d9-4566-bb1b-cb6bc6950021/image.png)

예제 입력 1에서는 5개의 수
5 4 3 2 1 이 주어졌다.
1 3은 첫번째 수 ~ 세번째 수까지의 합 5 + 4 + 3 = 12
2 4는 두번째 수 ~ 네번째 수까지의 합 4 + 3 + 2 = 9
5 5는 다섯번째 수 ~ 다섯번째 수까지의 합 5 = 5
와 같은 방식으로 문제를 해결하면된다.

🤔i와 j가 주어지면 -1한 값이 index 값이므로 i-1~j-1 index를 방문하며 더하면 될 것 같지만..
이는 N개의 수를 방문하면 시간 복잡도가 O(N)에 해당하고, 이를 M번 반복하므로 O(NxM) 시간 복잡도를 갖게 되고, **조건 1**에 만족하지 못 합니다.

💡**누적합**
수열에서 i번째 수 ~ j번째 수의 합을 구하는 방식에는 두가지가 있습니다.
1. 위에서 설명했듯이 직접 순회하면서 더하는 방식.**(A[i] + A[i+1] + .. + A[j])**
2. **누적합**을 통해서 구하는 방식 **(S[j]-S[i-1])**
_#여기서 S[i]란 첫번째 수부터 i번째 수까지의 합을 의미합니다._

🔥예제를 통해서 살펴봅시다.
1 2 3 4 5 가 주어졌을 때, 
두번째 수부터 네번째 수까지의 합은 2 + 3 + 4 = 9 입니다.
**누적합** 풀이 방식을 적용시키려면 우선 S를 구해야 합니다.
(각 수를 입력 받을 때, 이전의 수에 더하여 저장하면 됨 **시간복잡도 O(N)**)
S = { 1 3 6 10 15 } 가 되고
S[4] - S[2-1] = 9가 됩니다.
이는 첫번째 방식과 달리 **시간 복잡도가 O(1)**에 해당합니다.
따라서 최종적으로 M번 반복해야하므로 **시간 복잡도가 O(N+M)**이 됩니다.

### 문제 풀이 설계
>
1. 수의 갯수 N, 합을 구하는 횟수 M 입력 받기
2. 누적합을 저장할 배열 S에 N개의 수를 입력 받으며 누적합 저장 (S[i] = S[i-1] + 입력한 수)
3. M번 반복하며 누적 합을 구해야 하는 구간 i, j 입력 받기
4. 누적합 풀이 방식을 활용하여 구간 i~j까지의 합 S[j] - S[i-1] 출력

## 문제 풀이
```java
import java.io.*;
import java.util.*;

public class Main {
    static int N, M;
    static int[] S;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        //1. N, M 입력 받기
        StringTokenizer st;
        st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        //2. 누적 합을 저장할 S 선언 및 값 할당
        S = new int[N+1];
        st = new StringTokenizer(br.readLine());
        for(int i=1; i<=N; i++){
            S[i] = S[i-1] + Integer.parseInt(st.nextToken());
        }

        StringBuilder sb = new StringBuilder();
        //3. M번 반복하며 구간 i, j 받기
        while(M-->0){
            st = new StringTokenizer(br.readLine());
            int i = Integer.parseInt(st.nextToken());
            int j = Integer.parseInt(st.nextToken());
            //4. i~j까지의 합을 구하기 위해 누적합 방식 활용
            sb.append(S[j]-S[i-1]).append("\n");
        }
        System.out.print(sb.toString());
    }
}
```
![](https://velog.velcdn.com/images/keumsiun0503/post/224d7c39-00ac-4a66-9498-93dc6233098d/image.png)
### 느낀점
>
중, 고등학교 수학시간에 배웠던 기본적인 합공식인데 잊고 있었다. PS 문제에서 기본적인 수학 공식을 활용하는 문제가 많이 나오는데, 이를 많이 풀어보며 수학 감각을 되찾고, 잊지 않으려고 노력해야겠다..
