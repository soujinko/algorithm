## 백준 12015 가장 긴 증가하는 부분 수열2

긴 증가하는 부분 수열1과 같이 dp로 풀면 시간초과가 뜬다.  
주어진 수열의 폭이 넓기 때문. 주어진 수열에서 반복문을 돌면서 스택에 넣어준다.  
스택의 마지막 원소보다 i가 크다면 append를 해주고 아니라면 함수를 호출하여  
이분탐색으로 스택에 대체할 위치를 찾아준다. 결과 수열은 비록 예상하던 수들의   집합이 아닐 수 있지만  
수열의 길이만 구하면 되는 것이기 때문에 다음과 같은 방식으로 구현.  

    import sys

    n = int(sys.stdin.readline())
    n_array = list(map(int, sys.stdin.readline().split()))

    def get_location(l, i):
        s = 1
        e = l
        while s < e:
            mid = (s + e) // 2
            if i < stack[mid]:
                e = mid
            elif i > stack[mid]:
                s = mid + 1
            elif i == stack[mid]:
                s = e = mid
        return e

    stack = [0]
    for i in range(n):
        if n_array[i] > stack[-1]:
            stack.append(n_array[i])
        else:
            mew_location = get_location(len(stack)-1, n_array[i] )
            stack[mew_location] = n_array[i]

    print(len(stack)-1)

아래와 같이 bisect를 이용하여 구현 가능하며 속도도 1/3로 빠르다.   
bisect_left(literable, value) : 왼쪽 인덱스를 구하기  
bisect_right(literable, value) : 오른쪽 인덱스 구하기  

    import sys
    from bisect import bisect_left
    n = int(sys.stdin.readline())
    n_array = list(map(int, sys.stdin.readline().split()))

    stack = [0]
    for i in range(n):
        if n_array[i] > stack[-1]:
            stack.append(n_array[i])
        else:
            stack[bisect_left(stack, n_array[i])]

    print(stack)

다음과 같이 특정 범위에 속하는 원소의 개수를 구하는데 활용 가능  

    from bisect import bisect_left, bisect_right 

    def calCountsByRange(nums, left_value, right_value): 
        r_i = bisect_right(nums, right_value) 
        l_i = bisect_left(nums, left_value) 
        return r_i - l_i 
        
    nums = [-1, -3, 5, 5, 4, 7, 1, 7, 2, 5, 6]
    nums.sort() 
    print(calCountsByRange(nums, 5, 7))

    출처: https://programming119.tistory.com/196 [개발자 아저씨들 힘을모아]