# 임베디드 게임 프로그램 - Embedded

'[길건너 친구들](https://www.crossyroad.com/)'에서 영감을 받아 ARM 프로세서 기반에 임베디드 리눅스에서 8x8 도트 매트릭스 상에 장애물과 유저를, LED에 신호등을, CLCD에 최고 점수를, FND에 현재 점수를 출력하고 택트 스위치로 조작하며 이루어지는 장애물 피하기 게임 프로그램을 개발했습니다.

![demo](https://raw.githubusercontent.com/hee000/portfolio/main/images/Embedded.png)

#### 규칙 설명

목적은 최대한 앞으로 전진하며 높은 점수를 획득하는 것입니다.  
이동의 경우 전후좌우 이동이 가능합니다.  
이동하는 경로에는 장애물이 설치되어 있으며 일부 장애물들과 접촉하게 되는 경우 게임이 끝나게 됩니다.  
플레이어의 이동과 무관하게 맵이 앞으로 이동하므로 오랜 시간 가만히 있어 맵의 뒷부분과 닿게 되어도 게임이 끝나게 됩니다.
플레이어는 신호등이 파란색, 노란색인 경우에만 이동이 가능하며 빨간색일 때 이동한다면 게임이 끝나게 됩니다.

#### 특징

- 도트 매트릭스

  게임이 이루어지는 공간으로 메인 로직과 그래픽 관련 로직을 분리해서 플레이어와 장애물들을 표현하는 구조체를 정의하고 이러한 구조체를 리스트에 담아서 보내기만 하면 각 요소들의 좌표를 계산하고 바이너리 값을 계산해서 8x8 매트릭스 디바이스 쓰기 작업을 통해서 실제로 불이 들어오도록 만들었습니다.

  ```cpp
  // cross.cpp
  typedef struct coord {
      int y;
      int x;
  } coord;

  typedef struct obstacle {
    bool isMoving;
    bool direction;
    vector<coord> crd;

  } obstacle;

  class Cross {
    coord currentYX;
    vector<obstacle> obstacles;

    ...

    coord get() { return currentYX; }
    vector<obstacle> getObstacle() { return obstacles; }
  }

  // main.cpp
  void print(int microSec) {
    vector2Matrix(s.getObstacle()); // Cross s
    DM.set(s.get());
    DM.drawToMatrix(microSec);
  }

  ```

  > 메인 로직에서 각각의 도트 매트릭스가 어떻게 켜지는지 몰라도 되도록 추상화했습니다.
