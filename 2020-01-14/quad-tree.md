# Problem: Binary Images' Quad-Tree Compression & Image reflection algorithm
출처: [https://algospot.com/judge/problem/read/QUADTREE](https://algospot.com/judge/problem/read/QUADTREE)

## Problem description

대량의 좌표 데이터를 메모리 안에 압축해 저장하기 위해 사용하는 여러 기법 중 쿼드 트리(quad tree)란 것이 있습니다.
주어진 공간을 항상 4개로 분할해 재귀적으로 표현하기 때문에 쿼드 트리라는 이름이 붙었는데, 이의 유명한 사용처 중 하나는 검은 색과 흰 색밖에 없는 흑백 그림을 압축해 표현하는 것입니다.
쿼드 트리는 2N × 2N 크기의 흑백 그림을 다음과 같은 과정을 거쳐 문자열로 압축합니다.

이 그림의 모든 픽셀이 검은 색일 경우 이 그림의 쿼드 트리 압축 결과는 그림의 크기에 관계없이 b가 됩니다.
이 그림의 모든 픽셀이 흰 색일 경우 이 그림의 쿼드 트리 압축 결과는 그림의 크기에 관계없이 w가 됩니다.
모든 픽셀이 같은 색이 아니라면, 쿼드 트리는 이 그림을 가로 세로로 각각 2등분해 4개의 조각으로 쪼갠 뒤 각각을 쿼드 트리 압축합니다.
이때 전체 그림의 압축 결과는 `x(왼쪽 위 부분의 압축 결과)(오른쪽 위 부분의 압축 결과)(왼쪽 아래 부분의 압축 결과)(오른쪽 아래 부분의 압축 결과)`가 됩니다.
예를 들어 그림 (a)의 왼쪽 위 4분면은 `xwwwb`로 압축됩니다.
그림 (a)와 그림 (b)는 16×16 크기의 예제 그림을 쿼드 트리가 어떻게 분할해 압축하는지를 보여줍니다. 이때 전체 그림의 압축 결과는 `xxwwwbxwxwbbbwwxxxwwbbbwwwwbb`가 됩니다.

쿼드 트리로 압축된 흑백 그림이 주어졌을 때, 이 그림을 상하로 뒤집은 그림 을 쿼드 트리 압축해서 출력하는 프로그램을 작성하세요.

### 입력
    첫 줄에 테스트 케이스의 개수 C (C≤50)가 주어집니다. 그 후 C줄에 하나씩 쿼드 트리로 압축한 그림이 주어집니다. 모든 문자열의 길이는 1,000 이하이며, 원본 그림의 크기는 220 × 220 을 넘지 않습니다.

### 출력
    각 테스트 케이스당 한 줄에 주어진 그림을 상하로 뒤집은 결과를 쿼드 트리 압축해서 출력합니다.

### 예제 입력
    4
    w
    xbwwb
    xbwxwbbwb
    xxwwwbxwxwbbbwwxxxwwbbbwwwwbb
    
### 예제 출력
    w
    xwbbw
    xxbwwbbbw
    xxwbxwwxbbwwbwbxwbwwxwwwxbbwb
    
    
## Solution in Python
        class Queue(list):
            # enqueue == > insert
            enqueue = list.append
            # dequeue == > delete
            def dequeue(self):
                return self.pop(0)

            def is_empty(self):
                if not self:
                    return True
                else:
                    return False

            def peek(self):
                return self[0]

        def stractify(quadtree):
            result = []
            for i in range(4):
                if len(quadtree)==0: break
                q = quadtree.pop(0)
                if q == 'x':
                    result.append(stractify(quadtree))
                else:
                    result.append(q)
            return result

        def decode(code):
            answer = ''
            if len(code)==4:
                answer += 'x'
                for i in code:
                    answer+=decode(i)
                return answer
            else:
                return code

        def up_and_down(code):
            if len(code)==4:
                return [up_and_down(code[2])+up_and_down(code[3])+up_and_down(code[0])+up_and_down(code[1])]
            #'x'+up_and_down(code[2])+up_and_down(code[3])+up_and_down(code[0])+up_and_down(code[1])
            elif len(code)==1:
                return [code]

        def left_and_right(code):
            if len(code)==4:
                return [left_and_right(code[1])+left_and_right(code[0])+left_and_right(code[3])+left_and_right(code[2])]
            else:
                return [code]

        def up_down_left_and_right(code):
            if len(code)==4:
                return [up_down_left_and_right(code[-1])+up_down_left_and_right(code[-2])+up_down_left_and_right(code[-3])+up_down_left_and_right(code[-4])]
            elif len(code)==1:
                return [code]

        if __name__=="__main__":
            # w
            # xbwwb
            # xbwxwbbwb
            # xxwwwbxwxwbbbwwxxxwwbbbwwwwbb

            # w
            # xwbbw
            # xxbwwbbbw
            # xxwbxwwxbbwwbwbxwbwwxwwwxbbwb

            quadtree = 'xxwwwbxwxwbbbwwxxxwwbbbwwwwbb'
            print(quadtree)

            quadtree = Queue(quadtree)
            code = stractify(quadtree)[0]
            print(code)

            reflected_code = up_and_down(code)[0]
            print(reflected_code)

            reflected_quadtree = decode(reflected_code)
            print(reflected_quadtree)
