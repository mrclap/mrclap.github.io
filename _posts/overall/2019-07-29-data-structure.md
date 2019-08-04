---
layout: post
title: data structure
categories:
  - overall 
# feature_image: ""
tags: 컴퓨터과학, data structure
---
> 데이터 구조, [한재엽님의 Part 1-2 Data Structure](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/DataStructure)를 기반으로 여러분들의 블로그를 참조!

# 02. DataStructure


### Array vs LinkedList
#### Array
- 논리적 저장 순서 = 물리적 저장 순서
- 접근하고자 하는 값의 인덱스를 알고 있다면, 상수 시간(Big-O(1))에 접근할 수 있다.
  - Random access가 가능하다
- 하지만 삭제, 삽입 과정에서 엄청난 시간이 걸린다(논리적 저장 순서 = 물리적 저장 순서 때문에..)


#### LinkedList
- 자기 자신 다음의 원소가 누군지만 알고있음, 삭제와 삽입이 상수 시간에 가능
- 특정 값을 사용하기 위해서는 O(n) 시간 필요(처음부터 물고물고 찾아가야함)
- 결국 삭제, 삽입 시에도 특정 값을 찾아야하므로 O(n)시간이 걸림..

<br/>
### Stack and Queue
#### Stack
- LIFO

#### Queue
- FIFO


<br/>
### Tree
비선형자료구조, 계층적 관계를 표현(Hierarchical Relationship)
- node: 트리를 구성하는 각각 요소
- edge: 노드와 노드를 연결하는 선
- root node: 트리 구조의 최상단에 있는 노드
- terminal node(leaf node): 하위에 아무 노드도 없는 노드
- internal node: 단말 노드(terminal node)를 제외한 모든 노드, 루트 노드 포함

#### Binary tree
- 루트 노드를 중심으로 두 개의 서브 트리로 구성
- 서브 트리도 binary tree
- root node의 level : 0
- height : 트리의 최고 레벨
- Full Binary Tree (포화 이진 트리) / Complete Binary Tree (완전 이진 트리)
  - 포화 이진 트리 : 모든 레벨이 꽉찬 이진 트리
  - 완전 이진 트리 : 위->아래, 왼쪽->오른쪽 순서로 차곡차곡 채워진 트리
    - parent(i) = i/2
    - left_child(i) = 2i,
    - right_child(i) = 2i+1
    
#### BST(Binary Search Tree)
효율적인 탐색을 위한 **저장**
- 규칙1. 이진탐색 트리의 노드에 저장된 키는 유일함
- 규칙2. 루트 노드의 키가 왼쪽 서브 트리를 구성하는 어떤 노드의 키보다 크다.
- 규칙3. 루트 노드의 키가 오른쪽 서브 트리를 구성하는 어떤 노드의 키보다 작다.
- 규칙4. 왼쪽과 오른쪽의 서브트리 역시 이진 탐색 트리.
트리가 한 쪽으로 편향되어 구성될 경우 의미가 없어지므로 균형을 잡기위해 rebalancing(Red-Black Tree 등과 같은)을 한다.

<br/>
### Binary Heap
- Complete Binary Tree
- 0번을 건너뛰고 1번 index부터 루트노드가 시작
- 루트노드 제거 시, 맨 마지막 노드를 root node에 놓고 heapify
    - heapify
        - O(log n) : 최대 root node부터 맨 아래 까지 내려가며 교체가 일어날 수 있으므로
- max heap / min heap
    - max heap 
        - 각 노드의 값이 노드의 children보다 크거나 같은 complete binary tree
        - max : root node
    - min heap
        - 각 노드의 값이 노드의 children보다 작거나 같은 complete binary tree
       
<br/>
### Red Black Tree
BST 기반의 트리형식 자료구조, Search, Insert, Delete에 O(log n)의 시간 복잡도 소요
1. 각 노드는 Red or Black의 색깔을 가짐
2. Root node : black
3. leaf node : black
4. 어떤 노드의 색깔이 red이면, children은 black
5. 노드 ~ descendant leaves까지의 단순경로는 같은 수의 black nodes를 포함 : Black-Height


