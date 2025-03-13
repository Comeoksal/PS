## 문제

![](https://velog.velcdn.com/images/keumsiun0503/post/17344ae9-2340-44a1-aebd-aa14c4a1d5ac/image.png)
https://www.acmicpc.net/problem/2606

### 문제 설명
>
1. 컴퓨터가 바이러스에 걸리면 해당 컴퓨터와 네트워크 상으로 연결되어 있는 모든 컴퓨터는 바이러스에 걸린다.
2. 예를 들어 <그림 1>과 같이 컴퓨터가 연결되어 있을 때, 1번 컴퓨터가 바이러스에 걸리면 연결된 컴퓨터(2, 3, 5, 6)들은 모두 바이러스에 걸린다.
3. 1번 컴퓨터가 바이러스에 걸렸을 때, 컴퓨터의 수(1번 부터 차례대로 번호가 매겨짐)와 연결되어 있는 정보가 주어질 때, 1번 컴퓨터를 통해서 바이러스에 걸리게 되는 컴퓨터의 수를 출력

![](https://velog.velcdn.com/images/keumsiun0503/post/4ed0b4bc-3690-4ed6-a289-8965dfa6493d/image.png)
테스트 케이스를 보면, 우리는 각 컴퓨터들의 연결 정보를 알 수 있다.
문제는 우리한테 두 가지 문제 해결 방식을 요구한다.
**1. 컴퓨터들의 연결 정보를 주었을 때, 이를 자료구조를 활용해서 연결할 수 있는가?**
**2. 자료구조를 활용하여 연결된 컴퓨터들을 1번부터 시작하여 탐색하고, 1번과 연결된 모든 컴퓨터를 찾아낼 수 있는가?**

문제의 <그림1>을 예시로 한 번 풀어보자면,
![](https://velog.velcdn.com/images/keumsiun0503/post/8af01bb5-b4d4-4dfb-80e7-b3f3aa11cd24/image.png)
먼저 예제 입력1번과 같이 입력을 받게 될 것이다.
각 컴퓨터 번호를 헤드포인터로 하여 연결된 컴퓨터를 입력 받는 순서대로 연결한다. **(무방향으로 연결 되어 있으므로 입력 받을 때 양쪽으로 연결 될 수 있게 해준다.)**
ex)
1 -> 2, 5
2 -> 1, 3, 5
3 -> 2
4 -> 7
5 -> 1, 2, 6
6 -> 5
7 -> 4
다음과 같이 자료구조를 만들려면, 자바에서는 **List 배열 안에 ArrayList<>() 컬렉션을 넣어서 List의 각 인덱스는 헤드포인터(1, 2, .. , 7) ArrayList안의 숫자 배열은 각 헤드포인터에 연결된 컴퓨터** <<와 같은 형식으로 만들 수 있다.
그렇다면 연결된 컴퓨터들을 1번부터 탐색하는 문제는 어떻게 해결하면 될까?
일단 우리가 머릿속으로 <그림1>을 봤을 때 푸는 방식을 생각해보자.
- 1번부터 시작해서 1번과 연결된 **2, 5**를 찾아가서 체크하고,
2번과 연결된 **3**을 찾아가서 체크, 5번과 연결된 **6**을 찾아가서 체크.
이런 방식으로 총 4대의 컴퓨터가 1번 컴퓨터로 인해서 바이러스에 걸리는 것을 찾음.

그렇다면 이러한 탐색 방식이 있을까?
💡BFS(Breadth-First Search) 너비 우선 탐색
<그림 1>과 같이 정점(컴퓨터)과 연결된 간선을 모아 놓은 자료구조, 연결되어 있는 객체간의 관계를 표현한 자료구조를 **그래프**라고 한다.
그래프를 탐색하는 방법은 다양한 방식이 있는데, 그 중 **BFS**는 널리 사용되고, 활용도가 높은 탐색 방식이다. BFS의 특징은 다음과 같다.
- BFS에서는 방문할 정점을 담아둘 자료구조로 **큐**를 사용한다.
- 방문했던 정점을 체크할 **배열(visited)**이 필요하다.

탐색 방식은 위에서 설명한 방식과 일치하고 간단하다.
1. 처음 탐색을 시작하려는 정점을 **큐**에 넣고 **2, 3번 과정**을 **큐**가 비어있는 상태가 될 때까지 반복
2. 탐색을 시작하려는 정점이 만약 방문한 적이 없다면, **방문했음**을 체크하고, 해당 정점으로부터 연결된 정점 중 방문하지 않은 모든 정점을 **큐**에 삽입
3. **큐**에서 정점을 하나 꺼내어 2번과정으로

위 과정을 간단하게 그려보면 다음과 같다.(방문했음 체크 생략)
1. 큐[1] (탐색 시작 정점 1을 큐에 삽입)
2. 1-> 큐[2, 5] (큐에서 1을 꺼내어 연결된 모든 정점 큐에 삽입)
3. 2-> 큐[5, 3] (큐에서 2를 꺼내어 연결된 모든 정점 큐에 삽입, 1은 방문했으므로 제외)
4. 5-> 큐[3, 6] (큐에서 5를 꺼내어 연결된 모든 정점 큐에 삽입, 1, 2는 방문했으므로 제외)
5. 3-> 큐[6] (큐에서 3을 꺼내어 연결된 모든 정점 큐에 삽입(없음), 2는 방문했으므로 제외)
6. 6-> 큐[] 🔥종료🔥 (큐에서 6을 꺼내어 연결된 모든 정점 큐에 삽입(없음))

### 문제 풀이 설계
>
1. 컴퓨터의 수 **N**과 연결된 간선의 수 **C**를 입력 받음
2. 컴퓨터의 수만큼 **헤드포인터** 역할을 할 **List**의 크기 지정
3. 각 헤드포인터에 연결할 배열을 생성**(List안에 ArrayList)**
4. 연결된 컴퓨터 정보를 입력 받음, **무방향 연결이므로 양쪽 헤드포인터에 각각 연결.**
5. 방문했음을 체크할 visited 배열의 크기를 지정해주고 **BFS(1)**탐색 시작
6. **BFS(1)** (**count 변수는 1번과 연결된 컴퓨터의 수**)
6-1. 파라미터로 받은 정점 1을 큐에 삽입(시작 정점)후 **큐가 비어있는 상태가 될 때까지 반복**
6-2. 탐색을 시작하려는 정점이 만약 방문한 적이 없다면, **방문했음**을 체크하고, count값 1증가. 해당 정점으로부터 연결된 정점 중 방문하지 않은 모든 정점을 **큐**에 삽입
6-3. **큐에서 정점 하나를 꺼내어** 2번 과정으로
7. 탐색이 끝난 후(큐가 비어 종료) count-1(1번 컴퓨터 제외)을 출력

## 문제 풀이
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {
    static int N, C;
    static boolean [] visited;
    static List<Integer>[] computers;
    static int BFS(int x){
        Queue<Integer> queue = new LinkedList<>();
        int count = 0;
        //6-1. 시작 정점 1을 큐에 삽입 후 큐가 빌 때까지 반복
        queue.add(x);
        while(!queue.isEmpty()){
            int target = queue.poll();
            //6-2. 방문하지 않은 컴퓨터에 대해서 탐색 과정 수행
            if(!visited[target]){
                visited[target] = true;
                count++;
                for(int i=0; i<computers[target].size(); i++){
                    int next_target = (int)(computers[target].get(i));
                    if(visited[next_target]!=true){
                        queue.add(next_target);
                    }
                }
            }
        }
        //7. 방문한 컴퓨터 중 1번 컴퓨터를 제외한 연결된 컴퓨터의 수 출력
        return count-1;
    }
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        //1. 컴퓨터의 수, 연결된 간선의 수 압력 받기
        N = Integer.parseInt(br.readLine());
        C = Integer.parseInt(br.readLine());
        //2. List 크기 지정
        computers = new List[N+1];
        //3. List 내부 각 요소의 값을 ArrayList로 할당
        for(int i=1; i<=N; i++){
            computers[i] = new ArrayList<Integer>();
        }
        //4. 연결된 컴퓨터 정보 입력 받기
        StringTokenizer st;
        for(int i=0; i<C; i++){
            st = new StringTokenizer(br.readLine());

            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());
            //4. 무방향 연결이므로 양쪽 헤드포인터에 각각 연결
            computers[a].add(b);
            computers[b].add(a);
        }
        //5. visited 배열 크기 지정
        visited = new boolean[N+1];
        System.out.println(BFS(1));
    }
}
```
![](https://velog.velcdn.com/images/keumsiun0503/post/e13a05f5-734a-4aeb-84c9-0bcc6d611ae1/image.png)
### 느낀점
>
DFS, BFS 문제는 머리 속으로는 쉽게 생각하는 풀이 방식인데, 처음 접하면 구현하기 까다로운 문제 중 하나라고 생각한다. DFS, BFS 문제를 잘 해결하기 위해서는 두 방식의 특징과 활용 방식을 잘 이해해야하며, 두 방식을 활용하는 문제를 자주 풀어보는 것이 중요한 것 같다. 나도 자주 풀면서 익숙해져야겠다.