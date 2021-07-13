# Makefile



### 기본구조
```
target: depencencies ..
        commands ..
```
- 구성요소
  - target
    - 빌드 대상의 이름
  - dependencies
    - 의존 파일들, target 빌드 이전에 depencencies 에 정의된 대상들이 먼저 만들어진다.
  - commands
    - target 을 빌드하는 명령
- 일반적으로 위와 같은 구조의 반복 나열로 되어있다.
- 각 구조는 의존여부에 따라 특정 순서로 실행 될 수 있다.



## 동작 방식 이해를 위한 예제
- main.cpp, tools.cpp 두 개의 소스코드로 이루어진 util.exe 을 빌드하는 상황을 가정한다.



### make 없이 g++을 이용해 직접 빌드하는 경우
- main 과 tools 의 Object 파일을 각각 만들고, 두 Object 파일을 연결해서 util.exe 실행 파일을 만든다.

```
g++ -c -o tools.o tools.cpp
g++ -c -o main.o main.cpp 
g++ -o util.exe main.o tools.o
```



### 직접 빌드하는데 사용한 내용을 Makefile 양식에 맞게 서술하면 아래와 같이 된다.

Makefile
```
util.exe: main.o tools.o
  g++ -o util.exe main.o tools.o

main.o: main.cpp
  g++ -c -o main.o main.cpp

tools.o: tools.cpp
  g++ -c -o tools.o tools.cpp
```



### Makefile 에 서술된 내용을 변수 규칙을 사용해 축약한다.
- 이렇게 작성해두면 cpp 파일의 단순 추가 상황에는 Makefile 내용을 특별히 수정 할 필요 없다.

Makefile
```
CC = g++
TARGET = util.exe
SRC = $(wildcard *.cpp)
OBJ = $(SRC:.cpp=.o)

$(TARGET): $(OBJ)
  $(CC) -o $(TARGET) $(OBJ)
  
$(OBJ):
  $(CC) -c $(SRC)
```


