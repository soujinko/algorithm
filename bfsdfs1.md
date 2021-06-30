## 백준 7576토마토 / 2667 단지번호 붙이기 풀이

1. 7576토마토

배열에서 1인 지점부터 시작해서 +1일마다 주변 토마토가 익어간다.
기존에 풀던 bfs와 다른점은 하나씩 q에 넣어주는 것이 아니라, 토마토의 위치를 한번에
큐에 넣고 bfs 탐색을 시작해야 한다는 점.
때문에 for문으로 우선 토마토있는 위치를 큐에 넣은 다음 함수를 실행시켜 토마토
위치들로 부터 동시에 bfs탐색을 하며 토마토를 익게 한다.

    from collections import deque
    import sys

    m, n= map(int, input().split())
    q = deque()
    dx = [-1, 1, 0, 0]
    dy = [0, 0, -1, 1]
    matrix = [list(map(int, sys.stdin.readline().split()))for _ in range(n)]

    def bfs():
        while q:
            x, y = q.popleft()
            for i in range(4):
                nx = x + dx[i]
                ny = y + dy[i]
                if 0 <= nx < n and 0 <= ny < m:
                    if matrix[nx][ny] == 0:
                        matrix[nx][ny] = matrix[x][y] + 1
                        q.append([nx, ny])

    for i in range(n):
        for j in range(m):
            if matrix[i][j] ==1:
                q.append([i, j])

    bfs()
    isTrue = False
    result = -2
    
    for i in matrix:
        for j in i:
            if j == 0:
                isTrue = True
            result = max(result, j)
    #안익은게 있으면 -1, 익을 토마토가 없으면(최대수가 1)0, 이외에는 최대 일수 반환
    if isTrue == True:
        print(-1)
    elif result == -1:
        print(0)
    else:
        print(result - 1)

2. 단지번호 붙이기

재귀호출하여 dfs탐색하였다. 재귀호출로 인자 cnt +1 하여 인접한 집의 수를 카운팅하고 리턴.
방문한 곳은 숫자를 1 -> 0으로 바꿔주어 방문여부를 표시.
인접한 집들은 하나의 단지로 묶이기 때문에 함수를 호출한 수가 곧 단지의 수가 됨.
함수 결과를 result에 추가하여 길이 및 원소를 소팅하여 프린트한다.

    import sys
    n = int(input())

    def dfs(n_list, cnt,  x, y):
        n_list[x][y] = 0
        dx = [-1, 1, 0, 0]
        dy = [0, 0, -1, 1]
        for i in range(4):
            nx = x + dx[i]
            ny = y + dy[i]
            if 0 <= nx < n and 0 <= ny < n:
                if n_list[nx][ny] == 1:
                    cnt = get_num(n_list, cnt+1, nx, ny)
        return cnt

    n_list = [list(map(int, sys.stdin.readline().strip())) for _ in range(n)]
    result = []
    for i in range(n):
        for j in range(n):
            if n_list[i][j] == 1:
                result.append(dfs(n_list, 1, i, j))


    print(len(result))
    for i in sorted(result):
        print(i)