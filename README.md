# 자바 미니 프로젝트 : pong_game

## 참고 자료
- https://www.youtube.com/watch?v=oLirZqJFKPE
  <img width="596" alt="image" src="https://github.com/jongheonleee/pong_game/assets/87258372/ee7facad-e945-498f-bb60-71d8a6945fc2">
<br/>

## 애플리케이션 정의
- 해당 애플리케이션은 두 명의 사용자가 방향키로 탁구 게임을 하는 것으로 패달에 탁구공이 튕겨나올 때 속도가 점점 빨라지며 공을 못친 경우 상대방에게 점수가 올라가며 재시작함
</br>

## 애플리케이션 객체 역할 정의

### GameFrame
- 게임 윈도우임, 이 위에 Panel 객체를 올려서 게임이 진행되는 동안 이미지를 지속적으로 그림
- 게임 윈도우를 세팅해는 역할

### GamePanel
- 전체 게임 상황을 그려주는 역할을 함
- 해당 패널에 필요한 객체들을 모두 그려주고 게임을 진행시키는 역할을 함
- 컨트롤러임, 게임에 필요한 객체들을 생성해주면서 각각의 객체들이 지원하는 메서드를 게임에 상황에 맞게 적절히 호출하면서 게임을 진행시키는 역할
- 한마디로 말해서 게임 컨트롤러임

### pongGame
- GameFrame을 생성하면서 애플리케이션을 실행해주는 역할을 함


### Paddle
- 게임 윈도우 내에 존재하는 탁구체 객체
- 색상, 크기, 속도, id에 대한 정보를 갖고 있음
- 또한, 사용자의 방향키에 따라 이동해줌

### Ball
- 게임 윈도우 내에 존재하는 공 객체
- 방향, 속도, 색상에 대한 정보를 가지고 있음


### Score
- 현재 진행되는 게임의 스코어를 기록하는 역할을 함
- 게임 내에 사용자는 2명이기 땨문에 2명에 대해 값을 저장하는 변수와 게임 윈도우 크기를 갖고 있음
- 게임 윈도우 크기와 글씨의 크기를 고려하여 적절하게 정중앙에 스코어 배치



## 게임 동작 원리 분석


### 📌 1. 탁구 공이 튀기는 원리

</br>
<img width="95" alt="image" src="https://github.com/jongheonleee/pong_game/assets/87258372/e993a353-d0f4-4f15-9fc7-8b5f620f09b3">
</br>
<img width="1004" alt="image" src="https://github.com/jongheonleee/pong_game/assets/87258372/a36466eb-3066-4936-aabc-aaa6a2d88d30">
</br>

- 페달과 벽에 맞으면 공이 튀기면서 속도가 빨라짐

#### (1) Ball 객체 분석
- 일단, Ball 객체에는 공에 대한 정보가 저장되어 있는데 여기에는 속도((xVelocity, yVelocity)와 방향(randomXDirection, randomYDirection)에 대한 정보가 기록되어 있음가 있음
  </br>
  <img width="359" alt="image" src="https://github.com/jongheonleee/pong_game/assets/87258372/5c2fbabb-762a-4f8f-9448-6e73c8e3ec1a">
- 이때, 밑에 사진에서 보이는 것과 같이 초기 속도 정보가 곱해진 값이 방향을 나타내는 변수에 곱해져서 속도의 값을 세팅해줌
  <br/>
  <img width="699" alt="image" src="https://github.com/jongheonleee/pong_game/assets/87258372/d3f34eb3-f098-4b31-96c3-56320fe09b2a">
  <br/>
- 위의 메서드를 통해 x, y의 속도 정보가 결정되면 move()메서드를 통해서 해당 공의 x, y에 속도를 각각 더해줌
  <br/>
  <img width="185" alt="image" src="https://github.com/jongheonleee/pong_game/assets/87258372/383bff08-c52e-4da7-8583-4326bc0f1671">
- 위를 통해 알 수 있는 것은 Ball 객체는 해당 공의 속도와 방향, 그리고 위치 정보를 담고 있음, <b>그래서 페달과 벽에 공이 튀기는 겨서 속도가 빨라지는 작업은 다른 객체에 의해 이루어짐</b>

#### (2) GamePanel 객체 분석
- 해당 객체를 통해서 Ball이 페달이나 벽에 부딪힌 것을 판단하고 적절히 처리해줌(속도를 높임,,,)
- 이 객체는 컨트롤러임, 전체 게임 판에서 이루어지는 상황에 대해 적절히 컨트롤해줌
- 일단, 분석하고자 하는 공이 벽이나 페달에 부딪히는 부분부터 확인해보자
- 해당 작업을 처리하는 메서드는 밑에 메서드임
  <br/>
  <img width="490" alt="image" src="https://github.com/jongheonleee/pong_game/assets/87258372/ce157f90-a98b-4719-96ae-b123c5540fac">
  <br/>
  <img width="496" alt="image" src="https://github.com/jongheonleee/pong_game/assets/87258372/705a70fc-1109-48d1-8005-a1e0abe3c74e">
  <br/>
- 첫 번째 주석 부분을 보자 "bounce ball off top & bottom window edges", 즉 볼이 윈도우 너비의 가장 자리를 떨어져 나갈때 해당 볼 객체의 속도에 - 를 곱해주어서 반대방향으로 돌림
- 두 번째 주석 부분을 보자 "bounce ball off paddles" 즉, 공이 페달에 맞았을 때를 의미함
  - 일단, 알아두어야 하는 것이 x축은 가로축, y축은 세로축을 의미함
  - 페달에 부딪히면 x축의 속도의 크기를 증가시키고 y축의 속도도 증가시킴(이때, 음수이면 -1해주고 양수이면 +1 상대 속도 증가)
  - 마지막에 ball객체의 setXDirection(), setYDirection()을 통해서 해당 공의 속도 정보를 업데이트 시켜줌
    - paddle1은 왼쪽에 위치한 페달로 공이 튕겨져 나가면 x축 기준 -> 으로 이동하기 때문에 양수 세팅
    - paddle2는 오른쪽에 위치한 페달로 공이 튕겨져 나겨면 x축 기준 <- 으로 이동하기 때문에 음수 세팅

- 또한, 해당 메서드는 페달의 이동도 제한시킴 그래야지만 페달이 게임 윈도우 밖으로 안나감
  <br/>
  <img width="400" alt="image" src="https://github.com/jongheonleee/pong_game/assets/87258372/b085167c-c450-4201-9981-e088b366b243">
  <br/>

- 마지막으로, 이 메서드는 페달이 공을 못 받아치고 윈도우 밖으로 나가는 경우도 처리함
  <br/>
  <img width="472" alt="image" src="https://github.com/jongheonleee/pong_game/assets/87258372/8d2af9fd-f250-474a-8827-4179183039a8">
  <br/>
  - 공이 paddle1 뒤쪽으로 빠진 경우, 그러니깐 사용자1이 못 받아친 경우에는 사용자2의 점수가 올라가고 다시 패달과 공을 생성함
  - paddle2 뒤쪽으로 빠진 경우도 마찬가지로 처리해줌
  


### 📝 결론 : 


## 🛠 새롭게 배운것들

### 1. Rectangle


### 2. 사용자가 2명 이상일 때 keyPressed 구현방식과 keyReleased()

### 3. Runnable


