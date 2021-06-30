## 백준 11053 가장 긴 증가하는 부분 수열

해당 원소(i)까지 최대 배열의 길이를 result에 저장하며 배열을 돈다. 새로운 원소(i)에 자신보다 작은원소(j)중 가장 긴 배열값을 가진 j의 값에 +1 한 값을 할당한다. 
배열을 모두 돌면 result에 저장된 값 중 최대값에 +1 한 값을 반환한다.(처음 원소 수 더하기)

    n = int(input())
    array = list(map(int,input().split()))
    result = [0]*n

    for i in range(n):
        for j in range(0, i):
            if array[i] > array[j] and result[j] >= result[i]:
                result[i] = result[j] + 1

    max_length = max(result) + 1

    print(max_length)