# Makefile




### 기본구조
```
target: depencencies ..
        commands ..
```
- target
  - 빌드 대상의 이름
- dependencies
  - 의존 파일들, target 빌드 이전에 depencencies 에 정의된 대상들이 먼저 만들어진다.
- commands
  - 최종적으로 target 을 빌드하는 명령

## 동작 방식 이해를 위한 예제
- main.cpp, tools.cpp 두 개의 소스코드로 이루어진 util.exe 을 빌드하는 상황을 가정한다.

### make 없이 직접 빌드하는 경우
```
g++ -c -o tools.o tools.cpp
g++ -c -o main.o main.cpp 
g++ -o util.exe main.o tools.o
```

### 직접 빌드하는데 사용한 내용을 Makefile 양식에 맞게 서술
Makefile
```
util.exe: main.o tools.o
  g++ -o util.exe main.o tools.o

main.o: main.cpp
  g++ -c -o main.o main.cpp

tools.o: tools.cpp
  g++ -c -o tools.o tools.cpp
```
빌드
```
make
```

### Makefile 에 서술된 내용을 변수를 사용해 축약
- cpp 파일이 추가되어도 Makefile 의 내용을 특별히 수정 할 필요 없다.
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
빌드
```

```