- [***나중에 다시보자!!!***](https://ko.wikipedia.org/wiki/%EB%A0%88%EB%93%9C-%EB%B8%94%EB%9E%99_%ED%8A%B8%EB%A6%AC)
- [zedd0202님의 친절한 설명](https://zeddios.tistory.com/237)

<br/>
### HashTable
- 내부적으로 배열을 사용하여 특정값을 찾는데에 상수시간에 접근이 가능
- 특별한 알고리즘을 이용하여 저장데이터와 연관이 되는 고유한 숫자를 인덱스로 사용

#### hash function
- Collision을 최소화하는 방향으로 설계되어야함
- 1:1대응에 가갑게 만드는것은 거의불가능 / array와 다를바 없음

#### Collision 해결방법
1. Open Address방식(개방주소법)
  - 해시 충돌 발생시, 다른 해시 버킷에 자료를 삽입
  - Linear Probing : 순차적으로 빈 버킷 탐색
  - Quadratic probing : 2차 함수를 이용하여 탐색
  - Double hashing probing : 충돌발생시 2차 해쉬 함수를 이용하여 새주소 할당(연산량 가중)

2. Seperate Chaining방식(분리연결법)
  - Open Address방식에 비해 빠름
  - Open Address방식의 경우 해시 버킷 밀도가 높아지면 재 탐색 비율이 높아짐
  - Linked List : 각각의 버킷들을 연결리스트로 만들어 충돌 발생시 list삽입
  - Red-Black Tree : 연결 리스트 대신 트리를 사용, 다만 메모리 측면에서 데이터의 갯수가 작으면 Linked List사용
    - 데이터가 작다는 것의 의미
      - 하나의 해시 버킷에 할당된 key-value 쌍의 갯수 기준
      - 6-8개 기준, 기준이 7이면 6되었다가 7이 될 때마다 구조변경이 발생하므로 약간의 버퍼를 둠.
     
#### 해시 버킷 동적 확장(Resize)
- 해시 버킷의 개수가 적으면 메모리의 절약이 가능하지만 해시충돌로 인한 성능 손실이 발생, 따라서 HashMap의 key-value 쌍이 일정 개수 이상(해시 버켓의 0.75, load factor)이 되면, 해시 버킷의 개수를 두 배로 늘림. 

- [Java HashMap은 어떻게 동작하는가?](https://d2.naver.com/helloworld/831311)

<br/>
### Graph
#### graph : 정점과 간선의 집합  
  - 트리, 싸이클이 허용되지 않는 그래프

#### 그래프 관련 용어
  - Undirected Graph / Directed Graph(Digraph)
    - 정점과 간선 간 방향성 유무에 따라 구분
    - Undirected Graph

  - Degree
    - **Undirected Graph**에서 각 정점(vertex)에 연결된 Edge의 개수를 Degree라 함.
    - Directed graph에서는 간선의 방향이 존재하므로 Degree가 2개로 나뉨, 정점으로 **나가는 **간선의 수를 **outdegree**,
    들어오는 간선의 수를 **indegree**라 함.

  - 가중치 그래프(Weight Graph)
    - 간선에 가중치 정보를 두어 구성한 그래프

  - 부분 그래프(Sub Graph)
    - 부분 집합과 유사한 개념의 부분 그래프

#### 그래프 구현 방법
- 인접 행렬(adjacent matrix) : 정방 행렬을 사용
  - 해당 위치의 value값을 이용하여 연결관계를 상수시간에 파악할 수 있다.
  - V^2의 space complexity를 가진다.
  - [Dense graph](https://ko.wikipedia.org/wiki/%EB%B0%80%EC%A7%91_%EA%B7%B8%EB%9E%98%ED%94%84)에 적절(edge의 수가 최대 간선의 수에 가까운 그래프)
- 인접 리스트(adjacent list) : 연결 리스트를 사용
  - 확인코자 하는 vertax의 adjacent list를 확인해야 하므로 시간이 오래 걸림
  - Space Complexity : O(E+V)
  - Sparse graph에 적절

#### 그래프 탐색
그래프는 정점의 구성과 연결규칙이 존재하지 않아 탐색이 복잡하다.
- 깊이 우선 탐색(Depth First Search:DFS)
  - 연결할 수 있는 정점을 끝까지 쫒아간다, 더이상 연결된 정점이 없으면 바로 직전으로 돌아가며 반복한다.
  - Stack을 이용해 구현한다. 
  - Time Complexity : O(V+E) ... vertex 개수 + edge 개수

- 너비 우선 탐색(Breadth First Search:BFS)
  - 한 정점에 연결된 모든 정점들로 나아감
  - Queue를 이용
  - 탐색을 시작하는 정점을 Queue에 넣고, dequeue하며, 각 정점과 간선으로 연결된 정점들을 enqueue 반복
  - Time Complexity : O(V+E) ... vertext 개수 + edge 개수

#### Minimum Spanning Tree
그래프의 spanning tree(그래프의 모든 vertex가 **cycle**없이 연결) 중 edge weight 합이 최소인 것.

- Kruskal Algorithm
  - weight가 가장 작은 간선부터 검토
  - cycle형성 하지 않는 경우에만 간선 추가
  - spanning tree 완성 시까지 or 모든 간선 검토 시까지(spanning tree 불가 시)
  - cycle 생성 여부 판단?
    - 연결된 vertex들에 동일한 group-id 할당,
    - 연결하고자 하는 두 vertex의 group-id가 동일하면 cycle!!
  - 시간복잡도
    - O(ElogE)

- [Prime Algorithm](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A6%BC_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)
  - 그래프에서 하나의 꼭짓점을 선택하여 트리 생성
  - 그래프의 모든 변이 들어있는 집합 생성
  - 트리가 모든 꼭지점을 포함할 때 까지
    - 트리가 포함하는 꼭지점끼리의 연결이 아닌, 가중치가 가장 작은 변이 연결하는 변을 그래프에 추가
  - 시간복잡도
    - O(ElogV)