## 문제
![](https://velog.velcdn.com/images/keumsiun0503/post/b7a090b8-1f12-4c9c-a23e-089f75f4e457/image.png)
https://www.acmicpc.net/problem/2178
### 문제 설명
>
1. NxM 크기의 미로가 주어진다
2. 1은 이동할 수 있는 칸, 0은 이동할 수 없는 칸
3. (1, 1) 에서 출발하여 (N, M) 위치로 이동할 때 지나야 하는 최소 칸 수를 출력(시작 위치, 도착 위치 포함)
- 조건1 : 인접한 칸으로만 이동할 수 있음
- 조건2 : 항상 도착 위치로 이동할 수 있는 경우만 입력으로 주어짐

![](https://velog.velcdn.com/images/keumsiun0503/post/43ab1deb-5c32-4342-bfc0-955f6a9fac2f/image.png)

📘예제 입력1번을 통해서 해결책을 찾아보자.
N x M 미로에서 좌측 상단의 1부터 시작하여, 우측 하단의 1까지 최소로 몇 칸을 이동하는지 묻는 문제이다.
그렇다면 좌측 상단의 1부터 시작하여, 상하좌우 위치를 탐색하는 것이 필요할 것이고, 여러 방향에 길이 있을 수도 있으므로 다음 알고리즘을 필요로 한다.
💡**BFS(Breadth-First Search) 너비 우선 탐색**
한 정점에서 시작하여 인접한 정점을 차례대로 방문하며 탐색하는 방법이다. 더 이상 방문할 정점이 없다면 종료한다.
🔥**BFS**에서는 자료구조로 **큐**를 사용한다.
- 시작 정점으로부터 인접한 모든 정점을 **큐**에 삽입.
또 **큐**에서 정점을 뽑아서 인접한 모든 정점을 **큐**에 삽입.
이러한 방식을 반복하는 탐색 방식이다.

✅**해결 방식**
좌측 상단의 위치 (문제에선 1, 1 위치지만 **편의상 0, 0위치**로 설정)
에서 시작하여 **인접한 위치에 길이 있다면(1이라면) **
1. 해당 위치를 **큐에 삽입**
2. 해당 위치에 **이전 위치의 값+1** 저장 
⭐(몇 칸을 이동하여 해당 위치에 왔는 지 알 수 있음)
위 1, 2 과정을 큐가 비어있는 상태가 될 때까지 반복하면
우측 하단의 위치(N-1, M-1)에 해당 위치까지 이동하는 데에 몇 칸을 이동했는지 저장된다.

📖좌표 값은 **Java의 awt 라이브러리에 포함된 Point 클래스** 활용
- 생성자 new Point(x, y)는 x, y의 좌표 값을 가지는 Point 객체 생성
- 객체.getX() 메소드는 객체의 x좌표 값을 불러옴
- 객체.getY() 메소드는 객체의 y좌표 값을 불러옴
### 문제 풀이 설계
>
1. N(행), M(열) 값 입력 받음
2. N, M 크기의 arr(미로)배열을 정의하고, 각 미로의 값을 입력 받음
3. BFS(0, 0) 탐색 시작
4. 시작 위치(0, 0)을 큐에 삽입
5. 큐가 비어있는 상태가 될 때까지 아래 과정을 반복
5-1. 큐에서 좌표 하나를 꺼내온다.
5-2. 해당 좌표의 인접한 위치에 길이 있다면(범위를 벗어나지 않고, 1이라면),
->해당 위치를 큐에 삽입, 
->해당 위치에 이전 위치의 값 + 1 저장.
6. arr[N-1][M-1]값을 반환 받고 출력

## 문제 풀이
```java
import java.awt.Point;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;
class Main{
    static int N;
    static int M;
    static int[][] arr;
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static Queue<Point> queue = new LinkedList<>();
    static int BFS(int x, int y){
        //4. 시작 위치 0, 0 위치를 큐에 삽입
        queue.offer(new Point(x, y));
        //5. 큐가 빌 때까지 반복
        while(!queue.isEmpty()){
            //5-1. 큐에서 좌표 하나 꺼내오기
            Point currentPoint = queue.poll();
            int currentX = (int)(currentPoint.getX());
            int currentY = (int)(currentPoint.getY());
            for(int i=0; i<4; i++){
                int nextX = currentX + dx[i];
                int nextY = currentY + dy[i];
                //5-2. 인접한 위치에 길이 있다면(범위를 벗어나지 않고, 1이라면)
                if(nextX>=0 && nextX<N && nextY>=0 && nextY<M && arr[nextX][nextY]==1){
                    queue.offer(new Point(nextX, nextY));
                    arr[nextX][nextY] = arr[currentX][currentY] + 1;
                }
            }
        }
        //6. arr[N-1][M-1] (0,0에서 시작했으므로) 값 반환
        return arr[N-1][M-1];
    }
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        //1. N, M 입력 받기
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        //2. N, M크기의 미로 배열에 값 입력 받기
        arr = new int[N][M];
        for(int i=0; i<N; i++){
            String str = br.readLine();
            //str.charAt(i)로 i인덱스 위치의 char 자료형 값 = 아스키 코드 값(0은 48, 1은 49) 불러오기 ( - '0의 아스키 코드 값(48)')
            for(int j=0; j<M; j++){
                arr[i][j] = str.charAt(j) - '0';
            }
        }
        //3. BFS탐색 값 반환 받아 출력
        System.out.println(BFS(0, 0));
    }
}
```
![](https://velog.velcdn.com/images/keumsiun0503/post/72fbf9a9-4bbe-475e-8cea-13914ef90e4a/image.png)

### 느낀점
>
이 문제도 코딩테스트를 처음 시작해서 여러 알고리즘을 접할 때는 못 풀던 문제인데, 문제들을 꾸준히 풀다보니 풀이 방식이 잘 떠올랐다. 꾸준하게 실버 상위 문제를 풀면서 골드 하위 문제들에 계속 도전해야겠다.
