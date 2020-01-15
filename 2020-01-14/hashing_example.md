# Problem: hashing_example

출처: [https://www.acmicpc.net/problem/7615](https://www.acmicpc.net/problem/7615)


## Problem Description

상근이는 정수를 0부터 m-1까지의 정수로 매핑시키는 해싱 함수 h(y) = a·y + b mod m를 만들었다.
x, n, c, d가 주어졌을 때, 해시값 h(x), h(x+1), ..., h(x+n) 중 몇 개가 구간 [c,d]에 포함되는지 구하는 프로그램을 작성하시오.

### 입력
첫째 줄에 테스트 케이스의 개수 t (1 ≤ t ≤ 105)가 주어진다. 다음 t개 줄에는 a, b, x, n, c, d, m이 주어진다.
(1 ≤ m ≤ 1015, c ≤ d < m, a,b < m, x+n ≤ 1015, a·(x+n)+b ≤ 1015)
입력으로 주어지는 모든 수는 음이 아닌 정수이다.

### 출력
각 테스트 케이스마다 c ≤ a·(x+i)+b mod m ≤ d 을 만족하는 0 ≤ i ≤ n의 개수를 출력한다.

### 예제 입력
    2
    2 3 1 3 0 1 7
    1 0 0 8 0 8 9

### 예제 출력
    1
    9
    
## Solution in Python
    N = int(input())
    for i in range(N):
        data = input()
        count = 0
        a, b, x, n, c, d, m = [ int (i) for i in data.split(' ')]
        for y in range(x, x + n + 1):
            h_y = (a * y + b) % m
            if h_y >= c and h_y <= d:
                count += 1
        print(count)
